**Prerequisite: [Learning basic soldering skills](Learning-basic-soldering-skills)**

**Next Tutorial: [Arduino Tutorials (Learning Servo and Sensors)](Arduino-Tutorials-(Learning-Servo-and-Sensors))**

## Objectives
At the end of this self-learning lab, you should be able to:
* control motor’s rotation speed using PWM
* detect the speed of motor by speed sensor
* perform a basic feedback control

## 5V DC Motor

![DC motor](/m2robocon/m2_wiki/raw/master/images/(Arduino_Tutorials)_DC_Motor.jpg)

A DC motor (Direct Current motor) is the most common type of motor. DC motors normally have just two leads, one positive and one negative. If you connect these two leads directly to a battery, the motor will rotate. If you switch the leads, the motor will rotate in the opposite direction.

![Motor wiring](/m2robocon/m2_wiki/raw/master/images/(Arduino_Tutorials)_Motor_Wire.jpg)

For a simple demonstration, connect the motor to the 5V pin and GND pin on Blue Bill directly. You should have soldered the wire to the motor beforehand. The motor is directly drawing current from your PC, so please try it for **a few seconds only**.

Code is not required for the motor to run. The 5V pin will output the voltage continuously without any code. You can reverse the two wires connected to the motor so the motor will turn in another direction.

### Try it yourself 01
**Fail attempt**

