## LAB 3

### Lab 3

**Write Up:** 
Lab 3 considered of four stages: connection layout, hardware assembly and soldering, IMU and TOF configuration + preliminary testing, and IMU and TOF fine-tuning.

The connection layout process involved designing a wiring scheme to connect the 2 TOF's and IMU to the artemis board. In order to provide the same voltage to each sensor, the connections were set in parallel, splitting the current from QWIC connector amongst the three devices. Using the data sheet, the QWIC connector pin order was determined and the SDA and SCL were planned to be chained amongst all three sensors, resulting in all three sensors sharing a single SDA and SCL line. To manage the 2 TOF's (since they have the same address), the XSHUT pin of one of the TOF's was schemed to be wired the digital pin number 4 on the artemis.

The hardware and soldering stage involved several steps. The first step was to figure out a way to chain the various sensors in parallel. The solution invovled soldiering two wires together and outletting on of the wires to the current sensor while the other heads to the next sensor. The order followed was Artemis QWIC -> TOF_1 -> IMU -> TOF_2. Additionally, a wire stretching from digital pin 4 to the XSHUT pin of TOF_1. 

For the configuration step, the address of the single TOF was read first using the WIRE_I2C example provided. The address read was 0x29 while the data sheet provided address was 0x52. This occurs because...... The Wire_I2C.cpp file was later abondened due to the ainability to read the address of more than two devices. To test the effectiveness of a single TOF, the _Example1_Read Distance_ file. To test the IMU, the _Example1_Basics_ file was utilized. The example file for the TOF worked by activating the I2C wire, creating a TOF object, beginning ranging, storing the recorded distance, and stopping ranging. 
