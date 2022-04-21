## LAB 10

### Lab 10

**Write Up:** 

This lab involved utilizing Vivek’s simulator (Thanks Vivek!) and getting more experience with the various commands within the environment. 

**TASK 1**

The first involved moving the robot in a small square. This involved creating forward, turn, and stop methods. For the turn method, the .set_vel method had to be used with the first parameter as 0 for no translational velocity and 1.5 for angular velocity. 1.5 rad/s is assumed to be a close enough approximation to pi/2 rad/s (90 degrees/second). The forward method used the set_vel method with the first parameter set to 0.5 m/s for translational velocity and 0 rad/s for the angular velocity. Finally, the stop method set both translational and angular velocities to 0. 

![image](https://user-images.githubusercontent.com/23284665/164478055-a826a48e-1688-4f20-9930-e16bad54c651.png)

To execute the turns the following code was utilized:

![image](https://user-images.githubusercontent.com/23284665/164478591-9e7244cd-dca1-4ce0-8a45-3e35bc13384c.png)

This code is designed to only be run once and executes each of the defined actions for approximately only one-second using await asyncio.sleep(1). This was chosen over time.sleep(1) since await asyncio.sleep(1) is non blocking and only works within this particular thread’s scope. After each action, the plot_pose method is used as well which plots both the odometry and the ground truth in red and green respectively. The video of that is in the following [link](https://youtu.be/MKw_8O-XNko). The second test to prove that the same shape occurs during each iteration is in this [link](https://youtu.be/Vy05m8nSdhE). 

The pose_plot is included below:

![Screen Shot 2022-04-21 at 10 08 28 AM](https://user-images.githubusercontent.com/23284665/164479926-e01295dd-7620-4a0d-8cbe-f8a8ff46ed73.png)

As can be seen, the ground truth is vastly different than that of the odometry data. This likely occurs to the errors in measurements, and the turning potentially occurring when and how the measurements occur. Additionally, it seems as though odometry seems to struggle in representing the closed loop movement and thus does not itself produce the expected closed-loop shape. 


**TASK 2**

This involved doing closed-loop movements. The final code for the closed-loop movement is the following: 

![image](https://user-images.githubusercontent.com/23284665/164480258-1d7e203d-4104-40ed-8557-21809ebb3ca3.png)


The code runs as long as the environment is active and constantly checks if the sensor value is less than a particular threshold, then the robot turns by approximately pi/2 radians since the loop runs approximately every second. This roughly translates to 90 degrees. This allows the robot completely turn away from an object and heads in the opposite direction. The robot approaches the wall at approximately 0.35 m/s which seemed slow enough for the robot to move yet not hit the wall. If the speed was increased, the robot can often overshoot and get too close to the wall affecting the turning once an obstacle is detected. After each loop, the pose is plotted as well and can be seen in the video. Since a large enough padding was provided, there were no crashes noticed during multiple trials. Additionally, the closest sensor reading the simulations seems to return before the robot actually turns away is 0.36. 

[Result of closed loop](https://youtu.be/yb5D1bhuALE)

