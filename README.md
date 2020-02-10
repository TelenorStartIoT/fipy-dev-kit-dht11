# Connecting DHT11 sensor for FiPy

In this tutorial we will guide you through how to connect a DHT11 temperature and humidity sensor to your FiPy device. 

You will learn how to:

   * Connect the sensor to your device
   * Change the code on the FiPy to read real sensor data
   
This tutorial builds directly on the previous FiPy tutorials for [MQTT](https://github.com/TelenorStartIoT/tutorials/tree/master/02-fipy-mqtt) and [CoAP](https://github.com/TelenorStartIoT/tutorials/tree/master/01-fipy-coap), and will focus on how to implement the sensor code in to the main.py file of the previous tutorials. 


## 1. How to connect the sensor to FiPy device

**Important:** Disconnect your FiPy device from power before connecting the sensor.

This chapter will show you how to connect the **'DHT11 temperature and humidty sensor'** to your fipy device.

 ![dht11_sensor](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/00-DHT11_FipySensor.jpg)

### 1.1 Attaching the sensor to your fipy device.

The sensor comes with a cable consisting of three jumper wires with a plug in each end. The plug on one side goes into the socket on the sensor and the other plug needs to be extended by with **m/m jumper wires** to fit onto your device's expansion board. 

![jumper_wire](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/01-MTM-Jumperwire.jpg)

It is important that you keep track of which wire is which in order to connect your sensor correctly.

![sensor_pin](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/02-Sensor_pin.jpg)

The black **GND** is the grounding and goes in to the **GND** pin socket on your expansion borad. 
The red **3V3** is the voltage and goes in to the **3V3** pin socket on your expansion board.
The green **Data** wire can go in several of the sockets, but in this tutorial we are using the socket marked with **P3** on the expansion board.

Once you have connected the sensor to your device it should look like the picture below.

![connected_device](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/03-Sensor-connected-device.jpg)


**Important:**
Make sure you have connected the pins correctly before reconnecting your device to power. If they are placed incorrectly it may cause damage to the sensor and your board. 


## 2 Programing device to measure temperature and humidity

Now it's a time to start programming the dev-kit to measure temperature and humidity.

### 2.1 Download and unzip the Example Code

You can download the example code from [here](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11).

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/Sensor_download_ZIP.PNG)

Unzip the folder. How to do this varies depending on your computer's operative system. Most systems will unzip if you double click on the zip folder.

### 2.2 Integrate the sensor code into your existing project

In the folder you just downloaded there is another folder called *"example_code"*. Within this you will find *"lib"* and *"main"*. 

**Step 1**

The first thing you should do it to open *"lib"* and copy the only file you find there, called ***dht***. Now navigate to the folder that contain your project code from the previous tutorial. Find the *"lib"* folder in your project and paste the *dht* file. Now you have a library file supporting the sensor in your project code. 

Open your project in VSCode. If you did step 1 right your *lib* folder should contain *dht.py* in addition to *telenor.py* and *mqtt.py* like the picture below.

![Library](https://github.com/TelenorStartIoT/fipy-dev-kit-dht11/blob/master/assets/Sensor_lib_vscode.png)

**Step 2**

The second thing you need to do is to integrate the sensor code into your project's main.py file. In VSCode, open your main.py file so you can write in it. Below the *"import"* functions in line 1-5 write this to import the functions we need to read from the sensor:

```python 
import pycom 
from machine import Pin
from dth import DTH
```

Below this you should write the code that defines the led light on the FiPy and the sensor:

```python
pycom.heartbeat(False)
pycom.rgbled(0x000008)
th = DTH('P3',0)
sleep(2)
```

Lastly we need to replace the code that generates random data and replace it with code for the real sensor. You find this code in line 58 and 59. 

Delete these two lines:

```python
temperature = ((urandom(1)[0] / 256) * 10) + 20
humidity = ((urandom(1)[0] / 256) * 10) + 60
```

And repace them with this:

```python
result = th.read()
temperature = result.temperature
humidity = result.humidity
if result.is_valid():
  pycom.rgbled(0x001000) #green
  print("Temperature: %d C" % result.temperature)
  print("Humidity: %d %%" % result.humidity)
```

Now save (ctrl+s) your project.

### 2.3 Upload and run the Program

Connect your device to your computer (if not already). Make sure that the SIM and LTE antenna is connected! The Pymakr plugin in VSCode will automatically detect the dev-kit.

To upload and run the program on your FiPy, simply click the "Upload" button located at the bottom bar. This will first upload the code, then it will reset your FiPy and run the uploaded code.

### 2.4 Check the Output From the Program

If everything goes well you should see output from the program in the Pycom Console window in VSCode. This should look very similar to the output you got with the randomly generated data. 

Tips and tricks for fault-searching and problem-solving can be found both in our [forum](https://startiot.telenor.com/forums/forum/end-devices/pycom-fipy/) and the previous FiPy dev-kit [tutorials](https://startiot.telenor.com/tutorials/). 



