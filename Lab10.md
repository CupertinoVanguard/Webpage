## LAB 10

### Lab 10

**Write Up:** 

This lab involved utilizing Vivek’s simulator (Thanks Vivek!) and getting more experience with the various commands within the environment. 

**Simulator**
The simulator includes various functionalities. The various interfaces are included below:

GUI - allows start and stop from python

<img width="292" alt="image" src="https://user-images.githubusercontent.com/23284665/169671526-adb5b28f-40db-49bb-8705-d970dcbd26e1.png">

Functions and methods: These allow interacting with the GUI pages

<img width="184" alt="image" src="https://user-images.githubusercontent.com/23284665/169671570-bb2276d6-114a-4d60-aa9f-e00d331f5954.png">

Interactive Elements

I am able to interact with the simulator using the up and down arrow to increase the translational speed and the right and left arrows to increase/decrease the rotational speeds. There is no specific positional control for the simulator. For the plotter, There are several buttons available that allows me to reset, view only a particular type of plotting (odom, truth, and belief), and track distances. The mouse also allows moving around the map and control magnification. 

Commander class:

The commander class includes all the methods I need to extract information or handle info regarding the simulator and plotter.

<img width="1015" alt="image" src="https://user-images.githubusercontent.com/23284665/169671676-a48282b1-4725-43aa-b6d1-2e18472b215e.png">


**TASK 1**

The first involved moving the robot in a small square. This involved creating forward, turn, and stop methods. For the turn method, the .set_vel method had to be used with the first parameter as 0 for no translational velocity and 1.5 for angular velocity. 1.5 rad/s is assumed to be a close enough approximation to pi/2 rad/s (90 degrees/second). The forward method used the set_vel method with the first parameter set to 0.5 m/s for translational velocity and 0 rad/s for the angular velocity. Finally, the stop method set both translational and angular velocities to 0. 

![image](https://user-images.githubusercontent.com/23284665/164478055-a826a48e-1688-4f20-9930-e16bad54c651.png)

To execute the turns the following code was utilized:

![image](https://user-images.githubusercontent.com/23284665/164478591-9e7244cd-dca1-4ce0-8a45-3e35bc13384c.png)

This code is designed to only be run once and executes each of the defined actions for approximately only one-second using await asyncio.sleep(1). This was chosen over time.sleep(1) since await asyncio.sleep(1) is non blocking and only works within this particular thread’s scope. After each action, the plot_pose method is used as well which plots both the odometry and the ground truth in red and green respectively. The video of that is in the following [link](https://youtu.be/MKw_8O-XNko). The second test to prove that the same shape occurs during each iteration is in this [link](https://youtu.be/Vy05m8nSdhE). 

The pose_plot is included below:

![Screen Shot 2022-04-21 at 10 08 28 AM](https://user-images.githubusercontent.com/23284665/164479926-e01295dd-7620-4a0d-8cbe-f8a8ff46ed73.png)

As can be seen, the ground truth is vastly different than that of the odometry data. This likely occurs to the errors in measurements, and the turning potentially occurring when and how the measurements occur. Additionally, it seems as though odometry seems to struggle in representing the closed loop movement and thus does not itself produce the expected closed-loop shape. 

If I were not reset after each iteration and I continuously run the square loop, this is the ground truth I receive:

<img width="436" alt="image" src="https://user-images.githubusercontent.com/23284665/169671830-5d1d29da-cd6c-4ce8-b45c-554d1674b3cb.png">

The ground truth creates a pretty cool design since the square is never a perfect square. The design shows the square increasingly becoming titled due to the imperfect 90 degree turns. 

**TASK 2**

This involved doing closed-loop movements. The final code for the closed-loop movement is the following: 

![image](https://user-images.githubusercontent.com/23284665/164480258-1d7e203d-4104-40ed-8557-21809ebb3ca3.png)

The code runs as long as the environment is active and constantly checks if the sensor value is less than a particular threshold, then the robot turns by approximately pi/2 radians since the loop runs approximately every second. This roughly translates to 90 degrees. This allows the robot completely turn away from an object and heads in the opposite direction. The robot approaches the wall at approximately 0.35 m/s which seemed slow enough for the robot to move yet not hit the wall. If the speed was increased, the robot can often overshoot and get too close to the wall affecting the turning once an obstacle is detected. After each loop, the pose is plotted as well and can be seen in the video. Since a large enough padding was provided, there were no crashes noticed during multiple trials. Additionally, the closest sensor reading the simulations seems to return before the robot actually turns away is 0.36. 

[Result of closed loop](https://youtu.be/yb5D1bhuALE)

Obviously, there are a few instance, where small crashes do happen. These crashes occur in instances in which the robot enters the wall at a large obtuse angle which makes the distance reading relatively unreliable for the obstacle deflection. However, this case is relatively rare due to the extremely large padding I added. However, also sometimes, the padding does cause the rest of the robot to partially crash into the obstacle since the TOF is only in the front. 

A solution to the problem included above would be to add 3 more TOF's with two on both sides and one in the back. THis would limit the errors caused by only having one TOF and the crashes that occur at obtuse angle since the back TOF maybe utilized to get a better distance reading. 

