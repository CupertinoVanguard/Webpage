# Lab 13 - ENDGAME!

Lab 13 was an awesome, but hard last project. It involved following a trajectory and making sure all the points were hit more or less. Several challenges were faced along thew way, but overall it was a great experience putting all this work to make it work. 

## Introduction

The goal of this lab is to hit the various wavepoints:

<img width="109" alt="image" src="https://user-images.githubusercontent.com/23284665/169666428-20727dd0-a258-4103-97e2-5dcc2582b58f.png">

Our approach is listed below:
1) Start from a point (-2,-1) and get back to (-4,-3). The reason for not starting at (-4,-3) will be discussed in depth a bit later. 
2) Use compute control-esque logic from Lab 12 to calculate the movement and turns required from one point to another. 
3) Use closed loop control, specifically PID for both turning and going from one wavepoint to another. The forward movement will be controlled based on distance from the wall. 
4) Call the entire sequence from Python through a single function call.

The reaason we chose not to start from (-4,-3) were varied. The first involved the sheer inability of the robot to properly detect the wal from that far. The closest wall available from that particular point was nearaly 3 to 4 meteres out and was present at a very steep angle. As such the error in the distance measurement were simply too signficant to jeopardize the movement throughtout the rest of the trajectory. Secondly, it made more logical sense to sipmly return to the point once we were at 0,0 since that would simply mean turn and using the PID control yet again. Now the question maybe why not try right angles? This was our first solution when thinking about the point (-4, -3); however, we decided against it since it would involved potentially adding 2 more wavepoints to the trajectory leading to more error accumulation during the turns and PID movement that could impact the rest of the trajectory. 

THe final trajectory that still hits most all the points and mostly in the same order was the following. 
<img width="790" alt="image" src="https://user-images.githubusercontent.com/23284665/169666944-499a132b-85d5-4d2f-881f-f004a4150c3a.png">

## Python and Arduino

Before discussing the specifics of the movement, I first wanted to highlight how we designed python and arduino to work together. The following flow diagram helps better illustrate the process flow for a single transition:
<img width="1098" alt="image" src="https://user-images.githubusercontent.com/23284665/169667140-60b2dab7-4097-4911-b69d-d5180da1b587.png">

## Python Implementaation

The python implementation proved to be rather long and inefficient many times. The first idea we had was to just list every single command in series and tune it as needed. However, after realizing this was extremely bad programmatically and for debugging purposes, we utilized a class based approach. This approach involved creating a Trajectory class that would represent the individual trajectory. Based on the trajectory points in the format (x1, y1, x2, y2), the class would allow easy computation of the required traanslation in millimeters and turns involved to achieve the translation. A turnback function was utilized in the instances where the orientation of the robot post translation maay not be in the desired end orientationo fo the robot at that second point. With the spirit of modeling our movement afater compute control, we also accounted for the orientation of the robot in the trajectory class by storing an orientation variable. This allowed avoiding unnecessary turns and error accumulation when one was not necessary during a particular trajectory.

A note of interest here was that some angles maybe negative during turns. To compensate for that, we just did 360 + negative angle to get the clockwise spin angle.

Finally, from the python side, a method was created in which a loop runs through all the possible trajectories and executes the trajectory in the order of turn, wait, straight, wait, stop, wait, turn (if neeeded). As such the entire sequence if offboard and involves local path-planning. 

<img width="1275" alt="image" src="https://user-images.githubusercontent.com/23284665/169670986-2349a567-77d1-4385-9818-694ca04c1fb0.png">

## Arduino Implementation

The arduino implementation was relatively straight forward, or so we thought. The implementation of the turn was simple and only required a slight change from the lab 12 code. The only change was that rather than explicitly stopping at 360, now the method could stop at a custom angle provided. The turn was run exculisively on P with P = 4. The implementation of that code is included below:

<img width="1416" alt="image" src="https://user-images.githubusercontent.com/23284665/169671008-43ded3e6-a1ea-4b03-bc7f-ccbf8da89a51.png">

