# BETTER CIRCUIT DESIGN
## Changelog
I've done some modifications to my amplifier design, such that:
- Dark current error is mitigated
- Output signal is free from Common Mode offset (VCM) and input voltage offset (VOS)

### Dark current
The error caused by dark current are mitigated by getting rid of the accidental reverse bias present in the earlier iteration, where VCM >> VBIAS. In the zero-bias mode, the photodiode will have no (very very small) dark current, with increased sensibility being an additional advantage.
For the photodiode to work in the zero-bias mode, we must set the bias voltage of the diode's anode to the same value as our virtual ground and/or dc offset.
### 
To remove the static errors asociated with the design of this particular circuit, we must filter the ouput and remove all forms of DC offset, either in the form of VCM[



https://www.thorlabs.com/catalogpages/Obsolete/2018/DET36A_M.pdf
https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=9817
https://pages.olin.edu/files/isim/files/m15.lab_.oximeter.pdf
https://hackaday.io/project/18138-bullet-movies/log/49033-biased-photodetector-sensor
https://electronics.stackexchange.com/questions/695059/how-to-estimate-the-optical-power-given-the-scope-reading-for-this-scenario
https://electronics.stackexchange.com/questions/595250/what-is-the-role-of-capacitor-of-avalanche-photodiode-circuit
https://www.farnell.com/datasheets/3171157.pdf
https://www.instructables.com/How-to-Program-an-Attiny85-From-an-Arduino-Uno/
https://electronics.stackexchange.com/questions/109115/why-when-calculating-rise-time-do-we-use-2-2-%CF%84-rc-low-pass-circuit
