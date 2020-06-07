# Calculating-Total-Harmonic-Distortion
Calculating Total Harmonic Distortion with Arduino Due

THD is defined as the ratio of the equivalent root mean square (RMS) voltage of all the harmonic frequencies (from the 2nd harmonic on) over the RMS voltage of the fundamental frequency (the fundamental frequency is the main frequency of the signal, i.e., the frequency that you would identify if examining the signal with an oscilloscope).

The below equation shows the mathematical definition of THD (note that voltage is used in this equation, but current could be used instead):

Total harmonic distortion, or THD, is one way to gauge power supply quality. It indicates how much of a harmonic component the voltage and current waveforms contain, and it serves as an indicator of the extent of the waveform distortion that is caused as a result. The Japan Industrial Standards define THD as the ratio of the harmonic component to the fundamental component. In addition to power waveforms, THD is used to analyze vibration and audio waveforms.

The main features available in Arudino Due are:

 - 12-bit Resolution
 - 1 MHz Conversion Rate
 - Wide Range Power Supply Operation
 - Selectable Single Ended or Differential Input Voltage
 - Programmable Gain For Maximum Full Scale Input Range 0 - VDD
 - Integrated Multiplexer Offering Up to 16 Independent Analog Inputs
 - Individual Enable and Disable of Each Channel
 - Hardware or Software Trigger
 - External Trigger Pin
 - Timer Counter Outputs (Corresponding TIOA Trigger)
 - PWM Event Line
 - Drive of PWM Fault Input
 - PDC Support
 - Possibility of ADC Timings Configuration
 - Two Sleep Modes and Conversion Sequencer
 - Automatic Wakeup on Trigger and Back to Sleep Mode after Conversions of all Enabled Channels
 - Possibility of Customized Channel Sequence
 - Standby Mode for Fast Wakeup Time Response
 - Power Down Capability
 - Automatic Window Comparison of Converted Values
 - Write Protect Registers
