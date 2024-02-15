# DESIGN AND IMPLEMENTATION OF A PROPER PHOTODIODE TRANSIMPEDANCE AMPLIFIER
## Introduction
I wanted to build a proper photodiode transimpedance amplifier capable of working with infrared light pulses at frequencies slighlty above 100kHz.

A regular transimpedance amplifier or transresistance amplifier employs only a resistor in the feedback path of the op amp. Bandwidth limitations are dictated directly by the input capacitance of the circuit, i.e. the capacitance of the photodiode and that of the op-amp input stage.
An important distinction to make going forward with this projects is that what I've implemented is valid only for constant GBW product op-amps and does not apply to non-unity gain stable or decompensated op-amps.

A regular non-compensated transimpedance amplifier looks like this:
[diagrama schemeit cu transres]

The amplifier presents a second order transfer function, that can be found derived <a href="https://www.planetanalog.com/seemingly-simple-circuits-transresistance-amplifier-part-1-approximating-op-amps/">here</a>. Of more interest to us, at least from the view point of stability analysis, are **$A_{OL}$** and **$\frac{1}{\beta}$**. We need this functions to determine the Rate of Closure of the two curves, meaning the difference between the slopes of the open loop gain and $\frac{1}{\beta}$. A ROC below or equal to 20dB/decade is considered the caharcateristic of a stable system, alternatively the ROC can be used to estimate the phase margin.

As such we have for the non-compensated TIA the folllowing formulas:
$A_{OL}=\frac{A_{DC}}{1+\frac{s}{w_{A}}}$
$\frac{1}{\beta}=(1+s\cdot R_{F} C_{I})$
They approximatively intersect at $f_{I}=\frac{1}{2\cdot\pi}\cdot \sqrt{\frac{2\pi\cdot GBP}{R_{F}\cdot C_{I}}}$, where GBP is the Gain-Bandwidth Product of the used op-amp.

A proper derivation of this formulas would better account for the signs of the different functions, but for our ROC plot needs, those are irelevant, as we ignore phase in our analysis.

Below is presented the test circuit used to determine $A_{OL}$ and $\frac{1}{\beta}$. The test circuit could be bettered by adding a copy of the circuit as a dummy load, to better represent the actual loading at the nodes of the op-amp.
[imagine test circuit stabiliate]

Plotting the required functions, we observe in the plot made available below that the ROC is approximatively 40dB/dec.
[imagine bode plot 80dB]

The explanation for this is simple, the dominant-pole model of the op-amp ensures a -20dB/dec descending slope, whilst $\frac{1}{\beta}$ presents a single zero and a +20dB/dec ascending slope.

This circuit is not stable, ringing will be present at even moderate frequencies.

For such circuits, the size of the feedback resistor is quite important. A TIA with a very big feedback resistor, think on the order of 1 $M\Omega$ to 10s $G\Omega$, need exceedingly smaller compensation capacitors, sometimes only the parasitics of the PCB might suffice. However, any stray or intended capacitance across the feedback resistor will reduce the bandwidth of the TIA, so some attention is required on the layout side of things.

## The solution to our woes...
Comes from adding a carefully selected feedback capacitor across the feedback resistor.

This is the equivalent circuit of a TIA with input and photodiode capacitances depicted.
[circuit TIA cu capacitati parazitice]

The input capacitance is defined as $C_{I}=C_{D}+C_{CM}+C_{DIFF}$, where $C_{D}$ is the junction capacitance of the photodiode, and the other two terms are the parasitic capacitances of the input impedance of the op-amp.

A simple stability analysis is quite straightforward, the same as for the non-compensated transresistance amplifier, with the mention that addition of $C_{F}$ introduces a pole in expression of $\frac{1}{\beta}$. For us, $\frac{1}{\beta}=\frac{1+s\cdot R_{F}(C_{F}+C_{I})}{1+s\cdot R_{F}C_{F}}$, as depicted below:
[piecewise deconstruction of 1/beta]

The introduction of a pole cancels out the positive slope imparted by the zero and stability is achieved.

A fist step towards building a working circuit is determining the maximum feedback cap value, such that the frequency of the pole of $\frac{1}{beta}$ is smaller or equal to the desired bandwidth of the TIA. The idea behind this is that beyond the pole frequenct the overall transfer function of the circuit will decrease rapidly.

$$C_{F}\leq \frac{1}{2\pi\cdot R_{F}\cdot f_{-3dB}}$$

The next step would be to determine the minimum GBP of an op-amp that could be used for our application.

However, I've taken a different approach.

## Q-based design procedure
This is based on the presentation "Simple Transimpedance Designs Using High Speed Op Amps" by Steffens and Ramus.
After long work the gentlemen from TI obtain the following transfer function characterizing the workings of the TIA:
[slide 7 cu mentiune sursa]
A regular second order transfer function is of the form $G(s)=G_{0}*\frac{w_{o}^{2}}{s^{2}+s\cdot\frac{wo}{Q}+w_{o}^{2}}$. If you are familiar with the other notation convention, $\zeta$ is in this case is $\frac{1}{2\cdot Q}$, and $C_{S}$ is the same as $C_{S}$
[slide 8]
[slide 10]

Of interest to us is the equality $Q = \frac{F_{o}}{F_{c}}=\frac{P_{1}}{F_{o}}$

$$\begin{align}
Q\cdot F_{o}=P_{1}\\
\\
Q\cdot \sqrt{Z_{1}\cdot GBP}=P_{1}\\
\\
Q^{2}\cdot Z_{1}\cdot GBP=P_{1}^{2}\\
\\
Q^{2}\cdot GBP \cdot \frac{1}{C_{F}+C_{S}}=\frac{1}{2\pi\cdot R_{F}C_{F}^{2}}\\
\\
\end{align}$$

In the end we obtain the following quadratic equation: 
$$2\pi\cdot Q^{2}\cdot GBP\cdot R_{F}\cdot C_{F}^{2}-C_{F}-C_{S}=0$$

The equation has two solutions , only one will be valid, and that is simple to prove. There are two ways to go about this. The first one assumes that the capacitance of the photodiode is at least 10x times bigger than the feedback cap, as such we neglect the term $C_{F}$ from the equation $2\pi\cdot Q^{2}\cdot GBP\cdot R_{F}\cdot C_{F}^{2}-C_{F}-C_{S}=0$ and obtain $2\pi\cdot Q^{2}\cdot GBP\cdot R_{F}\cdot C_{F}^{2}=C_{S}$.
Approximated as such, the solutions for $C_{F}$ are $\pm\sqrt{\frac{C_{S}}{2\pi\cdot Q^{2}\cdot GBP\cdot R_{F}}}$. One of them is obviously negative, so we have the solution $C_{F}=\frac{1}{Q}\cdot\sqrt{\frac{C_{S}}{2\pi\cdot GBP\cdot R_{F}}}$.

This approximation (or similar) ones only work with gigantic photodiodes, for photodiodes with capacitances closer to the feedback values we need a different approach:
$$\begin{align}
