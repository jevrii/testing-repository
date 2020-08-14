**Next Tutorial: [Communication](Arduino-Tutorials-(Communication))**

## Objectives
At the end of this self-learning lab, you should be able to:
* control servo motors
* read analog signal
* understand analog signal reading and I2C communication protocol
* take reading from different sensors

## Servo motor
![Servo](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Servo.jpg)

A servo motor is a rotary actuator or linear actuator that allows for precise control of angular or linear position, velocity and acceleration. Because servo motors use feedback to determine the position of the shaft, you can control that position very precisely. As a result, servo motors are used to control the position of objects, rotate objects, move legs, arms or hands of robots, move sensors etc. with high precision.

Read more about servo motor from the following websites.
* [Servomotor - Wikipedia](https://en.wikipedia.org/wiki/Servomotor)
* [How Servo Motors Work | Servo Motor Controllers](https://www.jameco.com/jameco/workshop/howitworks/how-servo-motors-work.html)
* [Arduino : How to Control Servo Motor With Arduino - Instrutables](http://www.instructables.com/id/Arduino-How-to-Control-Servo-Motor-With-Arduino/)

As Blue Pill is much more powerful than genuine Arduino, any PWM pin can be used for servo signal.

You should notice that you need to use a library when you want to turn the servo motor to a designated position. The description of the library is as follows.
* [Servo - Arduino](https://www.arduino.cc/en/reference/servo)

Yet, the angle of servo motor is just controlled by PWM, which we have learnt before. Thus, we can also write a code to make a servo motor turn without using a library. Here is an example.
* [Servo and PWM without library - Arduino Forum](http://forum.arduino.cc/index.php?topic=5983.0)

### Do it yourself 01
**Servo Motor Controlling**

* Write a code which turns exactly 45 degrees and stops there.
* Write a code which controls the servo motor to rotate back and forth. 

## Analog Signal

![Analog vs Digital](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Signals.PNG)

Analog signal is sent and received in the form of a wave. Digital signal is the opposite of analog signal. For the graph above, the L.H.S. expresses what an analog signal looks like, while the one on the right imitates a digital signal. You can imagine there is a door, digital signal can only tell you whether the door is closed or opened, but analog signal can tell you how wide is the door opening. <br/>

Receiving and sending analog signals in Arduino is as easy as tackling digital signals. Read the following sites to learn more about analog signals.
* [Analog vs Digital](https://learn.sparkfun.com/tutorials/analog-vs-digital)
* [Analog input in Arduino](https://www.arduino.cc/en/Tutorial/AnalogInput)
* [IR distance sensor measurement](https://create.arduino.cc/projecthub/adammansour/distance-measuring-sensor-assignment-faeeb4)
* [Build a distance sensor from scratch](https://www.alanzucconi.com/2015/10/14/how-to-build-a-distance-sensor-with-arduino/)

![IR sensor](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_IR_sensor.jpg)

This is an IR distance sensor, which radiates IR light and measures the amount of IR reflected. It output analog singal with can be read by Blue Pill through any pin that support [ADC](https://en.wikipedia.org/wiki/Analog-to-digital_converter). Let's connect the sensor to Blue Pill.

* VCC: 3.3V
* GND: you guese it
* A0: pin that support ADC

```c++
const int SENSOR_PIN = PA0;   // or any other pin you like

void setup() {
  delay(1500);
  Serial.begin(9600);
  pinMode(SENSOR_PIN, INPUT); // pinMode can also be set as INPUT_ANALOG, but then it won't be able to use digitalRead() on the same pin
}

void loop() {
  Serial.println(analogRead(SENSOR_PIN));
  delay(1000);
}
```

Unlike genuine Arduino, analogRead() here on STM32 has 12-bit resolution. You should find that the signal is lower when there is more reflected IR light.

### Do it yourself 03
**Planet Analogue and Planet Di Gi Charat**

The signal can also be treated as a digital signal. Use digitalRead() and analogRead() at the same time and determine the threshold voltage that differentiate a HIGH signal from a LOW signal.

## I²C communication protocol

![I²C](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_I2C.jpg)

I²C is a communication protocol which allows master devices to send and receive data from slave devices. Although the speed of data transmission through I²C is not very fast (~100kbits/s), it occupies a very little number of connections to establish a connection between devices. Moreover, the number of connections needed does not increase with an increase in devices. This makes I²C a very popular protocol in MCU-sensor connections.
 
Find out more about I²C from the following websites.
* [I2C introduction](https://learn.sparkfun.com/tutorials/i2c)
* [Arduino wire library for using I2C](https://www.arduino.cc/en/reference/wire)
* [I2C communication between 2 Arduino board](http://www.instructables.com/id/I2C-between-Arduinos/)

![MPU6050 breakout board](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_MPU6050.jpg)

MPU6050 is an IC with a 3-axis gyroscope, a 3-axis accelerometer and temperature sensor. It uses I²C to communicate. Here is an example code to use MPU6050:

```c++
// MPU-6050 Short Example Sketch
// By Arduino User JohnChi
// August 17, 2014
// Public Domain
#include <Wire.h>

const int MPU_ADDR = 0x68;                 // I2C address of the MPU-6050
void setup()
{
    Wire.begin();
    Wire.beginTransmission(MPU_ADDR);
    Wire.write(0x6B);                      // PWR_MGMT_1 register
    Wire.write(0);                         // set to zero (wakes up the MPU-6050)
    Wire.endTransmission(true);
    Serial.begin(9600);
}
void loop()
{
    int16_t ac_x, ac_y, ac_z, temp, gy_x, gy_y, gy_z;

    Wire.beginTransmission(MPU_ADDR);
    Wire.write(0x3B);                      // starting with register 0x3B (ac_CEL_XOUT_H)
    Wire.endTransmission(false);
    Wire.requestFrom(MPU_ADDR, 14);        // request a total of 14 registers
    ac_x = Wire.read() << 8 | Wire.read(); // 0x3B (ac_CEL_XOUT_H) & 0x3C (ac_CEL_XOUT_L)
    ac_y = Wire.read() << 8 | Wire.read(); // 0x3D (ac_CEL_YOUT_H) & 0x3E (ac_CEL_YOUT_L)
    ac_z = Wire.read() << 8 | Wire.read(); // 0x3F (ac_CEL_ZOUT_H) & 0x40 (ac_CEL_ZOUT_L)
    temp = Wire.read() << 8 | Wire.read(); // 0x41 (TEMP_OUT_H) & 0x42 (TEMP_OUT_L)
    gy_x = Wire.read() << 8 | Wire.read(); // 0x43 (gy_RO_XOUT_H) & 0x44 (gy_RO_XOUT_L)
    gy_y = Wire.read() << 8 | Wire.read(); // 0x45 (gy_RO_YOUT_H) & 0x46 (gy_RO_YOUT_L)
    gy_z = Wire.read() << 8 | Wire.read(); // 0x47 (gy_RO_ZOUT_H) & 0x48 (gy_RO_ZOUT_L)
    Serial.print("Temp = ");
    Serial.print(temp / 340.00 + 36.53);   // equation for temperature in degrees C from datasheet
    Serial.print("\tAcc X = ");
    Serial.print(ac_x);
    Serial.print(" Y = ");
    Serial.print(ac_y);
    Serial.print(" Z = ");
    Serial.print(ac_z);
    Serial.print("\tGyro X = ");
    Serial.print(gy_x);
    Serial.print(" Y = ");
    Serial.print(gy_y);
    Serial.print(" Z = ");
    Serial.println(gy_z);
    delay(333);
}
```

### Further Readings
* [Getting Started With Arduino and MPU6050](https://www.electronicshub.org/getting-started-arduino-mpu6050/)
* [MPU6050 notes(Chinese)](http://gogoprivateryan.blogspot.com/2014/07/mpu-6050-google.html)

### Do it yourself 03
**MPU6050 Gyroscope**
* Try to detect whether the MPU6050 breakout board is tilted using the gyroscope and/or the accelerometer readings.

## Library
What is library?  It is code that written and contributed by many different people. It provides many great additional capabilities to run new and different hardware devices. The code will be inserted in your code if you add a library to your code. Find out more by yourself.
* [Libraries - Arduino](https://www.arduino.cc/en/Reference/Libraries)
* [Installing Additional Arduino Libraries - Arduino](https://www.arduino.cc/en/Guide/Libraries)

It is possible to calculate odometry using either gyroscope or the accelerometer readings. However, accelerometer is noisy and hence is inaccurate in short term; while gyroscope drifts and hence is inaccurate over long term. Here comes [Kalman Filter](http://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/) to save the world. It fuses different measurements over time to provide a better estimation of the value. It is a difficult topic in robotics, so understanding and using of Kalman filter is not required at this stage. Just for your reference.

Understanding how the stuff works is a bit complicated for beginners. Thus, there are always geniuses which make a library by themselves. We can include the library so that values can be obtained by just calling the corresponding function. There is a Arduinolibrary for Kalman Filer:
* [TKJElectronics/KalmanFilter: This is a Kalman filter used to calculate the angle, rate and bias from from the input of an accelerometer/magnetometer and a gyroscope - GitHub](https://github.com/TKJElectronics/KalmanFilter)

### Do it yourself 04
**kalman Filter**

There is an Kalman Filter example on MPU6050. After installing the library, click `File` → `Examples` → `Kalman Filter Library` → `MPU6050` and execute the code.

As stop bit is not yet implemented in the Arduino STM32 Core, `Wire.requestFrom(IMUAddress, nbytes, (uint8_t)true)` may need to be changed into `Wire.requestFrom(IMUAddress, nbytes)`.

## Assignment 03
**clickclickclick**

There was a game called [clickclickclick.com](http://www.clickclickclick.com/), which players compete to make most mouse clicks in a week. People use [different](https://www.youtube.com/watch?v=Z-1ZgWGbHiw) [methods](https://www.youtube.com/watch?v=O0ktC-V6K2E&t=35s) to fight for their victory. Unfortunately, it was closed in 2017, but we can still make a robot that click your mouse automatically to memorise this game.

* Use a servo to click a mouse automatically.
* As the user may want to temporarily use the mouse and move the mouse away, it should stop clicking when the mouse isn't nearby. Use a IR distance sensor to detect the presence of the mouse. You may want to use a mouse that's white in colour.
* It is dangerous if the robot still operate when it's falling! Stop servo action when it suspects that it is falling and restart only when there's a user reset.

## References
* [SG90 Digital - Tower Pro](http://www.towerpro.com.tw/product/sg90-7/)
* [MPU-6050 - Arduino Playground](https://playground.arduino.cc/Main/MPU-6050)
* [MPU6050 breakout board GY-521 Datasheet](http://synacorp.my/v2/en/index.php?controller=attachment&id_attachment=635)
* [MPU6050 Datasheet](https://www.invensense.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf)

## Further Readings
* [Arduino for STM32 + MPU-6050 == Improve your programming skills! - Youtube](https://www.youtube.com/watch?v=ImctYI8hgq4)

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))
* [Arduino Tutorials (Interacting with Computers)](Arduino-Tutorials-(Interacting-with-Computers))