The moving straight, surprisingly, was what gave us most of our issues. The first issue we noticed was the sensor seemed to "cache" its readings. In a simple test carried out with the help of Vivek, we moved the robot from a wall, effectively chaning the distance of the robot from wall during every iteration. What we noticed was, the sensor seemed to cache the values of a previous iteration and supply that value as the reading of the current iteration. The solution that we utilized to temporarily avoid the issue was not ideal but effective enough. The solution was to add a checkDataIsReady() boolean check before executing the movement. If the data was not reading, the rest of the code would be blocked and not allowed to run.

To properly execuite PID, we needed an initial value. This value was recorded on the immediate call of the command and the setPoint was dynamically calculated at that point. They Python side would pass through the required displacement in mm to to the command, and the command would utilize the initial distance and the translation distance to calculate the relative setPoint. A diagram to better demonstrate the point is included below:
<img width="492" alt="image" src="https://user-images.githubusercontent.com/23284665/169668173-5a662029-678d-468b-b157-307d72e339f7.png">

The rest of the PID logic stayed the same from Lab 6 and 7 including the P & D constants, the speed control, and the error calculations.

<img width="1414" alt="image" src="https://user-images.githubusercontent.com/23284665/169671030-e5724719-9529-48dc-9b21-ffcde2fdcb5a.png">

<img width="626" alt="image" src="https://user-images.githubusercontent.com/23284665/169671044-ef60b265-c97b-4c99-b09e-0957e93810fc.png">

## Results

This section is rather complex as we ran into various issues. Each issue is discussed in detail and the results are included below was well.

### Issue 1: Error Accumulation

PID at this level is obviously never perfect, the imperfect conditions, the lack of additionally sensing, and physical of the make of the robot are obvious concerns. The run straight method can causes error accumulation that affects the turn angle which can cause error accumulation. In our final run, we were able to avoid this mostly and fortunately, but the intermediate runs we show include some physical adjustments from our side. These were later mostly eliminated by improved tuning during the final run.

### Issue 2: Bluetooth

This has been an issue for us for essentially the entire semester. The struggle for my computer to sustain a prolonged bluetooth connection has been a losing one form my side and I was aware that it would be yet another uphill battle on this lab. We tried many solutions: adding random commands to keep the loop busy, making the robot go faster, and trying to shorten the trajectory. Keeping the loop busy never really worked since overtime it just cause a lot more interference to occur and messed with the current commands. Making the robot go faster further worsened our accuracy and jeopardized the entire trajectory and our goal of making the entire journey as hands-free as possible. Finally, shortening the trajectory seemed like an improper way to actually complete this assignment. As such, at the suggestion of the TA's, we were left to find the longest runs we could and combine them if needed. 

### True Result

Due to the bluetooh issue, we opted to find the 2 longest runs we could and analyze each one in depth.

#### Run 1

Run one (the longest of the 2) involved starting at our start point and getting all the way until 5, 3 hands free. However, the trajectory from 5, 3 on to 0,3 constantly fails as the blue tooh connection times out at that one point and the robot proceeds to just crash into the wall. The unsuccessful trial that displays the issue is included below:

This trail was conducted before the proper tuning of the PID turns and straight movement but it goes to emphasize the issue we were facing. The PID seemed fine until the trajectory from 5,3 to 0,3 at which pythong from the Python side, we could see that the bluetooth disconnected and the robot had run loose. Ultimately, our worst fear came to fruition. 

The succeful trial (aka no interference) is included below:

