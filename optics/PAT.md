# Photoacoustic tomography (PAT)

The object is irradiated by a short-pulsed laser beam. An intensity-modulated laser is an alterantive. 

Light absorbed by the object is partially converted into heat, in most cases with a fraction of approximately (1- fluorescence quantum yield)

Heat expansion generates acoustic wave.

I light -> heat by optical absorption ($\mu _a$) -> mechanical energy -> pressure wave

PA imaging combines optical excitation with acoustic detection, and holds the following benefits:

1. Deep penetration because the detection is based on ultrasound rather than photons (longer mean free path)
2. optical contrast. Signal is related to $\mu _a$, the absorption. For example, we can use PAT to measure light absorption of hemoglobin, then quantify the oxygen level in blood. But it's difficult to use ultrasound to generate that contrast. Ultrasound imaging's contrast comes from the scatter of the acoustic wave. 

Thermal relaxation time 
$$
t = \frac{d_c^2}{a_{th}}
$$
if duration of excitation is shorter than t, we say the excitation is "thermal confined"


Stress relaxation time
$$
t_s = \frac{d_c}{v_s}
$$

If the laserpulse width 

both stress and thermal confinemend

t0: pulse laser

absorption happens instantly, the temporature rises instantly

pressure builds up instantly

due to pressure amd thermal confined, we dont expect the temporalture and pressure to reduce in the duration of laser pulse


## Photoacoustic computed tomography

In optic imaging, we refer to 3D case when discussing tomography.

1. Laser pulse (< ANSI limit e.g. 20mJ/cm2)
2. Light absorption & heating (~mK)

1 mK -> 8 mbar = 800 Pa

3. Ultrasonic emission (~mbar)

4. Ultrasonic detection: an array of transducer or a scanning single transducer

90% PA targets on the blood. Hemoglobins as photon absorber, exist more in vessels compared to other tissues. Note that this absorption decreases in infrared light band due to hemoglobin's optical property.

acoustic wave: higher center frequency, shallower imaging depth, higher spatial resolution.

## Photoacoustic microscopy (PAM)


## Optical Resolution Photoacoustic Microscopy
Confine the laser illumination to single point and perform rasterized scanning (laterally). So the imaging depth advantage is lost due to illumination scattering. But it supports label free imaging.

Utilizing the relative slow traveling speed of acoustic wave, for every scanning point, we capture the A-mode signals, which tells us the signal at entire axial line.

## Photoacoustic nanoscopy
Non-linear absorption. Using curve fitting to find signals that has non-linear response to input photon energy. (Here we should input different intensity pulse and take a couple measurement). Those signals will mostly come from the center part of illumination.

