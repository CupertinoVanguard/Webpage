

### Lab 2

**Write Up:** 

This lab involved experimenting with and better understanding the BLE module onboard the Artemis Nano. The first component of this whole lab involved interfacing with the artemis nano via bluetooth. To retrieve the MAC address of the Nano was attained by running the provided ble_arduino.ino program and _BLE.advertise()_ method. Utilizing the MAC address and cross_checking the UUID's between the connection.yaml file and the ble_arduino.ino file. 

Before experimenting with the tasks, the demo.ipynb was run to make sure the connection between the computer and the artemis works. The demo included sending two integers and displaying on the serial montior and writing PING to receive PONG. The commands utilized to acheive these tasks was _.send_command()_ and _.receive_string_. 

The first task implemented was the ECHO command. In the arduino file, the ECHO command was implemented by first checking if the value could be properly read by artemis from the python command _.send_command(CMD.ECHO, "HiHello")_. If the read is a success, the read value is stored in the transmissing string and a ":)" is appeneded to the end. The transmission string is then written to the UUID for the chracterisitic string. From the python side, the echoed back string is read based on the RX_STRING UUID and displayed using a pring statement. The amended python code and arduino code is included below: 

<img width="667" alt="image" src="https://user-images.githubusercontent.com/23284665/153076907-e071c7de-590f-4d55-bafd-37290bf4622a.png">
<img width="593" alt="image" src="https://user-images.githubusercontent.com/23284665/153076958-dc188a3b-9445-435c-a0c4-b8595c6af92d.png">

The second task implemented was the send three floats command. Within the arduino, the process followed was extremely similar to that of the _SEND_TWO_INTS_. The difference in implementation included changing the values being sent and received being floats. The implmentation in arduino and python are included below:

<img width="756" alt="image" src="https://user-images.githubusercontent.com/23284665/153078110-465e05b5-ed17-4de9-855e-827d229bc9ee.png">
<img width="526" alt="image" src="https://user-images.githubusercontent.com/23284665/153078195-fb762fdb-6599-4b7f-a494-f244a4e1e809.png">

The third task involved writing a notification handler. In the notification handler in python, the uuid and the byte arraay are taken in as parameters. The content of the method involved assigning the a external global value with the float interpretation of the byte array using ble's _bytearry_to_float()_ method. The notification handler then is called within while loop equipped with an async process checker. In order for the async checker within the while loop to "wake up", the start notify method is used with UUID for RX_string and the notification_handler function. The start notify method allows sending notifications only when the particular characteristic specified by the UUID gets updated. This particular methodolgy helps avoid constantly checking for updates. The implmentation of the thrid task is included below:

<img width="1045" alt="image" src="https://user-images.githubusercontent.com/23284665/153080655-f1eb5b4c-2db8-4b95-9279-68da69beb64d.png">

**Answer to question 4 task**:
The difference between both approaches is the receive_float() only allows receiving a single float value while the receive_string() option to receive floats allows to atttain potentially a small list of floats. These floats can be separated by commas. The primary issue with the second approach is the need to perform string analysis to extract the specific float values. Yet, this hassle is outweighed by the simplicity of receiving a list compared to each float individually. 

