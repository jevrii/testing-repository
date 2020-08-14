Through these tutorials, you may learn basic electronics through **Arduino**. 

There are "**Try it Yourself**" and "**Assignment**" in the tutorials that you should follow.

## Basic Tutorials

1. [Getting Started with Arduino IDE](Arduino-Tutorials-(Getting-Started-with-Arduino-IDE))
    * Understand Arduino’s hardware
    * Start using Arduino IDE
    * Write basic Arduino codes
2. [Learning basic programming with Arduino](Arduino-Tutorials-(Learning-basic-programming-with-Arduino))
    * Know the meaning and usage of different data types
    * Know how to use arithmetic operators, comparison operators and boolean operators
    * Know how to use if…else, for, while statements
    * You may skip this if you are familar with C++
3. [Timer, PWM and Interrupts](Arduino-Tutorials-(Timer,-PWM-and-Interrupts))
    * Understand Timer
    * Use PWM
    * Use External Interrupt
    * Use Timer Interrupt
    * **Assignment 01**
4. [Learning Basic Control](Arduino-Tutorials-(Learning-basic-control))
    * Control motor’s rotation speed using PWM
    * Detect the speed of motor by speed sensor
    * Perform a basic feedback control
    * **Assignment 02**
5. [Learning Servo and Sensors](Arduino-Tutorials-(Learning-Servo-and-Sensors))
    * Control servo motors
    * Read analog signal
    * Understand analog signal reading and I2C communication protocol
    * Take reading from different sensors
    * **Assignment 03**
6. [Communication](Arduino-Tutorials-(Communication))
    * Set up serial communication in Arduino
    * Set up communication between Arduino and the computer via pySerial
    * **Assignment 04**

## Beyond Arduino

Arduino IDE is easy to use, but its functionalities are limited and low-level manipulations are impossible with Arduino IDE. If you want to dig deeper on MCU and STM32, you may consider the following:
* [**SW4STM32**](http://www.openstm32.org/System%2BWorkbench%2Bfor%2BSTM32
) is a toolchain to program STM32 with STM32Cube HAL and CMSIS, which can be integrate with Eclipse IDE with a Eclipse plug-in.
    * **STM32Cube HAL** is a hardware abstraction layer (HAL) provided by ST Microelectronics, the manufacturer of STM32. It is more easy to use than CMSIS, but ST may discontinue it whenever they like. (Just like what they have done to STM32 Peripheral Library) [Controllerstech](https://controllerstech.com/stm32/) offers basic tutorials on how to program STM32 with STM32Cube HAL.
    * **CMSIS** is a HAL for the Cortex-M processor (including STM32). It is independent to MCU vendors and is considered as the hardcore way. You'll have to read [the reference manuals](https://www.st.com/content/ccc/resource/technical/document/reference_manual/59/b9/ba/7f/11/af/43/d5/CD00171190.pdf/files/CD00171190.pdf/jcr:content/translations/en.CD00171190.pdf) by ST Microelectronics and manipulate the MCU by altering its registers.
* STM32 can be used with [**Real-time OS (RTOS)**](https://en.wikipedia.org/wiki/Real-time_operating_system), which offers functionalities such as event and multi-threading.
    * [**ChibiOS**](http://www.chibios.org/dokuwiki/doku.php?id=chibios:articles:start)
 is a RTOS that has been used by M2 members for a few years. There is [an article](https://github.com/m2robocon/m2_wiki/wiki/Setting-up-ChibiOS-workspace) that introduces how to set up a workspace for programming ChibiOS.
* Useful tool: [**STM32CubeMX**](https://www.st.com/en/development-tools/stm32cubemx.html) and [**STM32CubeIDE**](https://www.st.com/en/development-tools/stm32cubeide.html) but you might need a VPN to connect to their US server.
* **Quick Prototyping**. 
  * As Arduino boards are generally low-performance 8-bit devices, an upgrade path may be the now cheap and powerful ESP8266/ESP32 which have WiFi capabilities.
  * For USB emulations, a quick and cheap way is to use [Arduino Micro](https://www.arduino.cc/en/Guide/ArduinoLeonardoMicro) (ATmega32U4-based) that have support in generic Arduino IDE. [Advanced](https://github.com/NicoHood/HID) use can also be found.

## See Also
* [Necessary Information for Electrical Trainee](Necessary-Information-for-Electrical-Trainee)

## External Links
* [Tutorials - Arduino](https://www.arduino.cc/en/Tutorial/HomePage)
* [References - Arduino](https://www.arduino.cc/reference/en/)
