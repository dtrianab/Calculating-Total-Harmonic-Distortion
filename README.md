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


Configuration of ADC Module:
```c++
void adc_setup ()
{
  NVIC_EnableIRQ (ADC_IRQn) ;   // enable ADC interrupt vector
  ADC->ADC_IDR = 0xFFFFFFFF ;   // disable interrupts
  ADC->ADC_IER = 0x80 ;         // enable AD7 End-Of-Conv interrupt (Arduino pin A0)
  ADC->ADC_CHDR = 0xFFFF ;      // disable all channels
  ADC->ADC_CHER = 0x80 ;        // enable just A0
  ADC->ADC_CGR = 0x15555555 ;   // All gains set to x1
  ADC->ADC_COR = 0x00000000 ;   // All offsets off
  
  ADC->ADC_MR = (ADC->ADC_MR & 0xFFFFFFF0) | (1 << 1) | ADC_MR_TRGEN ;  // 1 = trig source TIO from TC0
}
```

Timer Counter (TC) [See page 856]

Configuration of Timer Module:

```C++
 pmc_enable_periph_clk (TC_INTERFACE_ID + 0*3+0) ;  // clock the TC0 channel 0
  TcChannel * t = &(TC0->TC_CHANNEL)[0] ;    // pointer to TC0 registers for its channel 0
  t->TC_CCR = TC_CCR_CLKDIS ;                // disable internal clocking while setup regs
  t->TC_IDR = 0xFFFFFFFF ;                   // disable interrupts
  t->TC_SR ;                                 // read int status reg to clear pending
  t->TC_CMR = TC_CMR_TCCLKS_TIMER_CLOCK1 |   // use TCLK1 (prescale by 2, = 42MHz)
              TC_CMR_WAVE |                  // waveform mode
              TC_CMR_WAVSEL_UP_RC |          // count-up PWM using RC as threshold
              TC_CMR_EEVT_XC0 |              // Set external events from XC0 (this setup TIOB as output)
              TC_CMR_ACPA_CLEAR | TC_CMR_ACPC_CLEAR |
              TC_CMR_BCPB_CLEAR | TC_CMR_BCPC_CLEAR ;
  
  t->TC_RC =  875 ;     // counter resets on RC, so sets period in terms of 42MHz clock
  t->TC_RA =  440 ;     // roughly square wave
  t->TC_CMR = (t->TC_CMR & 0xFFF0FFFF) | TC_CMR_ACPA_CLEAR | TC_CMR_ACPC_SET ;  // set clear and set from RA and RC compares
  
  t->TC_CCR = TC_CCR_CLKEN | TC_CCR_SWTRG ;  // re-enable local clocking and switch to hardware trigger source.
  setup_pio_TIOA0 () ;  // drive Arduino pin 2 at 48kHz to bring clock out

```


Calculation of Harmonics

http://wiki.openmusiclabs.com/wiki/ArduinoFFT


Task Next LAB:

Get the samples from COM and compare it

Do an application  in labview to visualize the data from  ADC conversion and FFT

●	Try FFT algorithm
●	Get harmonics of square wave and sinusoidal wave
http://www.instructables.com/id/Arduino-Frequency-Transform-DFT/





MODIFIED GOERTZEL ALGORITHM It is important to choose the right algorithm for detection to save memory and computation time. The Goertzel algorithm is the optimal choice for this application because it does not use many constants, which saves a great deal of memory space. Also, only eight DTMF frequencies need to be calculated for this application, and the Goertzel algorithm can calculate selected frequencies. This saves computation time. The DTMF frequency is transformed to a discrete fourier transform (DFT) coefficient. The relationship between the DTMF frequency (fi) and the DFT coefficient (k) is given in equation (1): (4) 

 
 

Note that k is the nearest integer to equation (1). For each k, the state variable, vk(n), is obtained by using the recursive difference equation shown in equation (2): (2)

 
 	





Block diagram of THD measurement system
 
 
