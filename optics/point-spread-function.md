# Point Spread Fucntion and Resolution
- When a high-contrast point target is imaged, the point appears as a blurred blob in the image because any practical image system is imperfect
- The blurred blob of the geometrically perfect point (delta impluse) is PSF (inpmulse function)
- The full width at half maximum (FWHM) of the PSF is often defined as the spatial resolution

## Line spread function (LSF)
What we get if imaging a perfect line. Basically it's a summation of PSF at all points at the line
$$
LSF(x,y) = \int \int Line(x',y') PSF(x-x',y-y')dx'dy' 
$$
$Line(x,y) = 0$, when $x != 0$, and we only need $LSF(x,y)$ when $y=0$.
$$
LSF(x) = \int_{-inf}^{+inf} PSF(x,-y')dy'
$$
It doesn't matter if we flip the PSF if the integral range is -inf to +inf
$$
LSF(x) = \int_{-inf}^{+inf} PSF(x,y)dy
$$
## Edge spread function (ESF)
An edge is, for example, an image with left side being black and right side being white. It can be regarded as a lot of lines at the white side.
$$
ESF(x) = \int_0^{+inf} LSF(x-x')dx'
$$
let $x'' = x-x'$
$$
ESF(x)=\int_{-inf}^{x} LSF(x'')dx''
$$
$$
LSF(x) = \frac{d}{dx}ESF(x)
$$

## Optical transfer function (OTF)
$H(u, v) = \mathscr{F}\{PSF(x,y)\}$ is a complex that describe the system's characteristics in fourier domain. The amplitude of it is called MTF (modulation transfer function). 
$$
MTF = |H(u,v)|
$$
MTF is often 1D plotted to show the general system response with respect to frequency after integral in one axis. Or it can be alternatively computed by Fourier transformation of Line Spread Function (LSF). Ideal system has MTF as constant 1 for entire spectrum. We also use a diffraction limited MTF as a reference in optic design.

## Contrast, Signal noise ratio (SNR), Contrast Noise Ratio (CNR)
$$
Contrast = \frac{dI}{I}
$$
$I$ is the overall signal level, which consists of signal and background and noise. $dI$ is the signal. 
$$
SNR = I/\sigma_n
$$
$\sigma_n$ is the standard deviation of the sigmal, which is taken as the noise.
$$
CNR = dI/\sigma_n = SNR * Contrast
$$

## Depth-Resolution-Ratio

$$
DRR = Depth / Axial Resolution
$$

# Ballistic imaging
Ideally, ballistic imaging is based on __unscattered__ or __singly back-sacttered__ ballistic photons. But this portion of photon is very small. So in reality, more-scattered __quasi-ballisitc__ photons are also measured to increase the signal intensity. __Quasi-ballisitc__ photons are those who have scattered but are still able to back-traced to a small region. When we talk about ballistic imaging, we normally mean quasi-ballistic imaging. 
We usually think the ballistic imaging happens within the transport mean free path of the tissue. 

The transport extinction coefficient is
$$
\hat \mu_e = \mu_a + \hat \mu_s = \mu_a + (1-g)\mu_s  
$$
$g=1$ for Mie scattering (forward scattering, when particle is at wavelength scale), and $g=0$ for Rayleigh scattering (isotropic scattering, when particle is smaller than wavelength). 
The transport mean free path is the reciporal of transport extinction coefficient, which quantifies the traveling distance of photon when the statistical intensity drops down to 1/e.
$$
\hat l_t = 1/\hat \mu_e 
$$
This gives us a "soft limit" distance, usually at several mm, and this is what we usually refer as the range of ballistic imaging. There's another "hard limit" from the penetration depth, which tells us that below this limit, there's hardly any available photons at all.

## Time-gated Early-photon Imaging
Because non-ballistic photons have larger optic path length when arriving at detection plane. We can use time-gating to select those with shorter optic path length.
- Raster scan to form a projection image
- 1 fs gated window -> select those photons travelling 0.3 microns in the medium (optic path length)
- 33 fs -> 10 microns

### Device for time gate
1. Kerr effect. Use a pair of orthogonal polarizers and a half waveplate (rotation 45 degree) controlled by a auxiliary beam. Without the beam, it's transparent. With the beam, it functions as waveplate (birefringent). Therefore we can control the photon pass/no pass through the auxiliary beam

    Speed~100 fs

2. Strike camera. Use scanning electric field to relocate photonelectrons according to their arrival time

    Speed~200 fs

## Spatial-frequency Filtered (Fourier Space Gated) Imaging
Since non-ballistic photons have high angular component, we can use spatial filter (pinhole, pinhole between 4f system) to filter out them. 

## Polarization-difference Imaging
Assumption:
1. Ballistic component is 100% polarized.
2. Nonballistic component is 100% unpolarized.
3. Nonbirefringent medium

We can have a pair of polarizers and place the scattering medium in between. First we align two polaizers, we will measure
$$
I_1 = I_B+0.5*I_{NB}
$$

$$
I_2 = 0+0.5*I_{NB}
$$

$$
I = I_1-I_2
$$