[Actual Run 1](https://www.youtube.com/embed/GLX7j7TNE9s)

THe difference in this trials was the robot was never touched for this first part of the trajectory, yet the robot disconnected at the exact same point. This first stitch was perfect all the way up until the (5,3,0,3) trajectory. The problem was truly out of our control as we had no proper solution to force a bluetooth connection. Checks for if the bluetooth was connected were added to the artemis code and they all confirmed that the bluetooth infact was disconnecting at thaat point due to timing out. 

Aside from the bluetooht issue, however, the acutal execution was extremely smooth. The turns were perfect, the compute control logic of turn, straight, turn actually worked, and any issues that persisted in the angling of the robot were autocorrected due to robust PID. Point (5,-3) was not properly touched but the tile was entered and crossed. 

#### Run 2

Run 2 involved just starting from 5,3 and allowing the robot complete the rest of the trajectory mostly hands free. The completion of this run was designed to prove that the issues discussed in the Run 1 section were not programmatic but weret truly bluetooth issues. The second run; however, did need one physical interference from my side though the error was caused by the imperfect placement of the robot when we begain Run 2. Had we not had to deal with the physical placement and had this been a proper extension of Run 1, no physical intereference would have been necessary. Run 2 was also perfect in its execution and even avoided crashing into another team's robot that was already present at the point -4,-3. The execution of Run 2 is included below:

[Successful Run 2](https://www.youtube.com/embed/jDliG3fiCXA)

#### Potential Result

Obviously, the entire run didn't go as planned due to the bluetooth issue. But the 2 separate runs did prove that our PID logic was robust, the offboard commands were properly being read, and the local path planning was a success. We could only hope that our robot would do the entire run without any issues, but alas, that unfortunately never happened. Nevertheless, after spending 10 straight days, over 45 hours on this task, and an enjoyable, yet tiring night grind from 2 pm to 3:30 am at the lab, we are also including a video of how a near perfect run would have been had the two separate runs above been one after another. [Perfect Run](https://www.youtube.com/embed/vB2Yq-hQNg4)


### Important Notes
There were also obviously imperfections in the actual obstacle layout. At times the boxes were not perfectly in their designated regions and the the walls not properly aligned. The most important imperfection we noticed throughout the entire run was the dust accumulation that cause every turn after a certain point a bit unreliable. However, they were minor imperfections that were never a big hurdle compared to the bluetooth issues we faced. These minor imperfections, though, did play a role in the times where we had to physically interfere with the trajectory.

## Localization

The obvious question so far is probably why no localization? Localization would have made the robot more tolerant to minor error and allows better self correction right? The answers to the previous question was yes it does and I too wish we could have done it. However, two issues did arise that we noticed:

### Issue 1: Localization errors:
When we ran localization on 0,3 and 5,3, we noticed that there was a relatively large error. The estimation showed that at 5,3 the robot thought that it was 6,2. This is clearly problematic since if localization were to be implemented and if localization did give me a result of 6,2, it would never be properly detect when it it as true 5,3 and if it were to move, it will likely crash into the big box without any obstacle avoidance. Additing obstacle avoidance on top of Localization would result in too complex a script. The result of the faulty localization at that point is included below

### Issue 2: Bluetooth yet again
While localization is relatively poor for our robot, it's a much less signficant hurdle than our bluetooth issues. Each localization for us takes approximately 3o to 40 seconds to complete. When we tried to do a few localization continuously, it seemed that the bluetooth disconnected yet again. So, using this logic, localization would have likely caused the entire script to timeout and the robot may not have even got out of the first 2 or 3 points.

Another question maybe why not do selective localization? In other words, why not only do localization in certain points. This was considered initially; however, we realized that selective localization is a fundamentally flawed concept. Localization introduces error, and to fix the error, yet another localization is necessary. As such, using this chain of thought, using localization at certain points introduces error only additional localizations can solve and thus, the entire trajectory must be localization to properly work. This ultimately creates a localization only or nothing situation

## Conclusion
There were many issues that this lab introduced, but the end result was all worth the effort. A special thanks to Professor Petersen to help walk me through some trajectory logic, Vivek for helping us debug our distance sensor issues, Jonathan for helping us try potential bluetooth issue soluttions (though they did not seem to work in our case), Jade for keeping us motivated despite the continous failed trials, and Aratrika for keeping the lab open for longer than intended on Saturday to finish our testing. 4960 was a great learning experience and this lab specifically was great! 
