# LAB 8
### Lab 8

**Write Up:** 

**Open Loop Implementation of Flip**

Before moving to the closed loop impelentation of the flip, the actualy action of the flip was tested. There were three seperate steps to the code: forward drive, stop, backward drive. The action of the flip, however, consisted of a combination of all these three stages during a particular instant. The forward motion, the sudden stop, and immediate flip of the wheel speed at a high pwm. In order to avoid coasting after stopping the robot, active breaking was used by driving all motor inputs high. The result of this open loop movement is included in this [link](https://www.youtube.com/embed/SpvhNd7bW58)


**Closed Loop Impelementation of Flip**

The closed loop implementation was a bit more complex. It invovled using the sensor readings to recognize when the appropriate distance from the wall was reached and then only executing the stunt once and turning back. The stunt code used is included below: 

![image](https://user-images.githubusercontent.com/23284665/164588900-ea7fc988-9742-4bdf-85ad-949e5704b352.png)

As can be seen from the impelementation, the distance is only sensed as long as the counter is 0. A single instance where the measured distance falls below 925 (which is relatively high to account for the coasting induced by such fast speeds), a flip is triggered, and the counter is increased. However,to make sure the robot continues to go backwards without being affected by the distance sensor, the second time the logic enters the if statement, the counter will be incremented again, making it neither 0 or 1 and putting it in a forever backwards stage. The video of this working is included in the following [link](https://youtu.be/1StS8apQ1rM). As can be seen the robot moves foward, detects the wawll, causing it to coast for a bit, flip and head backwards. Additionally a major observation was the necessity of the lack surface in comparieson the regula floor. When tried on the regular floor, the car struggled to flip; however, the black surface worked to provide the necessary friction it needed. Additionally for reference, the backward speed is 255 PWM and the forward speed is approximately 175PWM. 

**Open Loop Stunt**

For our open loop stunt, we decided to add some flair to our presentation and thus decided to build a ramp and decline similar to the ones found in the some hotwheels set. The stunt is here is the ability climb up the ramp, jump over to the decline, come down while staying on the decline for the majority of the descent. The speed of the robot was ramped up during the incline to provide maximal ability to gain enough speed to jump over the gaph. The three trials of this are included below:

[Trial 1](https://www.youtube.com/shorts/kTHjQ0wkGlM)
[Trial_2](https://www.youtube.com/shorts/gtQZkKfpIMI)
[Trial_3](https://www.youtube.com/shorts/V9A9JLMGVGo) 


**Bloopers**

[Blooper](https://youtube.com/shorts/-GQfIVtlC3A?feature=share)
Hey, at least it made it over the ramp and flipped right... :)

**Acknowledgements**
 
I worked on this lab with Swapnil Barot and Priyam Patel

