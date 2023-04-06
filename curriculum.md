The ideal pixel detector signal processing chain:

Measures the energy (ignoring timing and spatial resolution at this point)
Zero power, zero mass, instant response, digital w/ infinite resolution, zero error, perfect precision.
Also, we would like to understand the interplay between pixel dimensions, spatial energy error, and timing energy error. This gets complicated fast.

In it's most basic sense, we have signals with a continuous time and amplitude. We want to get a 

Basic Building Blocks:

Digitization: ADCs, TDCs, do this, but:
1) The don't have infinite precision
2) They don't have instance response
3) They can't read

they don't have infinite dynamic range, and so we need to rescale the signal to be in this range

Let's first simply consider our signal as a single dimensional signal in a continuous amplitude. Even here, we have issues of gain/linearity/dynamic range/distortion. Our initial detector has a transfer function, and the output of it is not in the range of a normal ADC, as so we need to apply gain. Also, our signal would go right from energy into digital bits, but sensors convert to charge, and so we need to work from charge to voltage, and then finally to digital bits. Noise also exists, but is is possible to consider this without considering time? Yes, I think it is. We can just say that on a measurement, there is a noise modeled by a random error which is sampled. We need to digitize this, to make it userful, and so now we face the issues of quatization error (which is a precision error). We will introduce the concept of decibeels, both in terms of gain, and in terms of noise. Gain: one wait to implement this is with a classic op-amp in feedback, but this isn't the only way. To avoid the intractible mathematics involved with non-linearity and distortion, we have to assume our signal is falling within the dynamic range.

Next let's say we also want to measure timing precision. In fact, even is we don't, we would have to acknowledge that our signal is actually two dimensional. This requires we consider the issues of jitter (timing noise), and bandwidth, and frequency response of a signal. The frequency response component is core to understanding this two dimensional problem. We must consider the issues of triggering, and realize that our signal is an inherintely two dimensional signal. The interaction of gain and time creates errors like time walk. Our initial charge signal is no instant. It's delivered by a current signal overtime, which based on sensor is converted to a voltage signal. But the placement of a feedback amplifier actually modifies the characteristics of this node, and so we have a complicated interaction. The limited bandwidth of components means that the gain available is diffferent at different speeds. Noise is also two dimensional, though, which can benefit us thorugh Filtering: Necessary because sometimes there are parts of the signal we don't want, or because there is noise, and it exists in domains of the frequency response that make it beneficial to be selective. It's in this stage that we really like to model our systems as linear and time invariant. This allows us to consider both the amplitude and time domain, with out the problem becoming mathematically intractable. Are Bode plots, transfer functions, fourier transfers, and laplace transforms something that exist in the undergraduate curriculm at Uni-Bonn? Shannon-Nyquit sampling theory may be able to be skipped in this discussion, as 


Once you add in time domain, however, things get complicated, because there are three fundamental components of noise: The shot noise, the thermal noise, and the 1/f noise. The pulse shaping filter, which has a high and a low frequency cutoff, but which should mainly be thought of as acting in the time domain to create a nice pulse with a return to baseline, has a different effect on each of these noises. If I remember correctly, thermal noise is not effected at all, and shot noise is reduced by pulse shaping, but 1/f noise is actually made worse by standard pulse shaping schemes. (But I may have flipped flopped the terms, so be sure to double check this.)

Next add in the question of spatial resolution. This requires that we acknowledge the challenges of power and space. Pixel detectors are physical devices that take up space.

Finally, let's dicuss implementation, and how analog and digital components can be instantiated by many different technologies including custom ASICs, board level components, and even FPGAs.
