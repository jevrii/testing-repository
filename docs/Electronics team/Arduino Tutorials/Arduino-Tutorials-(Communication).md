## Objectives
At the end of this self-learning lab, you should be able to:
* set up serial communication in Arduino
* set up communication between Arduino and the computer via pySerial

## Serial Communication

Serial communication, as its name tells, send data between devices bit by bit, in a serial way. The opposite of serial communication is parallel communication. Serial communication is commonly used nowadays. For example, USB device communicates with the computer using serial communication.

Actually, you have been using Serial communication in the previous Arduino tutorials. In Arduino, the statement Serial.begin() sets up serial communication between the Arduino board and the computer over USB.

In Blue Pill, besides Serial over USB, there are also 3 additional USART ports (`USART1` to `USART3`) that can be used for serial communication. They are `Serial1` to `Serial3` in Arduino IDE. ([API - STM32duino wiki](https://wiki.stm32duino.com/index.php?title=API#Serial_.26_USB_Serial))

[As seen from there pin out of Blue Pill](/m2robocon/m2_wiki/raw/master/images/(Arduino_Tutorials)_Blue_pill_pinout.gif):
* `USART1` occupies `PA9` and `PA10` (TX1 and RX1)
* `USART1` occupies `PA2` and `PA3` (TX2 and RX2)
* `USART1` occupies `PB10` and `PB11` (TX3 and RX3)

When program is uploaded using USB bootloader or ST-Link:
* `Serial` prints to Serial USB.
* `Serial1` prints to hardware `USART1`
* `Serial2` prints to hardware `USART2`
* `Serial3` prints to hardware `USART3`

When using USB-Serial adapter:
* `Serial` prints to hardware `USART1`
* `Serial1` prints to hardware `USART2`
* `Serial2` prints to hardware `USART3`

Read more about serial communication from the following websites:
* [Serial communication](https://learn.sparkfun.com/tutorials/serial-communication)
* [Serial Library in Arduino](https://www.arduino.cc/reference/en/language/functions/communication/serial/)
* [Serial communication between 2 Arduino board](https://www.hackster.io/harshmangukiya/serial-communication-between-two-arduino-boards-d423e9)

### Try it yourself 01
**Chinese whispers**

Grab 2 Blue pills. Flash a program to a blue pill which print `Hello, World!` continuously over `Serial1`. They should be connected like below. Then click `File` → `Examples` → `04.Communications` → `SerialPassthrough` and flash it on one of the blue pills. This program put all data received from `Serial1` to `Serial` and vice versa. Connect `PA9` on one blue pill to `PA10` on the other and vice versa. (When using USB-Serial adapter, use `USART2` i.e. connect `PA2` to `PA3` and vice versa)

![Serial Communication](https://github.com/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Serial_Communication.png)

Now open Serial Monitor and you should see `Hello, World!` is printed continuously from the second blue pill.

## pySerial

Arduino IDE provides convenient way for showing serial communication between the Arduino boards and the Computer. Just a click on the Serial Monitor in Arduino IDE will do. However, it does not allow the computer to react according to the value received. No worry, you can write your own Serial Monitor using Python! pySerial is a Library which allows python programs to access the serial port. Thus, we can now bridge Arduino board and the computer by ourselves.

If you haven't tried pySerial or want to further familiarize yourself with Python:
* (Windows) Check [Python Code | Arduino Lesson 17. Email Sending Movement Detector | Adafruit Learning System](https://learn.adafruit.com/arduino-lesson-17-email-sending-movement-detector/installing-python-and-pyserial).

* (Linux) Python and pySerial can be easily installed through `python-pip`. Take Ubuntu as an example.
    ```
    sudo apt install python python-pip
    pip install pyserial
    ```

Read more about Arduino and Python from the following websites.
* [Arduino and Python](https://playground.arduino.cc/Interfacing/Python)
* [Using PySerial to read data from Arduino](https://engineersportal.com/blog/2018/2/25/python-datalogger-reading-the-serial-output-from-arduino-to-analyze-data-using-pyserial)

### Try it yourself 02
**Take extra precautions while working at heights**

Flash the following program to a Blue Pill:

```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
}

void loop() {
  if (Serial.available()) {      // If anything comes in Serial (USB),
    Serial.write(Serial.read());   // read it and send it out Serial1 (pins 0 & 1)
  }
}
```

This should pass all data sent from PC back to itself. Open Serial Monitor to see if it works.

* (Linux)

    Copy the following 2 programs as 2 separate files. change the line `ser = serial.Serial('/dev/ttyACM0', 9600)` according to the port name of the blue pill in your OS. It should be something like `/dev/ttyACM0`.

    ```python
    # Listener: read char from USB Serial
    import serial
    ser = serial.Serial('/dev/ttyACM0', 9600) # port name
    while True:
	    print ser.read()
    ```
    
    ```python
    # Talker: send char through USB Serial
    import serial
    ser = serial.Serial('/dev/ttyACM0', 9600) # port name
    while True:
	    s = raw_input("Enter your input: ")
	    ser.write(s)
    ```

    Run the 2 programs together. Python programs can be executed with:
    ```
    	python (filename)
    ```

* (Windows)

    A serial port cannot be opened twice on Windows. Therefore, we have to use a multi-thread version. Copy the following program and save it as a  file. change the line `ser = serial.Serial('COM3', 9600)` according to the port name of the blue pill in your OS. It should be something like `COM4`. 

    ```python
    import serial
    import threading
    
    class thread_read_serial (threading.Thread): # read from the blue pill
	    def run(self):
    		while True:
    			print ser.read(),
    
    class thread_write_serial (threading.Thread): # write to the blue pill
    	def run(self):
    		while True:
    			s = raw_input("Enter your input: ")
	    		ser.write(s)
    
    thread1 = thread_read_serial()
    thread2 = thread_write_serial()
    
    ser = serial.Serial('COM3', 9600) # port name
    
    thread1.start()
    thread2.start()
    ```

    Run the program. Python programs can be executed with:
    ```
    	python (filename)
    ```

    IDLE can't handle `raw_input()` correctly. Use command prompt instead.

Words sent from the talker should be seen on the listener. Words have actually passed to the blue pill and back to PC through USB Serial.

## Assignment 04
**You accidentally made a servo motor again**

Remember the motor with speed feedback you have wrote in [Assignment 02](/m2robocon/m2_wiki/wiki/Arduino-Tutorials-(Learning-basic-control)#assignment-02)? Modify the Arduino program and write Python programs so that you can command the speed of the motor and read the RPM of the motor on your PC.

You may refer to:
* [The Python Standard Library](https://docs.python.org/2/library/index.html)
* [pySerial API - pySerial 3.0 documentation](https://pythonhosted.org/pyserial/pyserial_api.html)
* [Short introduction - pySerial 3.0 documentation](https://pythonhosted.org/pyserial/shortintro.html)

## Further reading
* [USB CDC class - Wikipedia](https://en.wikipedia.org/wiki/USB_communications_device_class)
* [WsIaR? Communication!](https://github.com/m2robocon/m2_wiki/wiki/WsIaR%3F-Communication!)

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))