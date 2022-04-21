# LAB 7

### Lab 7

**Write Up:** 

This lab took several trials and tribulations and was hard! The lab involved several sections: implementation, theoretical simulations, and artemis implementation. 

**Implementation**

Step Response:
The step response for Task A was implemented by holding the robot still for the first couple seconds, running at max forward speed - 60 PWM for the next few seconds, and stopping the car yet again for the next few seconds. Thus, this could simulate a controlled-duration step response with amplitude of 60 PWM. 
Programmatically, this was done by creating a stepResponse command that uses a counter to count up. Only when the counter reaches the specified range are the motors given an input of 60 PWM. Utilizing a counter gave more control over the duration and avoided having to deal with adding additional variables to handle previous time and current time.
Similar to the PID lab, the arrays that keep track of the pwm values, tof values, and time are updated and sent back to the python script to graph the PWM values and distance values vs time
Below is the code for the step response and the graphs for pwm vs time and distance vs time. 

![Screen Shot 2022-04-01 at 11 11 05 PM](https://user-images.githubusercontent.com/23284665/164489490-0ab6acf5-5048-4638-bf6a-b5df3423d6a3.png)


Velocity graph:
The velocity was computed using the graph of the TOF values vs time. This was done by taking the difference of the TOF value at i+1 and the TOF value at i over the delta in time. Since the initial graph had extreme variability, some data filtering was implemented to minimize the spikes in the graph. The graph is included below. 

![Screen Shot 2022-04-01 at 11 41 30 PM](https://user-images.githubusercontent.com/23284665/164489560-eb6ea35e-8584-41ee-9a39-c257d92f550b.png)


Finding dynamics values - m & d:
The maximal velocity noticed was -525 mm/s.
d was found by computer 1/525.
m was found by using the equations from the slides = d * 2.5ln(0.1) = 0.0021
The 2.5 seconds represents the 90% rise time. As such from the graph it can be seen that to reach the 90% of 525 mm/s, it took 2.5 seconds.

**Sanity Check**

![image](https://user-images.githubusercontent.com/23284665/164498316-e90cbeb9-1402-4134-8186-282840f1628c.png)
![image](https://user-images.githubusercontent.com/23284665/164498464-3157f49a-2e48-4279-8303-d3d02cdf94c7.png)


Dynamics matrix: Using the d and m values from the previous section the dynamics matrix was initalized using the form provided in the lecture slides. To discretize the matrix, the A & B matrices were multiplied by the provided DeltaT of 0.130 s and added to by the identity matrix. The final discretized matrices that were used is included below.
![image](https://user-images.githubusercontent.com/23284665/164491351-463e832c-6b5b-4b07-b225-27756976227a.png)
![image](https://user-images.githubusercontent.com/23284665/164491397-2fbffe9f-b433-4555-a7e2-c2a13b48ee3f.png)

The rest of the implementation followed very closely  with the code provided in the lab. Of interest however,were the various sigma values. The value that seeme to impact most was the value labeled sigma_3. Several tests on the sigma value were executed; however, the best value chosen was 8. This indicates a higher reliability on the sensor values in an effort to try and match up the KF estimates to the sensor readings from the PID as can be seen from the graph. This was done since the PID values seemed to show relatively effective control. 

This is the inital PID graph that is used as the template:
![image](https://user-images.githubusercontent.com/23284665/164500501-e7d689b1-6071-4702-8c81-6b1fdc6d50c2.png)

This is the Kalman result: 
![image](https://user-images.githubusercontent.com/23284665/164492401-a87323b4-f15e-4ea1-8445-26720b8c18a3.png)
![Screen Shot 2022-04-01 at 7 31 44 PM](https://user-images.githubusercontent.com/23284665/164495517-b7a9b4d4-ab88-40f0-b4b1-0f6b0755605b.png)

However, to avoid having to completely depend on measurement readings, the KF estimates were not exactly matched in the region that signifies the initial motion before reaching setpoint steady state. 

**Artemis Implementation**

The artemis implementation followed a very similar procedure as the python implementation. To aid with testing, a separate command called KF_Estimate was created. 
Primary Issue: How do you initialize the state array to feed in during the first pass of the KF:
Solution: assume a starting reading based on trial set up (in my case this was 2000 mm) and use this value to initialize the starting state. 
This was done to essentially use the first pass as already having a prior belief and a new measurement to better functionalize the kalman filter. 
The P, I, D constants are still utilized to assist with the PWM input values. Since, the KF values were theoretically tuned to better correspond to the measurements, the PID constants worked extremely well during the test run. The result of the test run is included below in the [video](https://youtube.com/shorts/A1lVyVmczrc?feature=share). 

![image](https://user-images.githubusercontent.com/23284665/164500187-40630b3e-ec4d-42d5-bc37-ec72d013e53d.png)

As can be seen from the graph above, the motion is extremely smooth, though once steady state is reached, the PWM value fluctuates constantly as the KF tries to estimate the position and the PID estimation contributes this fluctuation. 

The code below utilizes the Kalman estimation to find the PID input into the motors. Based on this, the motion model input is calculated and passed through to the kf_estimation method to update the position
![image](https://user-images.githubusercontent.com/23284665/164503596-9027832a-c2ee-48a2-9816-e3fb580dd6d0.png)
