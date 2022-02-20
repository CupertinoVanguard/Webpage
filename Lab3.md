## LAB 3

### Lab 3

**Write Up:** 
Lab 3 considered of four stages: connection layout, hardware assembly and soldering, IMU and TOF configuration + preliminary testing, and IMU and TOF fine-tuning.

The connection layout process involved designing a wiring scheme to connect the 2 TOF's and IMU to the artemis board. In order to provide the same voltage to each sensor, the connections were set in parallel, splitting the current from QWIC connector amongst the three devices. Using the data sheet, the QWIC connector pin order was determined and the SDA and SCL were planned to be chained amongst all three sensors, resulting in all three sensors sharing a single SDA and SCL line. To manage the 2 TOF's (since they have the same address), the XSHUT pin of one of the TOF's was schemed to be wired the digital pin number 4 on the artemis.

The hardware and soldering stage involved several steps. The first step was to figure out a way to chain the various sensors in parallel. The solution invovled soldiering two wires together and outletting on of the wires to the current sensor while the other heads to the next sensor. The order followed was Artemis QWIC -> TOF_1 -> IMU -> TOF_2. Additionally, a wire stretching from digital pin 4 to the XSHUT pin of TOF_1. 

For the configuration step, the address of the single TOF was read first using the WIRE_I2C example provided. The address read was 0x29 while the data sheet provided address was 0x52. This occurs because...... The Wire_I2C.cpp file was later abondened due to the ainability to read the address of more than two devices. To test the effectiveness of a single TOF, the _Example1_Read Distance_ file. To test the IMU, the _Example1_Basics_ file was utilized. The example file for the TOF worked by activating the I2C wire, creating a TOF object, beginning ranging, storing the recorded distance, and stopping ranging. The IMU example code worked in a relatively similar fashion where the I2C wire is activated, the sensor variable is created, and values aare read using provided library methods. The primary point of importance here is the ADD_VAL macro in the IMU exampl.e Sinc3 the ADR_JUMPER is wired on the artemis, the value is set to 0. 

For the actual execution of th etakss, firstly, the various distance modes were experimented with. As suggested by the methods name, the short mode for the TOF worked very well with very small distances; however, there was  a noticeable lack of stability when trying to detect longer distances. The inverse was true for the long mode. Since the robot is likely to run within a relatively confined space, the short mode would be more effective. This would allow sensing the distance from potential walls or barriers more accurately and effectively. 

Secondly, the IMU task involved several steps. In order to determine the cutoff frequency and thus the  alpha for complimentaary filters, taapping along the pitch and roll axes were used. The base pitch was calculated using the artctan2 of (accX/accZ), and the base roll was calculated using atan2 of a_y/a_z. By performing FFT data analysis on the base pitch and roll resulted in signficant noise for the roll and minimal noise for the pitch. The sampling frequency as noticed from time tracking was 8ms and thus with the maximal possible bandwidth being f_s/2 = 1/0.008/2 = 62.5 Hz. The taps were approximately timed for every seconds alternating in each direction. The pitch and roll taps and tests were done seperately.

Then initial results are included in the pictures below: 

<img width="401" alt="Screen Shot 2022-02-19 at 2 06 00 PM" src="https://user-images.githubusercontent.com/23284665/154846954-86c6a1a9-d2b4-454a-a293-246d754ea46b.png">
<img width="401" alt="Screen Shot 2022-02-19 at 2 06 20 PM" src="https://user-images.githubusercontent.com/23284665/154846963-865e4a5c-47b0-458d-a305-7f328bf8d90e.png">

<img width="428" alt="Screen Shot 2022-02-19 at 2 17 00 PM" src="https://user-images.githubusercontent.com/23284665/154846974-76bb5ad1-cd90-4ae4-9e26-5a2f157efcee.png">
<img width="391" alt="Screen Shot 2022-02-19 at 2 17 08 PM" src="https://user-images.githubusercontent.com/23284665/154846980-87153f53-5d3c-4841-abbc-5ae11b1e4f8e.png">




