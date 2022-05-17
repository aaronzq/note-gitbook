# Optics

## Some notes about incoherent imaging simulation

The diffraction model takes a complex wavefront and outputs another complex wavefront. For the input in our PSF calculation experiment, we set the mid of the FOV as 1 and the rest of pixels as 0. For the final image on the sensor, we take the square of the amplitude, i.e. the absolute value of the wavefront.

We call it incoherent imaging system. But we actually didn't introduce any incoherent factors. Is it correct?

This only works when we compute PSF, which is one single point in the object space, because a single point has no coherence concept: you can set any phase to it, and there's no second point to study their correlation. But this will not work (at least not work 'theoretically correctly') in multiple objects, such as the background.

Let's assume we have a system consisting of an Objective and a microlens array behind the Objective. How to simulate an uniform background? If we have a constant value (constant amplitude and 0 phase term) for the entire FOV, the frequency component of this constant value will be concentuated at the center of the aperture plane. But unfortunately, let's say we don't have a lenslet at the center aperture and therefore this component is completely lost after the microlens array.

This is not the case in real experiment. In incoherent light field imaging, the background will still pass the system, which indicates the input wavefront is not simply a plane wave with identical amplitude and phase. If they are illuminated by an incoherent light source, they should have uncorrelated phase component: we should generate random phase for each point at different position. Repeat this for hundreds times (the more, the better) and add them up to simulate incoherent imaging.

This is especially true in full field OCT-LIFT simulaiton. For the wavefront from reference arm, we should not simply set it to be a planar wave. In real system we use a wide-band LED light source. And for each position in the FOV, there are no correlation in the phase (while the intensity can be assumed to be uniform).

## Some notes about coherence

**Spatial coherent**: The extent of distance (usually talking about two points on one plane) that the wavefronts at two places are correlated. In most case, if we say the illumination is spatially coherent, the illumination light can be converged into an ideal point. For example, the light filtered by a pinhole can be a spatially-coherent light. Imaging system like scanning-based OCT (rather than full field OCT) and confocal microscope should use spatially-coherent light source.

p.s. To use a lens to focus a light bulb cannot generate spatially-coherent light because the smallest size of the focused light is larger than the image of the light bulb.

**Temporally coherent**: The extent of time (or propagation distance) that the wavefronts are correlated. This is the coherence related to the spectral width of light. White light has very low temporally coherence and thus short coherent length. Therefore it provides the Fourier OCT system the ability of optical sectioning.

## Dispersing in the materials

Lights with different frequency have different wavelength. This is true in vacuum and also in all kinds of materials. In vacuum, we have a constant light speed $c$.
$$
c=\lambda _0 f
$$
When the light with frequency $f$ enters a medium, the frequency stays the same. The refractive index $n$ of the medium is a material characteristic and a function of light frequency $f$. The speed and wavelength of light in this specific medium are then defined as:
$$
v_n = \frac{c}{n}
$$
$$
\lambda _n = \frac{\lambda _0}{n}
$$
$$
v_n = \lambda _n f
$$
We can emphasize that both light speed and wavelength are the function of the refractive index. Thus It's not always true that red light travels faster than violet light. Imagine a material that's dispersing-free, which means the refractive index is the same for all the light. Then red and violet light travels with the same speed. As for their wavelength:
$$
\lambda ^{red}_n = \lambda ^{red}_0 / n
$$
$$
\lambda ^{violet}_n = \lambda ^{violet}_0 / n
$$
Here the $\lambda _0$ is the wavelength in the vacuum. 

Another example can be: if we know the wavelength of the light in the medium, can we determine which light it is, i.e. what the wavelength it has in vacuum? 

No, since for same light, the wavelength will vary in different material. If we don't know the refractive index of this material, then we cannot identify the light. 

Although not stated before, it's commonsense that we refer to the vacuum environment when referring to the light with its wavelength. When we say light at 600 nm, we are talking about light with wavelength 600 nm in vacuum. And when we plot the refractive index of the material with respect to the wavelength (dispersing property of the material), we plot the wavelength in the vacuum. It won't make sense to plot against the wavelength in the specific material because the latter is actually determined by the refractive index itself. 

Also, it's dangerous to simply think that light with longer wavelenth travels faster in the medium. e.g. In the water, it holds true in the visible light spectrum: We see a descending refractive index when the wavelenth increases. But when light approaches longer wavelenth, it becomes the contrary where light with longer wavelength meets larger refractive index and thus travels slower.

[The refractive index of water](https://refractiveindex.info/?shelf=main&book=H2O&page=Hale)

In conclusion, wave number $k=2\pi /\lambda$, wavelength $\lambda $, light speed $v$ in the medium are dependent to refractive index. The refractive index is a function to the light frequency, or wavelength in the vacuum. 


