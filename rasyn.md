Table of content
1.	Foreword
2.	Board Description
3.	Performance
4.	Neural Engine
5.	Wireless connectivity
6.	Processing Speed
7.	Serial Connection
8.	Storage
9.	Demos
10.	Creating a thing in AWS IOT portal
11.	Modifying the configuration file
12.	Testing out premade model
13.	Monitoring the inference result
14.	Pros
15.	Cons
16.	Conclusion
### Foreward
I would like to apologize for the delayed submission of the roadtest. 
### Board Description
Usage of Renesas controller [RA6M4](https://www.renesas.com/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus/ra6m4-200mhz-arm-cortex-m33-trustzone-high-integration-ethernet-and-octaspi) is an odd choice considering the lack of resources to learn how to program Renesas microcontrollers. Luckily we don't need to as we can modify the template application provided by the Avnet.
The audio data is collected through [TDK T5838](https://invensense.tdk.com/products/digital/t5838/) and the motion data is acquired through IMU TDK [ICM-42670-P](https://invensense.tdk.com/products/motion-tracking/6-axis/icm-42670-p/)
The heavy lift task of processing the data and producing the inference result is assigned to [NDP120](https://www.syntiant.com/ndp120) which is also present in [Nicla](https://store-usa.arduino.cc/products/nicla-voice). 
[DA1660](https://www.renesas.com/en/products/wireless-connectivity/wi-fi/low-power-wi-fi/da16600mod-ultra-low-power-wi-fi-bluetooth-low-energy-combo-modules-battery-powered-iot-devices) acts the WiFi/Ble modem for the board connected using UART lines.

### Performance
The Renesas controller does a good job of co-ordinating with NDP, sensors, wireless module and SDcard. Data transmission between these nodes happen seamlessly. Log can be viewed either through the UART line or though USB C connection.

### Wireless connectivity
The Renesas DA16600MOD is an ultra-low-power combo module integrating Wi-Fi and Bluetooth Low Energy (BLE) for battery-powered IoT devices. It features Renesas' VirtualZero™ technology for minimal power usage in sleep mode, supports year-plus battery life, and includes built-in Wi-Fi and BLE SoCs, 4MB Flash memory, and various timing components.

### Serial Connection

### Neural Engine
The Syntiant NDP120 Neural Decision Processor is a specialized chip for always-on applications in battery-powered devices, featuring the Syntiant Core 2 neural processor for running multiple Deep Neural Networks (DNN) with 25x the tensor throughput of its predecessor

### Modifying the configuration file
Select the Mode as 1 to enable the audio keyword matching models.<br>
```
Mode=1
```
Set the debug port as per your wish. (1 - UART 2- USB)<br>
```
Port=1 
```
Set the below options to 0 or 1 to disable or enable them<br>
```
Write_to_file=0        # write data to a *.csv file on the sdcard
Print_to_terminal=0    # output data to the serial debug terminal
```
Add your wifi configurations here<br>
```
Access_Point=abcdxyz
Access_Point_Password=12345678
```
Set the value to 1 to use the above configured password<br>
```
Use_Config_AP_Details=1
```
Since we are going to use AWS as our cloud provider, I set the value as 2. (0=No Cloud Connectivity, #  1=Avnet's IoTConnect on AWS,#  2=AWS)<br>
```
Target_Cloud=2
```
Enter the endpoint of MQTT server, pub/sub topic names and device name. For more details [click here](https://github.com/Avnet/RASynBoard-Out-of-Box-Demo/blob/rasynboard_v2_tiny/docs/AWSIoTCore.md)<br>
```
Endpoint=abcdefgh-abc.iot.ap-locale-n.amazonaws.com
Device_Unique_ID=rasynboard
MQTT_Pub_Topic=publish
MQTT_Sub_Topic=subscribe
```

### Testing out premade model

### Monitoring the inference result
You can observe the inference result in AWS IoT management console using this link. Make sure to replace the region with your region. <br>
```
https://ap-luck-n.console.aws.amazon.com/iot/home?region=ap-luck-n#/dashboard
```
