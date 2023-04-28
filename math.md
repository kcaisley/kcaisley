The Mathematics of Circuit Analysis

These videos are primarily targeted at electrical engineerinng students and professionals. When I graduated from my undergraduate program I had been exposed to all the concepts I'll be covering in this video

-When should I be using a fourier transform vs a laplace transform in the analysis of a circuit?
-What is the fundemental difference between the equations that arise from circuit with and without non-liner devices?

If any of those


The purpose of this video series is to develop an understanding of the mathematical techniques used in circuit analysis. This video will not be explicitly analyzing the design or performance of specific circuits.

Given a circuit, the objective of circuit analysis is to find the approximate functions for the voltage and current waveforms/signals. Depending on the complexity of the circuit, the devices present in it, and the tools available to us (pen/paper vs computer), we will see that varying levels of detail in our analysis are possible. After watching these videos, you should be to identify what analysis techniques are necessary/available, by visual inspection of the circuit's elements/configuration.

1) Our first task is to appreciate the limitations of circuit theory. It is important to understand that circuit theory is an approximation of maxwell's equations, called the 'lumped circuit discipline' is only valid with these assumptions, compare an electromagnetics simulation to the circuit analysis. In general, the smaller the dimensions of your physical devices, and the higher frequency your exciting signal, the worse our lumped circuit model will perform. At a certain theshold, typically in the single digit GHz region for circuit board level designs

This of course depends on your tolerance for error. The more accuracy you demand, the sooner you'll be making that switch 

Electromagnetics is perhaps the best defined/understood fundemental force. Without this appoximation, however, we would never be able to compute. Explain how ideal circuit schematics differ from those meant for for fabrication

2) Linear circuit devices, give rise to directly linear equations. In this model the voltage and current in the circuit instantaneusly would change with the forcing function. In order for this model to be valid, you must keep the forcing function constant or changing slowly. Show how you can solve this on paper with hand solutions (DC opt as well as transiet, if the input is definable by  and also how you can use numerical solutions. Then show how to place in

3) Introduce non-linear devices. Basic square laws. Equations are still quite simple. Yet this still yields incredibly complicated simulations. The main challenge in this case is that the operation of the transistor, across all terminal voltages can not be defined by a single equation. This peicewise definition causes our

4) Differential circuits with energy storage (inductors and capacitors)

Explain that you need to keep rising edges slow for all of this to be valid.

If we start speeding up this circuit with transistors, we start to see non Non-linear differential equations. 

Show a transistion







There exists a relationship between


Differential Equations
Exponentials		Natural Log	Comple
Laplace transforms
Fourier Transforms	cos and sin
Eigen Functions
Linear Systems of Equations
Eigenvectors and Eigevalues
Linear Algebra


Bui  Linear Combination of Complex Time Exponentials

Holomorphic Functions
Cauchy Riemann @ Differentiability

Notes from Major Prep's Laplace transforms
Laplace transforms are comprised of slices 2D fourier transforms at varying alpha values
Laplace transforms have a region of convergence

If poles are on the imaginary access, then the original function has only sinusoidals
	The values equate to the frequecies of the sinusoidals
	Symmetric about the 
If poles are on the real access, then the 

If you start manipulating the damping and oscilating coefficients in the differential equation, you will be strengethening and weakening the eponential and sinusoidal terms in the solution (system impulse response.) The movement of these poles, to optimise something (rise/fall times, minimize oscillations) I think is the basis of the Root locus plots.

So then what is a dominant pole?

Holomorphic Functions
Cauchy Riemann @ Differentiability








