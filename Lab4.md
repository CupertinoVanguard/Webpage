## LAB 4

### Lab 4

**Write Up:** 

This lab involved relatively limited software steps. The amount of experiments I could personally run was unfortunately limited. I could not effectively utilize the  the sensors to attain various measurements since the week designated for lab 4 was one for which I had to head back to my home state for citizenship interviews and paperwork. The limitations included the inability to properly use the artemis battery (since can't use the JTAG connector), placing the sensors in various positions on the RC car, and communicating with the Artemis board. 

Properties of A that were measured:

Dimensions: 17cm vs 14.5cm
Weight: 
Time to charge depletion at full operation: 8 minutes

Experiment from B that was utilized:
Manual control - distance from the wall at full speed approach:

Preliminary statistics:
Distance starting to wall: 3.55 m (room limitations)
Battery charge at run: presumably 100%
Wall coordinate - 0 cm
Deceleration start - 120 cm from wall
Minimum distance achieved from wall - 14.5 cm 

The reasoning behind choosing this particular experiment was that it reveals a signficant piece of information: braking distance. Due to the lack of TOF's on the robot, this experiment allows determining the actual manual measurements for the break distance. With the data from above. The breaking distance is 120 cm - 14.5 cm = 105.5 cm. This manual measurement, though not ideal, is a good enough replacement for the TOF measurements. Additionally, potentially the 300cm - 120cm distance traveled in near full speed and time stamps from the video to attain a speed. The video of this experiment is included in the following link:

[EXPERIMENT - LINK](https://youtu.be/z19z21m2-6U)

The experiment above is run on carpet within a fixed length of the room. The robot starts from one side of the room in each trial and drives to the other rend of the room. The goal was always to make the robot drive straight; however, it seemed nearly impossible to achieve due to imperfect motor coupling present in the robot. An additional trial is included below(though not all these are completely perfect due to surface unneveness discussed later):

[EXPERIMENT 2 - LINK](https://youtu.be/odt9LKFyRoc)

In the above trial, the control was released a second earlier than Experiment B. This caused the robot to stop nearly 50cm from the wall, but it displayed a very similar breaking distance value of 90 cm

Another trial where the robot control was released one second after in comparison to Experiment B is included below: 

[EXPERIMENT 3 - LINK](https://youtu.be/3UiqbSONvFU)

While the breaaking distance is hard to recognize in this video duw to the limitations of the experiment region. These additional trials are included to ephasize the effect of timing in this experiment. A second early, and the robot is nearly 40 additional cm from the wall. A second late and the robot iss sure to crash into the wall. However, what was noticed through all these experiments was that the breaking distance wasnearly constant. 


## Experimental Differences

Obviously this experiment was run in a very different situation compared to where the rest of the labs for the semester would occur. The carpet used in these experiments raise 2 significant challenges that the floor at Phillips would not: surface unneveness and increased friction. Unlike the tiles of the Phillips floor, the carpet is bumpy and includes random and uneven height gradients throughout the surface. This would impact duration and completion of turns and the distance control. Additionally, carpet has much higher static friction, resulting in a much higher PWM to beat the friction compared to the PWM required to beat the minimal static friction of the Phillips tiles. As such if experiment B were to be run on the Phillips floor, the robot would cast much longer and would likely hit the wiall if the same control inputs are utilized. 

Again, the effectiveness and the experimental setup are relatively limited by the situation described prior.


