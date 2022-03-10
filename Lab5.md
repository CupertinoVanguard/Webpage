## LAB 5

### Lab 5

**Write Up:** 
 
Lab 5 included several sections: Parallel coupling the motor drivers, testing and experimenting utilizing an external power sources, and programming open loop. 

The motor driver utilized, as stated throughout the instructions, does not have enough power within a single H bridge to power the car’s motor. Since a single motor driver has two H-bridges for dual motor control. By parallel coupling the internal two H-Bridges, more current can be generated to drive the motor. The parallel coupling was physically achieved by joining Ain_1 & Bin_1 and Ain_2 & Bin_2. A similar looping structure was utilized for the output too. For each driver, there were two grounds, one from the artemis and one from the motor. In addition to the motor ground, the voltage (red wire) of the motor was connected to one of the 2 output from the motor driver with the ground being connected to the other. The difference in the voltage across the two outputs would lead to spin and power in a particular direction. The analog pins that were utilized for the PWM was A0-A2, A5. Additionally, the power and ground wires were appropriately attached to the VMM and Ground pins of the motor drivers respectively. 

Before directly attaching motor drivers to the battery source and the motors, the motor drivers were tested using an external power source and the oscilloscope. The external power source was fed into the VMM and GND. To mimic the 850 mAh batter, the voltage source was set to 3.7 V. To see the output of the motor drivers, the oscilloscope probe was connected to measure the voltage difference across one output to another. The input was driven through a PWM signal through the inputs using the arduino commands. The command utilized was analogWrite with the inputs being the associated input pin (output pin from the perspective of the Artemis) and the PWM signal (from 0 to 255). To see results on the oscilloscope, the PWM signal was written for a duty cycle of 50% or the analogWrite value of 128. The result on the oscilloscope is included below: 

![275226361_473929817546756_8211325731012277143_n](https://user-images.githubusercontent.com/23284665/157587006-5c914a17-6dc1-46b3-bc06-e4473497e43d.jpg)

The test described above was done right before the motor driver outputs were soldered directly to the motors. The same commands as above were run to see the spin on the wheels. The power source utilized was still external while the artemis was powered by the battery. The results of the spin are included in the two videos below. 


https://user-images.githubusercontent.com/23284665/157587066-6aec28df-cc98-46d4-8b4e-ae496a8d3fac.mp4



https://user-images.githubusercontent.com/23284665/157587102-10c55161-ebf2-40f5-80b1-04051e2d6c67.mp4



The final part of this lab involved implementing open loop drive. This involved executing a series of hardcoded commands for the robot to achieve a particular sequence of actions. The sequence of actions that I personally wanted to achieve was to drive forward and turn and drive back. The drive forward method was utilized by simply creating a desired PWM based voltage difference across each of the motors to move in a particular direction. To control the amount of time the motors move forward a delay of 2500 ms was added right after the move forward method. The forward method; however, presented one significant challenge. Unfortunately, one motor spun much faster than the other leading to a somewhat slanted motion of travel. The solution utilized in this instance was to bias the side the that was moving too fast by a particular scaling factor. This factor depend on several factors - the weight distribution across the car and the wheel friction/slippage. Since these values can not be directly calculated, the scaling factor was experimentally calculated through several tests. During testing, it was noticed that the only solution was to include a more flexible scaling factor that depend more on the range the PWM was in. As such, if-statements were included to make the scaling factor bias more flexible for any speed used, without having to constantly remeasure the scaling factor values. This is included in the code snippet below: 

<img width="367" alt="Screen Shot 2022-03-09 at 10 55 28 PM" src="https://user-images.githubusercontent.com/23284665/157586577-4e677992-34cd-45fa-9496-394cba3c9b5d.png">

<img width="262" alt="Screen Shot 2022-03-09 at 10 55 33 PM" src="https://user-images.githubusercontent.com/23284665/157586591-35bca77c-2c00-4dac-9695-651c94b5b29b.png">


The turning was achieved by spinning both motors in opposite direction. The 180 degree turn was determined rather empirically by changing the amount of time the turn was implemented since a high PWM was needed regardless to execute a turn on a slipper surface. 


The final video of the entire open loop with the robot moving in a near straight line and doing the a near 180 degree turn to drive back is included below. Though the return may look slanted, it is simply the lack of a perfect 180 degree turn being amplified:  


https://user-images.githubusercontent.com/23284665/157587579-19f72aa6-7b15-4788-b824-11845eca3084.mp4


**I APOLOGIZE IF THE VIDEOS MAY NOT SHOW UP ON THROUGH THE WAY IT IS CURRENTLY UPLOADED; HOWEVER, THERE SEEM TO BE PROBLEMS WITH MY ABILITY TO UPLOAD VIDEOS TO YOUTUBE AT THIS TIME
