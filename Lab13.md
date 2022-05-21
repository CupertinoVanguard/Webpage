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

The python implementation proved to be rather long and inefficient many times. The first idea we had was to just list every single command in series and tune it as needed. However, after realizing this was extremely bad programmatically and for debugging purposes, we utilized a class based approach. This approach involved creating a Trajectory class that would represent the individual trajectory. Based on the trajectory points in the format (x1, y1, x2, y2), the class would allow easy computation of the required traanslation in millimeters and turns involved to achieve the translation. A turnback function was utilized in the instances where the orientation of the robot post translation maay not be in the desired end orientationo fo the robot at that second point. With the spirit of modeling our movement afater compute control, we also accounted for the orientation of the robot in the trajectory class by storing an orientation variable. This allowed avoading unnecessary turns and error accumulation when one was n't necessary during a particular trajectory. 
