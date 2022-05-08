
# Lab 12

Lab 12 involved utilizing a version of the mapping code developed in lab 9 and fusing it with logic developed from lab 11. 

## Turning & Data Collection:

The turning in lab 9 involved turning at the desired rotation speed. As such, no particular angular control was provided and all stops came from the jupyter notebook side rather than the Arduino side. Now the code has been remodeled to only take readings every 20 seconds and is promptly stopped when the measured angle is between 350 and 370 while still moving at a theoretical speed of 35 degrees per second. 

To aid this movement, the PID process from lab 9 is still utilized. Please refer to lab 9 for my extensive discussion about how PID with turning works. The primary logic added during this lab was the boolean checks of the 20 degree increments and the 360 degree stop. A picture of these additions is seen below:


As can be seen above, the program checks whether the robot is at various multiples of 20 degrees. At each of these steps, the distance reading is taken and the angle is recorded as well for reference. In addition to taking the recordings at teach of these angular steps, logic was added additionally to stop the robot the robot at near 360 by zeroing the flag so that in the next call of the bluetooth while loop would recognize that the turning process should be stopped. At the stop, the data that is required to be sent is prepared and written to the appropriate UUID. The data reading step from the jupyter lab is also detailed in Lab 9. 

The result of the spin video is included in this link. As can be seen the robot turns from 0 to 340~350 degrees and stops and takes readings every 20 degrees. 


## Incorporation with Python:
The python incorporation involved testing in two steps. The first step involved simply testing based on consecutive command calls. The second involved porting all the prior command calls into one condensed function.

### Process 1:

First a separate command was created to execute the method described prior called OBS_DATA. The command promptly executes the turn, takes data at the appropriate 20 degree intervals, and stops at near 360. Once stopped, the robot sends all the data (distances and the angles) back to python.

During the process of data collection, two major issues were noticed. The first issue noticed was the repetition of certain angles with minimal decimal differences between each reading. This likely occurred due to modulo round-down in the artemis script included in the prior section. However, fortunately, and expectedly, the distance readings taken were largely similar, if not equal, in all these instances. As such, to combat this issue, a simple python script was included to only include one instance of each angle (0, 20, 40, 80…320, 340) and the average distance readings from the existing repetitions. The second issue revolved around the time required to properly complete the spin and send the appropriate data back. Since the command was constructed to handle the physical spin task and data collection, enough time was needed to pass before data could be properly read. This issue was first noticed when a print statement immediately after the command call was included, and the expected populated array was, in fact, empty. To resolve this issue, async delays were added. The async delays allowed provide enough time for the command to both execute and send data back. To provide enough headroom, the async delay was set to be approximately 60 seconds during the testing phase. However, through experimentation, the delay was cut down to eventually be 50 seconds since not all 1000 values being sent back by the Artemis were required. 

### Process 2:

This part of the process involved taking the code from the first process and appropriately moving it into the lab 12 file. The first step was to make all calls by or to perform_observation_loop method an async function to accommodate the await call within the function. This involved turning the get_observation_method in the Base Localization class an aysnc function. Within the perform_observation_loop method, the command call_command_obs method was first called which internally calls the OBS_DATA command to turn and return the data. Within this command, an async delay was added as mentioned prior to allow the turn and data collection to occur without an interruption. Additionally, within the class, the call back handler and few data processing methods were incorporated as well. Once the data had been stored, the values were passed into the data processing methods, converted to a column array using numpy methods, and returned (both distance in meters and the angles were returned). The biggest challenge programmatically faced in this task was having to declare a separate global variable tof_turn to allow the callback handler to execute the notification updates and for the perform_observation_loop method to access. 


## Results:

The localization script was run in four different locations. At each of these locations, a near 360 degree, and return the processed distance values (in meters) to be assigned to the obs_range_data variable of the base localization class. The data is then used in the update step to update the belief throughout the grid. In the prints of the results seen in the images below, the location (in meters). An important point of notice is the consistently high probabilities even for the “wrong” localization results. This implies that the measurements taken at those spots seemed to closely resemble that of another spot’s expected measurements and thus localizes at that “wrong” position with high probability. More interestingly, the wrong localization results seem to be off by a foot in both the x and y directions. This indicates errors during the imperfect inplace rotation where the robot may have moved to different tiles in both the x and y direction, thus picking up measurements that match with the foot translations in both directions. 

