# INTRODUCTION
This document covers the proper method of selecting compensation capacitors for the implementation of functional Photodiode Transimpedance Amplifiers.
## A short note on compensation
The basic circuit of a transimpedance amplifier is centered around an operational amplifier, with a feedback network consisting of a singular high-valued, high precision resistor. This circuit, however, suffers from stability issues related to the inherent capacitance
of the photodiode. Together with the feedback resistor, this input capacitance forms a zero in the transfer function of the Noise Gain. If left uncompensated, the Rate of Closure at the intersection between the Noie Gain and the Loop Gain will be 40dB / decade, insuring ony a marginal stability.

To achieve better performance, compensation is mandatory. A feedback capacitor is added across the gain resistor, thereby introducing a pole in the Noise Gain transfer function. This pole cancels out the 20dB/dec rise of the Noise Gain, introducing a stable plateau of 0dB/dec. As such, at the 
gain-crossing frequency, the ROC resolves to 20dB / decade, proper stability is achieved.

## Considerations
There are fout main considerations when building out the compensated TIA pictured in this doc. :
- Bandwidth: Feedback Capacitor CF degrades the bandwidth of the TIA
- Op-amp GBP: High-bandwidth TIA's neccessitate op-amp with large GBPs
- Stray Capacitances: Any stray capacitance across the gain resitor alters the transfer function of the circuit
- Q-factor: system's Q-factor influences Bandwidth and transient response

## Theory
The compensated TIA's transfer function is that of a second-order Low-pass filter, of the form $T(s) = \frac{A_{DC}\cdot \omega_{0}^{2}}{s^{2}+\frac{\omega_{0}}{Q}\cdot s + \omega_{0}^{2}}$, where $w_{0}$ is the natural frequency and the ratio $\frac{\omega_{0}}{Q}$ is equal to $2\cdot\zeta\cdot\omega_{0}$
and is called the Bandwidth. 

This circuit presents two poles, one at $f_{p1}=\frac{1}{2\pi\cdot R_{F}C_{F}}$ and one at $f_{p2}=\frac{GBP\cdot C_{F}}{2\pi\cdot(C_{I}+C_{F})}$. The first pole acts as the dominant pole of the system, having the greater impact on limiting the Bandwidth of the circuit. $C_{I}$ is the total
input capacitance (photodiode capacitance and parasitic op-amp input capacitance), while $C_{F}$ is the chosen value of the compensation capacitor.

Of great importance to our design workflow, the Q-factor has several values of great interest to us. Any second order system can be underdamped, overdamped or criticaly damped. Q-factor has great impact on rise-time ($t_{r}$) and overshoot ($PO$). We would like to have a signal with no overshoot
and no rise-time. This is, however, impossible, a compromise is desirable. 

-  Underdamped sytems have short rise-times, but large overshoots and settling times
-  Overdamped sytem have small overhoots, but long rise-times
-  Critically damped systems have short rise-times and no overshoots

A system is considered critically damped if the Q-factor is 0.5.

Other useful Q-factors are Q = 1 and Q = $\frac{\sqrt(2)}{2}$, with the second one being the Q-factor for which the transfer function attains a maximally flat frequency response.

## Design Process
Given the known variables and constraints:
- $C_{I}$
- $A_{DC}$
- $GBP$
- desired BW

We must find the values of the following critical parameters:
- $C_{Fmax}$: the maximum feedback capacitance such that the Bandwidth of the TIA is preserved
- $C_{Fmin}$: the minimum feedback capacitance such that the ROC is at most 20dB/decade
- $Q_{min}$: the minimum Q-factor attainable with a given op-amp's GBP

Any feedback capacitance value in the range $[C_{Fmin},\quad C_{Fmax}]$ will ensure stability and acceptable overshoot percentages/ rise-times. For CF = $C_{Fmin}$, the Q-factor will be 1. 
## Simulation
## Implementation
## Bibliography