Plug in the wires of the motor to GPIOs on Blue Pill, and try to control it using `digitalWrite()`. [Why doesn't it work](Arduino-Tutorials-(Getting-Started-with-Arduino-IDE)#board-anatomy)?

## L298N Motor Driver Module
![A L298N module](/m2robocon/m2_wiki/raw/master/images/(Arduino_Tutorials)_L298N_module.jpg)

[L298N](https://www.st.com/en/motor-drivers/l298.html) is a dual full bridge driver designed to accept digital signals and drive inductive loads like DC motors. Here is a popular L298N module that can drive up to 2 motors.

Check the instructions to see how it works.
* [L298N Dual H-Bridge Motor Driver User Guide](http://www.handsontec.com/dataspecs/L298N%20Motor%20Driver.pdf)
* [Control DC and Stepper Motors With L298N Dual Motor Controller Modules and Arduino: 3 Steps - Instructables](https://www.instructables.com/id/Control-DC-and-stepper-motors-with-L298N-Dual-Moto/)

Note that if you are using an external power supply, there should be a common ground connecting the power supply, L298N drive and Blue Pill.

### Section Check Box:
* How to provide power to L298N driver Module
* How to connect the motors to L298N driver Module

### Try it yourself 02
**Need for Speed**

* Instead of digital signal, applied the L298N driver with PWM signal. Try to control the speed of the motor.
* Check the supplied voltage to the DC motor using a multimeter. You may notice that the motor doesn't rotate under a certain voltage level. Find out the voltage needed to turn on the motor. [Why doesn't it move](https://ebldc.com/?p=34)?

## Speed Sensor
![Speed Sensor](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Speed_Sensor.jpg)

Speed sensors are used to detect the speed of a device moving or rotating. There are many kinds of speed sensors. In this lab, we will use LM393, which is a simple photo-resistor light sensor that has both analog and digital outputs. The digital output has a trim potentiometer that can be used to set a trigger light level.

![Encoder Disk](https://github.com/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Encoder_Disk.jpg)

You will also need to use the encoder disc above, placing inside the gap of the speed sensor. Thus, when the disk rotates, the speed sensor can detect the rotation.

### Do it yourself 03
**Motor speed sensing**

* Connect the encoder disc and speed sensor to the motor, and connect the digital pin of speed sensor to Blue Pill. Use digitalRead() and rotate the motor manually to observe how it behaves.
* Drive the motor with the board and write a code to measure the Revolutions Per Minute (RPM) of the motor. You may use interrupt to count the number of or with the help of built-in function [pulseIn()](https://www.arduino.cc/en/Reference/PulseIn).

## Feedback control
![Feedback Control](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_feedback_ctrl.png)

In order to control the speed of the motor, you can:
1. Apply a certain voltage to the motor.
2. Measure the speed of the motor and calculate the error, i.e. the difference between the desired speed and the actual speed.
3. Increase the supplied voltage if the speed is too low, vice versa. It may be more responsive if the adjustment is proportional to the error.
4. Repeat 2 - 3.

This is called a [negative feedback control](https://en.wikipedia.org/wiki/Control_theory#Open-loop_and_closed-loop_(feedback)_control). (The same as the one you learnt in secondary school biology) This method we have used is called [proportional control](https://en.wikipedia.org/wiki/Proportional_control).

### Do it yourself 04
**I have complete control**

* Write a code that can control the motor to rotate at around 100 RPM.
* Apply some resistance over the motor. How does the supplied voltage level changed? Can it maintain at 100 RPM?

## PID controller
![PID Compensation]((Arduino_Tutorials)_PID_Compensation_Animated.gif)

Proportional control is a primitive control method and has many problems:
* Slow response time
* Unable to reach target
* Overshoot and oscillate

There is another control method called [PID control](https://en.wikipedia.org/wiki/PID_controller). It takes account of the proportional, integral, and derivative terms of the error. Our measurement here is slow and inaccurate, so PID control doesn't perform well here. However, it's better to learn more about this:

* [PID controller - Wikipedia](https://en.wikipedia.org/wiki/PID_controller)
* [Improving the Beginner’s PID – Introduction - Project Blog](http://brettbeauregard.com/blog/2011/04/improving-the-beginners-pid-introduction/)
* [PIDLibrary - Arduino Playground](https://playground.arduino.cc/Code/PIDLibrary)

## Assignment 02
**You accidentally made a servo motor**

Write a code that can control motor speed. It should:
* Read desired speed from serial input (continuously) and rotate the motor at the desired speed.
* Print the value of currently supplied voltage and RPM of the motor.

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))
* [Arduino Tutorials (Learning Servo and Sensors)](Arduino-Tutorials-(Learning-Servo-and-Sensors))


<!--
Oh, you've found the "secret of the world"!
Here is the suggested answers of Assignment 03:

const int MOTOR_PIN = PA3;
const int SPEED_PIN = PA4;
const int DISK_RESOLUTION = 40;
const int DESIRE_RPM = 100;
const int CTRL_PERIOD = 200000; // in microseconds
const int CTRL_P = 2;

int g_count = 0;
float g_rpm = 0;
int g_motor_v = 200;

void handler_speed(void);
void handler_ctrl(void);

void setup()
{
  delay(1000);
  Serial.begin(9600);
  pinMode(MOTOR_PIN, OUTPUT);
  pinMode(SPEED_PIN, INPUT);

  attachInterrupt(SPEED_PIN, handler_speed, CHANGE);

  Timer1.setMode(TIMER_CH1, TIMER_OUTPUTCOMPARE);
  Timer1.setPeriod(CTRL_PERIOD);
  Timer1.setCompare(TIMER_CH1, 1);
  Timer1.attachInterrupt(TIMER_CH1, handler_ctrl);
}

void loop() {
  Serial.print(g_motor_v);
  Serial.print(' ');
  Serial.println(g_rpm);
  delay(200);
}

void handler_speed(){
  g_count++;
}

void handler_ctrl(){
  
  int count_tmp = g_count;
  
  g_count = 0;
  g_rpm = count_tmp*60.0*1000000/DISK_RESOLUTION/CTRL_PERIOD;

  int error = g_rpm - DESIRE_RPM;
  Serial.println(error);
  g_motor_v -= error * CTRL_P;
  
  g_motor_v = constrain(g_motor_v, 0, 255);
  analogWrite(MOTOR_PIN, g_motor_v);
  
}
-->