### LAB 1

**Write Up:** 

This lab involved setting up the artemis and experimenting with the Arduino IDE. The first example experimented with in this lab involved using `digitalWrite()` to blink the LED onboard by driving it high for half a second then driving it low for yet another half a second. The demonstration of this is included in link [1]. 

  The second task involved experimenting with the serial monitor. The taks involved (with code already provided) having the serial monitor output what it reads in as input. In such a fashion, it essentially _echoes_ the input. The video of the result is included in link [2]. 
  
  The next component involved experimenting with analog reading and A-to-D conversion. By utilizing the selected ADC pin that can sense temperature change and incoming analog voltage change, the analog value is stored using an integer. The analog value resolution is preset to be 16 bits on both read and write. The analog value is then written to the LED_BUILTIN Pin using `analogWrite()`. Since the write occurs to a digital pin, PWM conversion occurs. The value read by the analog pin is divided by the maximal value from 16 bits to derive the duty cycle. This leads to a LED intensity change when additional rise in value is detected on the ADC pin specified. 
  
  The microphone task involved utilizing the PDM library to read and process the read from the microphone onboard. The PDM vallue is read into a buffer and processed using the FFT ibarary in the printLoudest method. Additionally, for the extra task, the print Loudest method is made to return the float value calculated. Though the assignemnt says to turn the LED on when the frequency is the whistling range, the range included is in the lower frequency range due to my inability to whistle. A screenshot of the amended code is included below: 
  
  ![image](https://user-images.githubusercontent.com/23284665/151728195-753cce20-c708-4a3f-9206-365292d05e79.png)


**_Links for Lab 1_**
1. [1 - Blink it up](https://www.youtube.com/watch?v=De2FU9awyLI)
2. [2 - Serial](https://www.youtube.com/watch?v=1Dfg-vEx9Ps)


