**Next Tutorial: [Learning basic control](Arduino-Tutorials-(Learning-basic-control))**

## Objectives
At the end of this self-learning lab, you should be able to:
* Understand Timer
* Use PWM
* Use External Interrupt
* Use Timer Interrupt

## Things you need
* STM32 development board (Blue Pill)
* Solderless breadboard
* LED
* A resistor, anything between a few hundred to a few thousand ohms is OK. Check the resistance with a multimeter or by reading the [color code](https://www.allaboutcircuits.com/tools/resistor-color-code-calculator/) on it.
* Momentary button
* Hookup wire (or Dubon wires as we like to call them)

## Timer
STM32 has hardware timers, each consist of a counter and several registers to control the behaviour of the counter. The counter increments from 0 to its maximum, and then back to 0 again. (In upcounting mode. There are also downcounting and center-aligned mode, which behave similarly)

## PWM
PWM is the abbreviation of Pulse Width Modulation.

![PWM](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_PWM.gif)

There are 2 major functions:
* The length of the pulse width can be used to encoded data. Servo motor (which will be mentioned later) uses PWM to acquire the desire position.
* PWM wave can be used to control the power delivered to a desired value. In the example below, the brightness of an LED can be adjusted even though only 3.3V and 0V is delivered from Blue Pill. Also, for a circuit with sufficient inductance, average analog waveform at the approximate desired voltage level can be recovered. L298N motor driver (which will be mentioned later) is an example.

In STM32, PWM waves are generated with timers. The output pin is set to HIGH only when the conuter of the timer is lower then a preset value.

To use PWM control in Arduino, we may need to use analogWrite(). Here we will use a LED to demostrate. Select one of the PWM pins (you may check [the pinout](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Blue_pill_pinout.gif)). Connect the circuit as below:

![Schematic for Fading LED](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_PWM_schem.png)

Click `File` → `Examples` → `03.Analog` → `Fading`. Change the value of `ledPin` to the pin you've connected the LED. You should see a breathing LED light.

Section Check Box:
* What is PWM
* Using analogWrite()

### Further Reading
* [PWM - Arduino](https://www.arduino.cc/en/Tutorial/PWM)
* [analogWrite() - Arduino](https://www.arduino.cc/en/Reference/AnalogWrite)
* [analogWrite() - Maple v0.0.12 Documentation](http://docs.leaflabs.com/static.leaflabs.com/pub/leaflabs/maple-docs/0.0.12/lang/api/analogwrite.html)
* [Tutorial 10: Fade an LED with Pulse Width Modulation using analogWrite() - Programming Electronics Academy](https://programmingelectronics.com/tutorial-10-fade-an-led-with-pulse-width-modulation-using-analogwrite/)

### Try it yourself 01
**pwmWrite()**

pwmWrite() is a function only available to STM32 but not genuine Arduino, which offers 16-bit PWM resolution. Make a breathing LED light the same as above using pwmWrite(). Notice that pin mode needed to be changed from `OUTPUT` to `PWM` in order to use pwmWrite(). The following sites may help you:
  * [PWM - Maple v0.0.12 Documentation](http://docs.leaflabs.com/static.leaflabs.com/pub/leaflabs/maple-docs/0.0.12/pwm.html)
  * [Easy & Powerful Arduino Alternative? STM32 Beginner's Guide - Youtube](https://www.youtube.com/watch?v=EaZuKRSvwdo)


## External Interrupt
You may have heard of [interrupt service routine](https://en.wikipedia.org/wiki/Interrupt_handler) (ISR), which is a piece of code triggered by a interrupt signal. All digital pins on STM32 can be used to connect to external interrupt signal. (Unlike genuine Arduino, which has 2 - 6 pins available only)

Connect a button just like what you have did in [Getting Started with Arduino IDE](Arduino-Tutorials-(Getting-Started-with-Arduino-IDE)).

![Schematic for button](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_TIY02_schem.png)

Run the following code:

```c++
const int BTN_PIN = PB9;

bool g_state = false;

void handler(void);

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(BTN_PIN, INPUT_PULLDOWN);

  attachInterrupt(BTN_PIN, handler, RISING);
}

void loop() {
}

void handler(){
  g_state = !g_state;
  digitalWrite(LED_BUILTIN, g_state);
}
```

Here handler() is called once the button is pressed.

### Further Reading
* [attachInterrupt() - Arduino Reference](https://www.arduino.cc/reference/en/language/functions/external-interrupts/attachinterrupt/)
* [External Interrupts - Maple v0.0.12 Documentation](http://docs.leaflabs.com/static.leaflabs.com/pub/leaflabs/maple-docs/0.0.12/external-interrupts.html)


### Try it yourself 02
**Control a breathing light**

Make a breathing light that turns on only when the button is pressed. Make use of external interrupt.

## Timer Interrupt
It is possible to generate interrupt signals using a timer. The function can be executed at a regular time intervals. This is especially useful in real-time control.

Here we will be using the PC13 LED as an example:

```c++
const int LED_PIN = PC13;
const int LED_RATE = 500000;  // in microseconds; therefore the interrupt handler will be called every 0.5s

void handler_led(void);

int g_state = 0;

void setup()
{
  pinMode(LED_PIN, OUTPUT);
  
  // Setup LED Timer
  // In output compare mode, timer counts from 0 to its reload value repeatedly; every time the counter value reaches one of the channel compare values, the corresponding interrupt is fired.
  Timer1.setMode(TIMER_CH1, TIMER_OUTPUTCOMPARE);
  Timer1.setPeriod(LED_RATE); // in microseconds
  Timer1.attachInterrupt(TIMER_CH1, handler_led); // handler_led() is the interrupt handler
}

void loop() {
}

void handler_led() {
    g_state = !g_state;
    digitalWrite(LED_PIN, g_state);
}
```

Here handler_led() is called every 0.5s. The LED should be blinking at 1 Hz.

### Further Readings
* [HardwareTimer - Maple v0.0.12 Documentation](http://docs.leaflabs.com/static.leaflabs.com/pub/leaflabs/maple-docs/0.0.12/lang/api/hardwaretimer.html)
* [TimerPWMCheatsheet - Arduino Palygrou](https://www.instructables.com/id/Arduino-Timer-Interrupts/), which is an example code that manipulate timer counter registers such as TCCR2A, TCCR2B, OCR2A and OCR2B. Unfortunately, it only works on AVR MCUs. For STM32, it has a different system to control timers.
* [Controlling STM32 Hardware Timers with Interrupts | VisualGDB Tutorials](https://visualgdb.com/tutorials/arm/stm32/timers/)

## Assignment 01
**Control a breathing light Mk-II**

Make a breathing light that can be toggled by a button or a command from serial input. Use timer interrupt to adjust the brightness of the LED, and use external interrupt read the state of the button.

You may check the [maple documentation](http://docs.leaflabs.com/static.leaflabs.com/pub/leaflabs/maple-docs/0.0.12/lang/api/hardwaretimer.html) and see how to use `pause()` and `resume()`.

## Reference

### PWM
* [Pulse-Width Modulation (PWM) - 成大資工 Wiki](http://wiki.csie.ncku.edu.tw/embedded/PWM)
* [Using STM32 timers in PWM mode | VisualGDB Tutorials](https://visualgdb.com/tutorials/arm/stm32/pwm/)

### External Interrupt
* [從 Arduino 到 AVR 晶片(2) -- Interrupts 中斷處理 - 程式人雜誌](http://programmermagazine.github.io/201407/htm/article1.html), which is specfic to AVR MCUs and hence can be used on genuine Arduino only.

### Timer Interrupt
* [Arduino_STM32/TimerInterrupts.ino at master · rogerclarkmelbourne/Arduino_STM32](https://github.com/rogerclarkmelbourne/Arduino_STM32/blob/master/STM32F1/libraries/A_STM32_Examples/examples/Maple/TimerInterrupts/TimerInterrupts.ino)

## Further Reading
* [Secrets of Arduino PWM - Ken Shirriff's blog](http://www.righto.com/2009/07/secrets-of-arduino-pwm.html), for genuine Arduino only
* [General-purpose timer cookbook - STElectronics](https://www.st.com/content/ccc/resource/technical/document/application_note/group0/91/01/84/3f/7c/67/41/3f/DM00236305/files/DM00236305.pdf/jcr:content/translations/en.DM00236305.pdf)

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))
* [Learning basic control](Arduino-Tutorials-(Learning-basic-control))