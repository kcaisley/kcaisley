'Understanding and Characterizing Timing Jitter' - Primer by Tektronix

* Jitter is short term variation w/ frequencies above 10 Hz, other it is called "wander"
* Must be characterized w/ statistics (mean, standard deviation $\sigma$, and confidence interval)
* High pass filter is useful for physical measurement, to cancel "wander"
* This can be a PLL, which is "nice" because it mimics a real system
* Ideal sinusoid is ideal reference, most often (with same freq. and $\phi$), found via min $\Sigma(error)^2$

$$
A*sin(\omega_c*t+\phi_c)
$$

where $\omega_c$ and $\phi_c$ are constants chosen to minimize timer error of positions

Jitter can be measured in:

1. Periodic Jitter $J_p$ : Just a histogram of signal periods of a persistent period, measured often in persistence mode (trigger on one edge/peak of waveform, and then measure 'width' on the subsequent edge/peak)

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