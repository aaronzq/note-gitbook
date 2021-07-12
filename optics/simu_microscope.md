# [Simulation] A microscope's PSF

I start from simple model, a microscope consisting of one infinity-corrected Objective and one tube lens, to validate the accuracy of the numerical model. In microscope usage, sample is placed at the fore-focal plane of the Objective. An "infinity space" of parallel light is created between the Objective and the tube lens, which brings the advantages over the non-corrected Objectives such as the flexibility of installing filters, modulators in the middle. A tube lens is designed for the Objectives of each specific manufactuer. It collects the light and forms the image at the rear-focal plane. The pupil distance is the optimal distance between the tube lens and the entrance pupil of the objective, which is a range. But here I just treat it as a 4f system where the rear-focal plane of the Objective coincides with the fore-focal plane of the tube lens. 
![microscope_schematic](/assets/microscope_schematic.jpg)

## Some theories
### Fresnel diffraction
Fresnel formula describes the diffraction that happens in near field of the aperture. It's relatively accurate if Fresnel number $F = \frac{w^2}{z\lambda}$ is around 1~10.
$$E_2 (x_2 ,y_2 )=\frac{exp(ikz_1 )}{i\lambda z_1}\int \int E_1 (x_1 , y_1 )exp( \frac{ik}{2z_1} ((x_2 -x_1 )^2 +(y_2 -y_1 )^2) ) dx_1 dx_2 $$
$$ = \frac{exp(ikz_1 )}{i\lambda z_1}exp(\frac{ik}{2z_1}(x_2^2+y_2^2)\mathscr{F}(   E_1(x_1,y_1) exp(\frac{ik}{2z_1}(x_1^2+y_1^2)))_{u=\frac{x_2 }{\lambda z_1},v=\frac{y_2 }{\lambda z_1}}$$

Due to the existence of $exp(\frac{ik}{2z_1}(x_1^2+y_1^2)$ in the fourier transform, the diffraction pattern is dependent on the propogation distance. There are ways to numerically compute the pattern:

1. Dumb calculation by hand-written integral using the formula
```
scale = 16;
dx2 = lambda*fmla/dx1/fovN*scale;
x2space = dx2 * [ ceil(-fovN/scale/2):ceil(fovN/scale/2)-1 ];
y2space = x2space;
E2 = zeros(1, fovN/scale*fovN/scale);
parfor g = 1:fovN/scale*fovN/scale
    tic;
    xx2 = x2space(1, mod( (g-1), (fovN/scale) ) + 1 );
    yy2 = y2space(1, ceil( g / (fovN/scale) ) );
    Koi = exp(1j*k*fmla)/(1j*lambda*fmla) * exp(1j*k/2/fmla*(xx2^2+yy2^2));
    for aa = 1 : fovN
        for bb = 1 : fovN
            xx1 = x1space(aa);
            yy1 = y1space(bb);
            integrand = E1(bb,aa) * exp(1j*k/2/fmla*(xx1^2+yy1^2)) * exp(-1j*k/fmla*(xx2*xx1+yy2*yy1)) * dx1 * dx1;                
            E2(g) = E2(g) + integrand*Koi;
        end 
    end
    toc
end
E22 = reshape(E2', [fovN/scale, fovN/scale]);
E22 = E22';
```
Very time comsuming when image resolution is high (Time consumption is acceptable ). But there is no limitation from sampling issue. In another word, it doesn't affect the accuracy if we use a small sampling number, in contrast to following FFT-based methods.

2. Fresnel Transfer Function (TF) Propagator

$$
E_2 (x,y) = \mathscr{F}^{-1}(\mathscr{F}( E_1(x,y) H(f_x, f_y)   )   )
$$
$$
H(f_x,f_y) = exp(ikz)exp(-i\pi\lambda z(f_x^2 + f_y^2))
$$
**The convolution of two functions can be computed by the inverse fourier transformation of the multiplication of their fourier transformations respectively.**
Discrete sampling of the source field, sampling of the transfer function, and the periodic nature of the FFT can lead to a variety of artifacts in the propagation result. In order to suppress artifacts:
- The simulation array size should be larger than the source field, i.e. the aperture;
- $ dx >= \frac{\lambda z}{L}$, $L$ is the length of the array. This ensure the adequate sampling of the transfer function H.
**Note that the result is the same whether a single TF propagation or a sequence of shorter TF propagations is used. This is because a succession of TF propagations is the same as applying the product of the transfer functions to the initial field.**

```
function [f2, dx2, x2] = fresnelTF2d(f1, dx1, z, lambda)
% assuming dx1=dy1, Nx=Ny

k = 2*pi/lambda;
[~, N] = size(f1);

% determine if it oversamples
fprintf("dx: %f should be larger than lambda*z/L: %f; Adequately sampling: %d\n", dx1, lambda*z/N/dx1, dx1>lambda*z/N/dx1);

du = 1/(N*dx1);
u = du*[ ceil(-N/2):ceil(N/2)-1 ];
[U, V] = meshgrid(u, u);

H = exp(-1j*pi*lambda*z*(U.^2+V.^2));
H = fftshift(H);
% f2 = exp(1j*k*z)*ifft2( fft2(f1) .* H );
f2 = exp(1j*k*z)*ifftshift( ifft2( fft2(fftshift(f1)) .* H ) );

dx2 = dx1;
x2 = dx2*[ ceil(-N/2):ceil(N/2)-1 ];

end
```

3. Fresnel Impulse Response (IR) Propagator
$$
E_2 (x,y) = \mathscr{F}^{-1}(\mathscr{F}( E_1(x,y) \mathscr{F}(h(x,y))   )   )
$$
$$
h(x,y)=\frac{exp(ikz)}{i\lambda z}exp(\frac{ik}{2z}(x^2+y^2))
$$

Similar to FT Propagator, in order to suppress artifacts:
- The simulation array size should be larger than the source field, i.e. the aperture;
- $ dx <= \frac{\lambda z}{L}$, $L$ is the length of the array. This ensure the adequate sampling of the impulse function.

**Note that the second creterion is exactly the opposite to FT Propagator, which indicates compared to FT Propagator, IR Propagator is more suitable in long distace Fresnel propagation.**


```
function [f2, dx2, x2] = fresnelIR2d(f1, dx1, z, lambda)
% assuming dx1=dy1, Nx=Ny

k = 2*pi/lambda;
[~, N] = size(f1);

% determine if it oversamples
fprintf("dx: %f; lambda*z/L: %f; Adequately sampling: %d\n", dx1, lambda*z/N/dx1, dx1<lambda*z/N/dx1);

x1 = dx1 * [ceil(-N/2):ceil(N/2)-1];
[X, Y] = meshgrid(x1, x1);

h = 1/(1j*lambda*z)*exp(1j*k/(2*z)*(X.^2+Y.^2));
H = fft2(fftshift(h))*dx1*dx1;
U1=fft2(fftshift(f1));
U2=H.*U1;
f2=ifftshift(ifft2(U2));

dx2 = dx1;
x2 = dx2*[ ceil(-N/2):ceil(N/2)-1 ];

end
```

### Fraunhofer diffraction
Fraunhofer formula describes the diffraction that happens in far field of the aperture. It's relatively accurate if Fresnel number $F = \frac{w^2}{z\lambda}$ is << 1.
$$E_2 (x_2 ,y_2 ) = \frac{exp(ikz_1 )}{i\lambda z_1}exp(\frac{ik}{2z_1}(x_2^2+y_2^2)\mathscr{F}(   E_1(x_1,y_1) exp(\frac{ik}{2z_1}))_{u=\frac{x_2 }{\lambda z_1},v=\frac{y_2 }{\lambda z_1}}$$

1. Fourier transform

```
function [f2, dx2, x2] = fraunhofer2d(f1, dx1, z, lambda)
% assuming dx1=dy1, Nx=Ny

k = 2*pi/lambda;
[~, N] = size(f1);

% determine if it oversamples
fprintf("dx: %f should be larger than lambda*z/L: %f; Adequately sampling: %d\n", dx1, lambda*z/N/dx1, dx1>lambda*z/N/dx1);

dx2 = lambda*z/dx1/N;
x2 = dx2*[ ceil(-N/2):ceil(N/2)-1 ];
[X2, Y2] = meshgrid(x2, x2);

c = exp(1j*k*z) * exp(1j*k/2/z*(X2.^2+Y2.^2)) / (1j*lambda*z);
f2 = c .* ifftshift(fft2(fftshift(f1))) * dx1^2;

end
```

### Wave propagation through a single Lens 

### Debye integral

### Some notes about FFT in MATLAB

Take extra care of FFT and iFFT in MATLAB because their coordinate system is defined as following:
*For a 2D N x N matrix, on each axis, (0, N/2) is the positive half axis and (N/2+1, N) is the negative half axis.*

To compute the frequency domain of a 2D image: `img_fft = fft2( fftshift(img));`
To compute the frequency domain of a 2D image and visualize it in original image coordinate: `img_fft = ifftshift(fft2( fftshift(img)));`
To compute the space domain of an un-shifted result of the 2D image fft: `img_2 = ifft2(fft2( fftshift(img)));`
To compute the space domain of an un-shifted result of the 2D image fft and visualize it in original image coordinate: `img_2 = ifftshift(ifft2(fft2(fftshift(img)));`
To compute the space domain of a shifted result of the 2D image fft: `img_2 = ifft2(fftshift(ifftshift(fft2( fftshift(img)))));`
To compute the space domain of a shifted result of the 2D image fft and visualize it in original image coordinate: `img_2 = ifftshift(ifft2(fftshift(ifftshift(fft2(fftshift(img))))));`
## Compute the PSF of the microscope

### Debye integral (gold standard)

### Using lens model to compute from focal plane to focal plane

### Sequence of Fresnel propagations and lens pupil function
