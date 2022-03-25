
## LAB 6

### Lab 6

**Write Up:** 

This lab involved creating PID control to substitute in for the open loop control implemented in Lab 5. The base code for the PID involved three primary sectons: PID implementation, PID value collection, and output PWM control. 

**PID IMPLEMENTATION**
The PID implementation worked relatively simply. It involved having access to the variables corresponding to previous error, current error, current time, and previous time. The implementation worked by mutlipling the P constant by the current error, the D constant with the time difference in error, and the I constant with the accumulation of all errors. The simple one line PID Pass is included below: 

<img width="1006" alt="image" src="https://user-images.githubusercontent.com/23284665/160038469-2abab2c3-313d-4ddf-891b-dceac21f19bb.png">

**PID VALUE COLLECTION**
This section invovled determining the various variables required for the method above to work. This meant the current error (calculated by subtracting the TOF reading from the setPoint), the previous error (based on the calculated error from the previous iteration), the current time (through a simple call of _millis()_), and the previous time (based on the current time of the previous iteration. This section is attached below: 

<img width="372" alt="image" src="https://user-images.githubusercontent.com/23284665/160039250-136eac40-53cc-48b2-ace3-b441d7fc244c.png">

**OUTPUT PWM CONTROL**
This part involved utilizing the value of the PID output and handling the value to be able to pass to the analogWrite function. The forward speed was limited at 60 PWM while the backward speed was limited at 150. Based on the sign of the PID output, the appropriate direction is chosen. The method is included below: 
<img width="296" alt="image" src="https://user-images.githubusercontent.com/23284665/160039734-a7d863db-bb22-45ae-97c4-7e5ced305225.png">

The second part of the lab dealt with tuning the PID values and the bluetooth front of data collection. In order to facilitate the bluetooth collection, the framework from Lab 2 was utilized. A PID_START method was created that drives a PID flag high to indicate that the current process running is the PID. A _pid_start()_ method was also created that implemented the necessary PID logic highlighted previously. This method was then called inside the main blutettoh loop. Along with this, 3 arrays were also created, each with 1000 available spaces, to record the values of the PID output, the TOF reaadings, and the current time to send back to the jupyter notebook. These arrays were updated every 50 ms or with a **frequency of 20Hz** to get parity in data and better accomodate the movement of the robot. A higher frequency would result in repetition of the same values and a lower frequency would potentially results in extrapolation of data. This interval was thus chosen through experimentation of what best fit the data collection and quality of data being collected. These values arae then later concatenated into a single string and fed in the EString variable from lab 2 to be written to the TX_FLOAT uuid variable. The actual process of sending data is initiated utilizing yet another command _SEND_DATA_. This method sets the pid-start flag to 0 to indicate to stoop the PID, stops the motors, and starts a forloop that sends the concatenated string to the jupyter notebook to read. This section is included below: 

<img width="1290" alt="image" src="https://user-images.githubusercontent.com/23284665/160044008-05f3480a-934f-43e1-9429-c38967453242.png">

**PID TUNING**
The P variable was the first value tuned. To minimize the oscillation. a value of 0.3 was zoned in on. By utilizing P, a small, yet predicted oscillation was noticed around the set point. However, due to the lack of deadband and no noticeable difficulty for the robot to move baased on just P, an additional boost that an Integrator value may provide was ruled to be unnecessary. As such, I was set to 0. To limit the oscillation and the overshoot noticed, a derivative term was also incorporaated. This would allow the robot to slow down more dynamically and limit the amount of constant back and forth. At best a single oscialltion would be potentially noticed. A D value of 70 was experimentally determined. A higher value slowed down thr bobt too mcuh limiting its ability to move forward properly. With a D value of 70 (in reality 0.07 since 70 is in terms of ms), the robot has a small oscialltion but stops the appropriate distance.The video of the robot working is including the youtube link attached below: 

<img width="457" alt="image" src="https://user-images.githubusercontent.com/23284665/160048505-ec01873f-a552-4c18-b838-ef0ff2a923b0.png">


**BLUETOOTH**
The values were send back as a string and read in a similar fashion to lab 2 with a function handler to deal with the notifications on the UUID associated with RX_FLOAT. THe string was sent with delimiters in the following formation a|b|c where a is the TOF reading, b is the time, c is the pwm output. However, some signaal processing had to be done due to the incorporaation of the derivative. Without the signal processing aspect, certain spikes in the pwm output would persist making it harder to interpert the PWM output vs time output. The signal processed graphs are included below: 

<img width="507" alt="image" src="https://user-images.githubusercontent.com/23284665/160048458-18da3a93-df21-461b-9468-f3a07a0300ac.png">
<img width="457" alt="image" src="https://user-images.githubusercontent.com/23284665/160048520-f6d4205a-ce4b-4fee-b952-45a1f8f8157d.png">


