---
title: Pixel FOM
author: Kennedy Caisley
date: 06.04.2023
---

# Pixel Detector Figure of Merit




## from RD53A Specs 2015:

"following discussions with ATLAS and CMS sensor developers, for RD53A we have assumed sensors have less than 100 fF per pixel and deliver a single pixel signal greater than 600 e− in at least one pixel for 99% of incident particles"


* 4uA per pixel in analog circuits
* <1 % hit loss from in pixel pile up
* noise occupancy per pixel < 10^-6, for 50fF load, in a 25ns interval
* <500mA/cm^2, so 1 W/cm^2 at 2V supply
* min theshold of 600e- and min in-time (<25ns timewalk) threshold of 1200e-.
* all these specs must be met at 500Mrad dose

signal to noise ratio: quantization noise and jitter/noise can be combined into one measurement

pixel pitch/area may also be able to be combined into this parameter too, as the spacial resolution of the pixels ultimately just allows for

## questions:

* are the values for threshold (i.e. 600e-) input referred?
* why is power limited to 0.5-1.0W per cm^2?



And equations: $f(x)=x^2$

Power:

* Pixel energy consumption inactive baseline when no in-pixel hit arrives

* Pixel energy consumption rise per hit (for a certain size, above threshold) actually doesn't matter, what matters is how this then dissipates into heat, which limits the pitch of the pixels. We don't want to conserve power, we just can't exceed our thermal limits.

Actually no, the power consumption should not simply be blindly reduced. Power consumption is a budget for performance! So assume a constant maximum power budget?, and simply optimize the best ways to spend this budget? Or perhaps increasing the decreasing power budget does actually improve performance, because mass matters?


```julia, results="hidden"
f = (x) -> x^2
```

Deadtime? Is there any need to have pixel dead times less than 25ns in the application of the LHC? 

Assuming each pixel produces data with the same number of output bits, we can therefore assume a fixed amount of area is necessary per pixel for data read-out, and we can abstract this

Should radiation hardness be a factor? Or should be simply just perform our measurements after a certain amount of radiation damage? I think the latter case is better.

Assume a standardized jitter for clocks available across the pixel matrix


## Jitter

'Understanding and Characterizing Timing Jitter - Primer by Tektronix'

* Jitter is short term variation w/ frequencies above 10 Hz, other it is called "wander"
* Must be characterized w/ statistics (mean, standard deviation $\sigma$, and confidence interval)
* High pass filter is useful for physical measurement, to cancel "wander"
* This can be a PLL, which is "nice" because it mimics a real systme
* Ideal sinusoid is ideal reference, most often (with same freq. and $\phi$), found via min $\Sigma(error)^2$

$$
A*sin(\omega_c*t+\phi_c)
$$

where $\omega_c$ and $\phi_c$ are constants chosen to minimize timer error of positions

Jitter can be measured in:

1. Periodic Jitter $J_p$ : Just a histogram of signal peridos of a persistent period, measured often in persistence mode (trigger on one edge/peak of waveform, and then measure 'width' on the subsequent edge/peak)

2. Cycle-to-Cycle $J_{c2c}$: First order difference of period jitter, which show dynamics from cycle-to-cycle, for a PLL, etc

$$
J_{c2c}=J_{P_{n+1}}-J_{P_{n}}
$$

3. Time-Interval Error (TIE): uses deviation from ideal reference. Difficult to observe directly with oscilloscope in experimental setup, but is good as it reveals cumulative effect of jitter

$$
TIE_n=\sum\limits_{0}^{n}(J_{p_{n}}-t_{ideal_{n}})
$$

where $n$ is the cumulative edge number, in time. The more cycles go by, the further we will likely find ourselves from the ideal

```julia
using Plots
x = range(0, 10, length=100)
y = sin.(x)
plot(x, y)
```
