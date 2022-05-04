# Richardson-Lucy Deconvolution

### What is RL-Deconvolution

Optic imaging system can be analyzed as a linear system described by its impulse function, the point spread function (PSF). Due to diffraction limit and optic aberration, we measure degraded image from the sensor. If we know the PSF as a prior, RL-Deconvolution helps us solve the reverse problem and restore the 'perfect' image.

If the system can be modeled as

$$E = Hx$$

where 