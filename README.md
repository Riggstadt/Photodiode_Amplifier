# PDTIA
Photodiode Transimpedance Amplifier 
## Issues
PCB: 
- Used two 2X 2.5mm screw terminals without lateral rails instead of 4x 2.5mm screw terminal => issues with spacing
- IR photodiode is too close to the terminals
- Terminals should be moved more to the side
- Better connector for ammeter connection: instead of 2.54mm header use somethig else, like a JST connector
- The PCB should be a rounded rectangle with M3.5 mounting holes at every corner
- The opamp should either be bigger or use passive components more appropriate in size to the SO8 footprint
- Except for the feedback resistor, the others should be 10K instead of 100K to not affect the stability of the circuit
- The PD is reversible and cand have and additional bias voltage, but needs to have another type of connector, because soldering and desoldering it won't work every time
- To have a better learning platform I should add more testpoints and a develop a IR emitter-based test fixture as well as a nano-Ampere current sensor for the PD current
- Unused second opamp should be used as a buffer for the DC offset of the main TIA
- Add 0.1uF and 1uF filter capacitors as close as possible to opamp
- Narrower traces could be used
- make single vcc plane or physically separate VCC from output signal


Other observations:
- Some op amps purposefully designed to be used as TIAs will have gains exceeding 1M, most jellybean opamps will have at most 100K or less
- The slew-rate (SR) [V/us] should be way bigger than for a jellybean part: i.e. the LM358 has a SR of 0.5V / us; Testing the LM358 as a precision diode, I've learned that even modest 10kHz sinewaves experience destortion due to the low slew-rate
- Offset voltage should be as low as posssible (tens to at most hundreds of microvolts)
- Differential capacitance and Common Mode capacitance should be specified in the d/s
