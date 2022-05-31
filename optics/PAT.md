# Photoacoustic tomography (PAT)
 
## Imaging based on the photoacoustic effect

1. The object is irradiated by a short-pulsed laser beam. An intensity-modulated laser is an alterantive. But it has to have a varying intensity to generate acoustic wave. (which will be later explained)

2. Light absorbed by the object is partially converted into heat, in most cases with a fraction of approximately $1-\mu$, where $\mu $ is the fluorescence quantum yield.

3. The heat is further converted to a pressure rise via thermo-elastic expansion.

4. The pressure rise propagates as an ultrasonic wave, i.e. photoacoustic wave, and is detected by ultrasonic transducer

_Briefly: light -> heat by optical absorption ($\mu _a$) -> mechanical energy -> pressure wave -> detection_

## Benefits
The fundamental challenges in high-resolution optical imaging are:
1. Diffraction (wave phenomenon): limits the spatial resolution of ballistic imaging;
2. Diffusion (scattering phenomenon): limits the penetration of ballistic imaging.

PA imaging combines optical excitation with acoustic detection, and holds the following benefits:

1. Deep penetration because the detection is based on ultrasound rather than photons (longer mean free path)
2. Optical contrast. Signal is related to $\mu _a$, the absorption. Label free.

For example, we can use PAT to measure light absorption of hemoglobin, then quantify the oxygen level in blood. But it's difficult to use solely ultrasound to generate that contrast. Ultrasound imaging's contrast comes from the scatter of the acoustic wave. 

## Derivation
### Laser Pulse Width
Thermal relaxation time 
$$
t_{th} = \frac{d_c^2}{a_{th}}
$$
where $\alpha _{th}$ is the thermal diffusivity (e.g. for soft tissue: 0.13 mm^2/s) and $d_c$ is the characteristic length (targeted resolution).
e.g. if $d_c=15 \mu m$, then $t=1.7 ms$.

Stress relaxation time
$$
t_s = \frac{d_c}{v_s}
$$
where $v_s$ is the speed of sound (~1500 m/s=1.5mm/um=1.5um/ns). e.g. if $d_c=15 \mu m$, then $t=10 ns$.

__If the laser pulse width $t_l < t_s << t_{th}$, the excitation is said to be in stress and thermal confinements.__

### Initial Photoacoustic Pressure vs Temperature under Stress and Thermal Confinement Conditions
Fractional change in vol: 
$$
dV/V = -\kappa p + \beta T = - [Compressibility] [Pressure] + [Expansion coeff.][Temperature]
$$
Initial pressure induced by a short pulse: 

$$
p_0 = \frac{\beta} {\kappa }T = \frac{\beta} {\kappa } \frac{\eta _{th} A_e}{\rho C_V} = \Gamma \eta _{th} A_e
$$
where Mass density $\rho ~ 1000 kg/m^3$; Heat capacity $C_V ~ 4000 J/{kg K}$; $A_e$: specific optical absorption (energy deposition) in $J/m^3$

absorption happens instantly, the temporature rises instantly

pressure builds up instantly

due to pressure amd thermal confined, we dont expect the temporalture and pressure to reduce in the duration of laser pulse

### Photoacoustic Wave Equation in Thermal Confinement

$$
(\nabla ^2 - \frac{1}{v_s^2}\frac{\partial ^2}{\partial t^2} ) p(r,t) = -\frac{\beta }{C_p} \frac{\partial H}{\partial t}
$$
where Heating function: $H = \eta _{th}A_p = \eta _{th} \mu _a \Phi $, and $\Phi $ is optical fluence rate.

Therefore, it has to be pulse laser or intensity-varying laser $\frac{\partial H}{\partial t}$ to input energy to the photoacoustic wave, i.e. boundary condition at intial time point. 


## Photoacoustic Computed Tomography

In optic imaging, we refer to 3D case when discussing tomography.

1. Laser pulse (< ANSI limit e.g. 20mJ/cm2)
2. Light absorption & heating (~mK)

1 mK -> 8 mbar = 800 Pa

3. Ultrasonic emission (~mbar)

4. Ultrasonic detection: an array of transducer or a scanning single transducer

90% PA targets on the blood. Hemoglobins as photon absorber, exist more in vessels compared to other tissues. Note that this absorption decreases in infrared light band due to hemoglobin's optical property.

acoustic wave: higher center frequency, shallower imaging depth, higher spatial resolution.

## Photoacoustic Microscopy (PAM)


## Optical Resolution Photoacoustic Microscopy
Confine the laser illumination to single point and perform rasterized scanning (laterally). So the imaging depth advantage is lost due to illumination scattering. But it supports label free imaging.

Utilizing the relative slow traveling speed of acoustic wave, for every scanning point, we capture the A-mode signals, which tells us the signal at entire axial line.

## Photoacoustic Nanoscopy
Non-linear absorption. Using curve fitting to find signals that has non-linear response to input photon energy. (Here we should input different intensity pulse and take a couple measurement). Those signals will mostly come from the center part of illumination.

