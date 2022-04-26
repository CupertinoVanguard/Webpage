## LAB 11

### Lab 11

**Write Up:** 

This lab involved utilizing Vivek’s simulator (Thanks Vivek!) and getting more experience with the various commands within the environment. 

**Various Methods**

**Compute Control method**: The compute control method works by find the rotation angles necessary to achieve the proper orientation and the translation needed to move to the second position. There are two primary angles that were computed: delta_rot_1 & delta_rot_2. Delta_rot_1 is computed to be the angle required to move into the proper angle to achieve the translation. Delta_rot_2 is computed to be the angle to move into the desired orientation once the translation has been achieved. The translation is calculated from the x & y displacements based on the Pythagorean theorem. The code is included below:
![image](https://user-images.githubusercontent.com/23284665/165348769-8c22c2be-8be1-45b3-9de1-fcb0e2adcb7d.png)

**Odometry Motion Model**: The idea behind this method is to find the probability that a particular motion u occurred given that we have access to what motion actually occurred based on the poses provided and the sensor noise. In order to calculate this conditional probability, the individual conditional probabilities of the various compute control results had to be multiplied together. This utilizes the gaussian function from the BaseLocalization class with the means of the angle inputs being 0 to avoid normalization issues and the comparison value being the normalized error with the normalize_angle function. The angle must be normalized since without such normalization true marginal differences can be amplified to be faulty larger differences. The code is included below:![image](https://user-images.githubusercontent.com/23284665/165348966-698658a2-e1fb-4121-9009-c0e4dc277439.png)


**The prediction step**: The main concept behind this prediction step is working with the idea that all input start points and end start points are probabilistic. As such, the entire grid’s predicted probabilities must be updated. To achieve this, every possible start point and every possible endpoint must be looped through (3 + 3). This results in 6 for loops. The first step involved getting the actual movement based on the odometry. Considering each start,end pose combinations, the odometry motion model is called with the first two inputs being the current odometry and previous pose and the third input being the motion model required to move from start, end combination being considered. This result is then multiplied by the belief associated with the current start point grid cell. The result is then assigned to the bel_bar (the prediction probability) to the end point grid cell.  

The logic behind this is to see if the motion associated with the (start,end) is likely to have occurred knowing the true (start,end) poses. In the final step, the bel_bar is normalized using the np.sum method and before going through the 6 for loops, the bel_bar is zeroed to avoid accumulation over previous estimations. The resulting code is included below:
![image](https://user-images.githubusercontent.com/23284665/165349051-f47a2c7e-48ac-4582-a4e2-a18c9b6d3dee.png)


**Sensor Model**: The sensor model works by doing a conditional probability between the actual measurements based on the expected measurement and the measurement noise. This is done for each of the 18 measurements, and all are multiplied together to get the net probability. The code is included below:
![image](https://user-images.githubusercontent.com/23284665/165349257-4d116d86-ced6-453a-9623-dd679f55f29e.png)


**Update Step**: This represents the final step. It goes through each of the grid cells in the 3 dimensional space and computes the associated belief based on the stored bel_bar and the results of the measurements. Similar to the bel_bar prediction step, the beliefs are also normalized. The code is included below: ![image](https://user-images.githubusercontent.com/23284665/165349304-63993da0-cf36-421a-9399-bbd23e82bca5.png)



**Results**

The results of the movement is included in the following [Results](https://youtu.be/1g1w67ZkL6I).

The individual iterations and the results of the prediction and update steps are included below:
![image](https://user-images.githubusercontent.com/23284665/165350319-f960eccf-9a41-4838-827d-97a8cd0a966d.png)
![image](https://user-images.githubusercontent.com/23284665/165350379-62a47d60-6699-4015-8944-308f90879893.png)
![image](https://user-images.githubusercontent.com/23284665/165350426-242520d4-6edc-4e94-ac26-bd644274bb80.png)
![image](https://user-images.githubusercontent.com/23284665/165350478-a31121c1-6237-4705-9357-159792307adf.png)
![image](https://user-images.githubusercontent.com/23284665/165350540-db618b09-5339-4c16-99d7-0861a1873345.png)
![image](https://user-images.githubusercontent.com/23284665/165350594-b6bb54d4-113f-4037-8d7f-e921b0c39dd4.png)
![image](https://user-images.githubusercontent.com/23284665/165350647-c19e3211-5a5c-4714-b56d-dcca962e6c13.png)
![image](https://user-images.githubusercontent.com/23284665/165350705-c67d87fa-b0eb-4a62-ad54-a57ae6dc2253.png)

The final run of the result is included here: ![image](https://user-images.githubusercontent.com/23284665/165351709-8d531145-3c92-45b6-b8eb-de5afd3dbab5.png)


As can be seen in the video, the blue line follows extremely closely to the ground truth trajectory which is repreasented in green. The most insteresting aspect of the overall plot seems to be the discrepancies of the belief and the ground truth during sharp turns in the trajectory. During the ground truth angular portions,the belief seems to follow straight lines, leading to less smooth turns from the Bayes Filter. As usual the odometry is extremely different than the ground truth. 

Looking at the individual iterations, a clear patter can be noticed. The predicted location in the 3 dimensional grid consistently improves over several iterations, but does become more different during the aforementioned sharp turn points. Another major observation is that the probability of the predicted locations never eclipses 20%, but once incorporating the observations, the probability and trust in the position jumpts to near 98%. This indicates significant trust in the observation. However, when running this program in real time, this trust will not perfectly map over since there is a lot more noise which will not be perfectly uniform. 

**Run Time**

The run time without any optimization is approximately 42 minutes with each iteration taking approximately 2 to 3 minutes. In order to reduce the runtime, an optimization was added in the prediction function, where if the start point probability is less than 0.001, the point is ignored to save some complexity. The resulting run time then becomes under 2 minutes

d
