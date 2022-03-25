
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

The second part of the lab dealt with tuning the PID values and the bluetooth front of data collection. In order to facilitate the bluetooth collection, the framework from Lab 2 was utilized. A PID_START method was created that drives a PID flag high to indicate that the current process running is the PID. A _pid_start()_ method was also created that implemented the necessary PID logic highlighted previously. This method was then called inside the main blutettoh loop. Along with this, 3 arrays were also created, each with 1000 available spaces, to record the values of the PID output, the TOF reaadings, and the current time to send back to the jupyter notebook. These arrays were updated every 50 ms or with a **frequency of 20Hz** to get parity in data and better accomodate the movement of the robot. A higher frequency would result in repetition of the same values and a lower frequency would potentially results in extrapolation of data. This interval was thus chosen through experimentation of what best fit the data collection and quality of data being collected. These values arae then later concatenated into a single string and fed in the EString variable from lab 2 to be written to the TX_FLOAT uuid variable. 

