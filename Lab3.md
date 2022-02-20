## LAB 3

### Lab 3

**Write Up:** 
Lab 3 considered of four stages: connection layout, hardware assembly and soldering, IMU and TOF configuration + preliminary testing, and IMU and TOF fine-tuning.

The connection layout process involved designing a wiring scheme to connect the 2 TOF's and IMU to the artemis board. In order to provide the same voltage to each sensor, the connections were set in parallel, splitting the current from QWIC connector amongst the three devices. Using the data sheet, the QWIC connector pin order was determined and the SDA and SCL were planned to be chained amongst all three sensors, resulting in all three sensors sharing a single SDA and SCL line. To manage the 2 TOF's (since they have the same address), the XSHUT pin of one of the TOF's was schemed to be wired the digital pin number 4 on the artemis.

The hardware and soldering stage involved several steps. The first step was to figure out a way to chain the various sensors in parallel. The solution invovled soldiering two wires together and outletting on of the wires to the current sensor while the other heads to the next sensor. The order followed was Artemis QWIC -> TOF_1 -> IMU -> TOF_2. Additionally, a wire stretching from digital pin 4 to the XSHUT pin of TOF_1. 

For the configuration step, the address of the single TOF was read first using the WIRE_I2C example provided. The address read was 0x29 while the data sheet provided address was 0x52. This occurs because...... The Wire_I2C.cpp file was later abondened due to the ainability to read the address of more than two devices. To test the effectiveness of a single TOF, the _Example1_Read Distance_ file. To test the IMU, the _Example1_Basics_ file was utilized. The example file for the TOF worked by activating the I2C wire, creating a TOF object, beginning ranging, storing the recorded distance, and stopping ranging. The IMU example code worked in a relatively similar fashion where the I2C wire is activated, the sensor variable is created, and values aare read using provided library methods. The primary point of importance here is the ADD_VAL macro in the IMU exampl.e Sinc3 the ADR_JUMPER is wired on the artemis, the value is set to 0. 

For the actual execution of the tasks, firstly, the various distance modes were experimented with. As suggested by the methods name, the short mode for the TOF worked very well with very small distances; however, there was  a noticeable lack of stability when trying to detect longer distances. The inverse was true for the long mode. Since the robot is likely to run within a relatively confined space, the short mode would be more effective. This would allow sensing the distance from potential walls or barriers more accurately and effectively. In order to chane the address of a second TOF, _setI2CAddress()_ method was utilized. THe change is documented here: 

<img width="292" alt="image" src="https://user-images.githubusercontent.com/23284665/154853168-0421b00f-162b-4b35-826a-b6d9c55f3b92.png">

The code above shuts down TOF_1 and changes its address to 0x49 which is not an already occupied device address. 

Secondly, the IMU task involved several steps. In order to determine the cutoff frequency and thus the  alpha for complimentaary filters, taapping along the pitch and roll axes were used. The base pitch was calculated using the artctan2 of (accX/accZ), and the base roll was calculated using atan2 of a_y/a_z. By performing FFT data analysis on the base pitch and roll resulted in signficant noise for the roll and minimal noise for the pitch. The sampling frequency as noticed from time tracking was 8ms and thus with the maximal possible bandwidth being f_s/2 = 1/0.008/2 = 62.5 Hz. The taps were approximately timed for every seconds alternating in each direction. The pitch and roll taps and tests were done seperately.

Then initial results are included in the pictures below: 

<img width="401" alt="Screen Shot 2022-02-19 at 2 06 00 PM" src="https://user-images.githubusercontent.com/23284665/154846954-86c6a1a9-d2b4-454a-a293-246d754ea46b.png">
<img width="401" alt="Screen Shot 2022-02-19 at 2 06 20 PM" src="https://user-images.githubusercontent.com/23284665/154846963-865e4a5c-47b0-458d-a305-7f328bf8d90e.png">

<img width="428" alt="Screen Shot 2022-02-19 at 2 17 00 PM" src="https://user-images.githubusercontent.com/23284665/154846974-76bb5ad1-cd90-4ae4-9e26-5a2f157efcee.png">
<img width="391" alt="Screen Shot 2022-02-19 at 2 17 08 PM" src="https://user-images.githubusercontent.com/23284665/154846980-87153f53-5d3c-4841-abbc-5ae11b1e4f8e.png">


Since the Pitch has extremely minimal noise, the cutoff frequency considered was approximately 62 Hz. Using this cutoff frequency, the alpha determined was 0.76. Since the noise is extremely high, the cutoff frequency considered was aapproximately 35 Hz to cutoff a second wave of spikes starting after 35 Hz. The determined alpha was 0.637.


For the gyroscope, the values were recorded much more easily since calculations were minimal. The dt utilized was 8ms since that waas the sampling period. The initial data included a signficantly amount of drift. Unfortunately, this result could not be recorded since the Serial plotted does not work on the MAC. To avoid this drift and attain stability with minimal noise, a complementary filter with both the gyroscope reading and the accelerometer reading. Various alphas were tested, and the alphaa that minimized drift and noise was determined to be 0.75 for pitch and 0.7 for roll. The resulting FFT graphs from a repeated tap experiment are included below:

<img width="304" alt="image" src="https://user-images.githubusercontent.com/23284665/154847813-b7c0de26-65ae-42b1-bca3-191f152c09e2.png">

<img width="421" alt="Screen Shot 2022-02-19 at 2 59 11 PM" src="https://user-images.githubusercontent.com/23284665/154847827-76271712-7fe7-4521-b0e9-5cad11b8b4f4.png">

The drift was also noticed to be signficantly lower though still present. 

The amendments to the existing IMU:

<img width="250" alt="image" src="https://user-images.githubusercontent.com/23284665/154853248-1da07959-c68a-4fc0-94a1-dac39ed95091.png">

<img width="809" alt="image" src="https://user-images.githubusercontent.com/23284665/154853280-ea326277-ca23-4503-9fa0-73387955068b.png">

<img width="544" alt="image" src="https://user-images.githubusercontent.com/23284665/154853294-0da2eae7-4094-4979-a6fb-8c5f3d4fe3c2.png">

<img width="421" alt="Screen Shot 2022-02-19 at 2 59 11 PM" src="https://user-images.githubusercontent.com/23284665/154847827-76271712-7fe7-4521-b0e9-5cad11b8b4f4.png">
