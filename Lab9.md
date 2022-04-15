## LAB 6

### Lab 6

**Write Up:** 

This lab involves creating a mapping of the obstacle layout based off of measurements from the front TOF! This lab involves 3 major parts: in place rotation, data recording, data interpretation. 

**IN PLACE ROTATION**

This part of the lab took a while to execute since figuring out constant rotation with the prior PID logic, especially since the gyroscope readings seemed to drift. From the Arduino perspective, the PID code from lab 6 and gyro readings from lab 3 were fused into a new method called pid_map(). In order to utilize values from the python perspective, the distance, the time, and the rotation speed was stored in an array to be sent back through the SEND_DATA command (documented under Lab 6). To record more data for better accuracy, the interval between consecutive data collections was set to 250 ms. Another major edit made from Lab 6 PID was the PWM set Speed. As seen below, rather than including a potential reversal of PWM direction, only single direction PWM is maintinaed to avoid constant togggling bakc and forth during the turn to limit noisy and shifting data. The setpoint angle is approximately 35, though it never actually turns out to reach that particualr rotation speed. 

The final product of the turn is included below:


Notes about the turn: As can be seen from the video, there is a slight displacement that occurs during the turn. This seemed unavoidable, despite adding tape to lower the static friction. However, the translation is relatively minimal in real-time, leading to minimal impact on the final data as can be seen in the final mapping images.

**DATA RECORDING**

The data was sent back to jupyter lab in a similar fashion as Lab 6. The dataq was also parsed and appropriately assinged to the proper arrays. Data was collected at 4 different points: (5,-3), (5,3), (0,3), and (-3,-2). After each trial, the data was stored in a particular array and then stored in an appropriately named text file as seen by the snippet below: 

![image](https://user-images.githubusercontent.com/23284665/163515870-15f4fe71-b180-44a4-b1e0-0a42807c3e7f.png)

While recording the data, the robot was alwasy started in the same orientation, pointing along the positive x-axis. Additionally, all the data was taken along the -z axis, meaning the robot would turn in the counter-clockwise direction for achieving the postive 25 DPS. 

**DATA INTERPRETATION**

This part of the lab happened entirely from jupyter lab. The first step in interpreting the data involved attaining the various angles the robot achieved during the turn. Since the stopping of the robot during the turn was manual, there were certain moments of overshoot that persisted. The script for achieving the angle is included below: 

![image](https://user-images.githubusercontent.com/23284665/163516480-a8d6ae98-55ff-4fbf-8ed2-6859aba2ca3b.png)

THe angle at each step is calculated by find the delta_T based on current time and previous time and multiplying it by the gyro's rotational speed reading at that time step. The angle is accumulated throughout rotation to better represent the 360 degree rotation. 

Based on these angles, the following polar plots are plotted to provide a better representation of the measurements during the rotation
![image](https://user-images.githubusercontent.com/23284665/163516788-cb3bd14e-71c7-4bb8-b94c-5c3efbaf9b5d.png)
![image](https://user-images.githubusercontent.com/23284665/163516857-0ca9dfef-9d2a-4e9b-a11a-bef2b30d3bd3.png)
![image](https://user-images.githubusercontent.com/23284665/163516893-2c1dba0d-428e-4754-9f80-9d890f36150b.png)
![image](https://user-images.githubusercontent.com/23284665/163516913-075b36ff-90a3-4a5c-99b2-7d6be7e0dff6.png)

As can be seen the polar plots are relatively hard to comprehend and as such to get a better view, the polar coordinates are converted rectangular coordinates to better represent each perspective:

![image](https://user-images.githubusercontent.com/23284665/163517029-040454fe-be22-4af8-af1f-2570b4ba8263.png)
![image](https://user-images.githubusercontent.com/23284665/163517048-582d60a7-3ce0-4146-983e-665baab34e98.png)
![image](https://user-images.githubusercontent.com/23284665/163517086-6ae0714a-f03a-45c5-bbb4-461ffdaa4d39.png)
![image](https://user-images.githubusercontent.com/23284665/163517103-0100557a-7317-4e38-a6d1-0b96c1ddf57e.png)

From the 4 cartesian coordinate perspectives, all four corners are significantly clearer in comparison to the polar graph. Additionally, the sides of the bigger box are also clear, though the sides are slanted due to the positional drift during the turn analyzed before. However, the major issue with these four cartesian plots is the lack of clarity pertaining to the smaller box. This occurs since not all four rotation spots are equidistant from the box. As such, the corrdinate that provides the best perspective of the side(s) of the smaller box was the point (-3,-2) since it was relatively closer to the smaller obstacle. The uncertainty surround this paraticular obstacle is more exaggerated and apparents when the box's dimensions are superimposed upon the TOF based mapping. The process through which the plar coordiantes were converted to rectangular coordinates is included in the code snippet below: 


![image](https://user-images.githubusercontent.com/23284665/163517131-b2714b38-3531-416e-87df-4a4f06f70108.png)


![image](https://user-images.githubusercontent.com/23284665/163517146-e50c134c-14a3-4b04-b85d-c524858f8f66.png)


![image](https://user-images.githubusercontent.com/23284665/163517199-07a40c21-d144-468b-b6ab-5ff536261c99.png)



