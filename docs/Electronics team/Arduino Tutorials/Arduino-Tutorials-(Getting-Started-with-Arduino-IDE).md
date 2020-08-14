**Next Tutorial: [Learning basic programming with Arduino](Arduino-Tutorials-(Learning-basic-programming-with-Arduino))** 

## Objectives
At the end of this self-learning lab, you should be able to:
* understand Arduino’s hardware
* starting using Arduino IDE
* write basic Arduino codes

## What is Arduino?
![Arduino Uno](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Uno.jpg) ![Arduino Nano](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Nano.jpg)

Arduino is an open-source platform used for building electronics projects. It consists of Arduino boards and the Arduino software. Arduino board is a physical programmable circuit board, or simply called a **microcontroller** (**MCU** for microcontroller unit). There are many different types of Arduino boards with different characteristics, including Arduino UNO, Arduino NANO and Arduino MEGA.

Arduino IDE makes it easy for beginners to write code and upload it to the board. It uses a dialect of features from C++.

## What's STM32?
STM32 is a family of 32-bit MCU by STMicroelectronics. There are many development boards based on STM32 chips on the market.

## So, is STM32 Arduino?
No.

## Then, why are you telling me about STM32?
People have made a Arduino Core for STM32 development boards, thus we can use Arduino IDE to programme STM32 boards, which has a shallower learning curves than using µVision.

## Blue Pill

![pinout of Blue Pill](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Blue_pill_pinout.gif)

[**Blue Pill**](https://wiki.stm32duino.com/index.php?title=Blue_Pill) is the nickname given to the most popular, ultra-cheap and compact STM32F103C8T6 development board made by Chinese manufacturers.

### Hardware Specification
|Hardware Spec    |    |
|-----------------|----|
|MCU              |STM32F103C8|
|Flash            |64 KB|
|RAM              |20 KB|
|Clock Speed      |72 MHz|
|Voltage regulator|[RT9193-33](https://www.richtek.com/assets/product_file/RT9193/DS9193-16.pdf) (300 mA)|
|Schematic        |[Vcc-gnd.com-STM32F103C8-schematic.pdf](https://wiki.stm32duino.com/images/c/c1/Vcc-gnd.com-STM32F103C8-schematic.pdf)|

### Board Anatomy
* Pins
  * PA0 - PA15 (marked as A0 - A15 on the board)
  * PB0 - PB15 (marked as B0 - B15)
  * PC13 - PC15 (marked as C13 - A15)

      They are the main characters of the board, GPIO (General-purpose input/output), which have digital signal inputs and outputs depend on how the program has defined them, which will be explain later. They also have alternate functions, such as PWM, ADC, DAC, USART, I2C, SPI, CAN and many others (You may read [STM32F1 Reference Manual](https://www.st.com/content/ccc/resource/technical/document/reference_manual/59/b9/ba/7f/11/af/43/d5/CD00171190.pdf/files/CD00171190.pdf/jcr:content/translations/en.CD00171190.pdf#%5B%7B%22num%22%3A233%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C67%2C755%2Cnull%5D) to get to know more). The maximum current output from a GPIO is 25 mA.
  * GND (marked as G)

      Ground. Should be connected to the ground of your circuits.
  * 3.3V

      3.3V Power supply from the voltage regulator ([RT9193-33](https://www.richtek.com/assets/product_file/RT9193/DS9193-16.pdf)). Can be used to provide +3.3V power.
  * 5V

      Even though it is called 5V, it is actually a power input pin and does not guarantee to be 5V. You may connect it with external power supply to power the board, and the recommanded power supply is 3.3V - 5.5V.
      It is directly connected to the 5V pin of USB, so remember **NEVER** connect USB and other power supply at the same time, or draw to much current from this pin. Otherwise, it may **FRY** your PC.
  * NRST (marked as R)

      It is connected to the reset button. Alternatively, connecting this pin to the ground can reset the microcontroller too. (The name start with N means it is activated when connecting to the ground)
  * DIO, DCLK

      It is used to connect to a hardware programmer such as ST-Link and J-link.
  * VBAT (marked as VB)

      Power supply for RTC, external clock 32 kHz oscillator and backup registers. Doesn't really matter.
* LED
  * Power LED

      It indicates that your Blue Pill is receiving power. It is useful for debugging.
  * PC13 LED

      It is connected to PC13 and 3V3 (sure with a current-limiting resistor), and it will light up when a LOW signal is assigned to pin PC13. Besides being a handy target for your first blink sketch, this LED is very useful for debugging.
* STM32F103CT6

    The heart of your board. The processor, flash memory, SRAM and EEPROM written in the specification above are all integrated in this small chip.
* micro USB port

    Used for powering your board, uploading your sketches to your board, and for communicating with your board via serial communication.
* Reset button

    Resets the microcontroller.

Now, you have already had a basic understanding of the hardware. Yet, without the software, it is just a piece of rubbish. Let get into the software of Arduino IDE.

## Arduino IDE

The Arduino Integrated Development Environment(IDE), or Arduino Software, contains a text editor for writing code, a message area, a text console, a toolbar with buttons for common functions and a series of menus. It connects to the development boards to upload programs and communicate with them. Arduino provides both online IDE (Arduino Web Editor) and desktop IDE for installation. More details of Arduino IDE can be seen at [Guide - Arduino](https://www.arduino.cc/en/Guide/HomePage). Web Editor provides less functionality which is not suitable for our works. Let’s have a look at it.

### Installation

#### Arduino IDE
Download the Arduino IDE from [Arduino official website](https://www.arduino.cc/en/Main/Software).
* Please **DON'T** use the Arduino Web Editor. It is impossible to install third-party hardwares and libraries, which we will be doing in the next step.
* If you are a Windows user, please **DON'T** install from Microsoft store. Use Windows Installer instead.
* If you are a Ubuntu user, please **DON'T** install from [official Ubuntu package](https://packages.ubuntu.com/bionic/arduino). It is version 1.0.5, released at 2013-05-15.

#### STM32 Arduino Core

1. Open Arduino IDE. Press File -> Preferences. You should see you `Sketchbook location`, which should be `<something>\Arduino`. Go to the sketchbook location and make a new folder called `hardware` in the `Arduino` folder.
2. Download [Arduino STM32](https://github.com/rogerclarkmelbourne/Arduino_STM32) from Roger Clark.
3. Extract the ZIP file and put it under `hardware` you just made.
4. 
    * For Windows user, go to `Arduino_STM32\drivers\win` and run `install_drivers.bat`.
    * For Linux user, go to `Arduino_STM32/tools/linux64` and run `install.sh`. Install 32-bit shared libraries for AMD64 too (`sudo apt install libc6-i386` for Ubuntu).
5. Restart Arduino IDE.
6. Press `Tools` -> `Board: <something>`. You should see `STM32 Boards (STM32duino.com)` in the menu.
7. Press `Tools` -> `Board: <something>` -> `Boards Manager`. Install `Arduino SAM Boards`. All we want is the compiler `arm-none-eabi-g++`.
8. Press ![Verify](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Verify.png) to see if it can compile.

* Please **DON'T** install [Arduino Core STM32](https://github.com/stm32duino/wiki/wiki) from ST official, i.e. don't use the method with 'Additional Boards Managers URLs'. It does not support uploading the codes with USB bootloader.

### Interface

![Arduino IDE](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_IDE.png)

Meaning of buttons in IDE interface:
* ![Verify](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Verify.png)
Verify 
  * Checks your code for errors. Will not upload your code to the board.
* ![Upload](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Upload.png)
Upload 
  * Compiles your code and uploads it to the board. See uploading below for details.
* ![New](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_New.png)
New 
  * Creates a new sketch.
* ![Open](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Open.png)
Open 
  * Presents a menu of all the sketches in your sketchbook. Clicking one will open it within the current window overwriting its content.
* ![Save](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Save.png)
Save 
  * Saves your sketch.

Inside the file “PlaceForCode”, it is the place where you put your code in. On the 1st line of the code, you can see `void setup()`. It is a function for the STM32 to run once at begin. Meaning of `void` and `function` will be discussed later. The braces symbol `{}` on line 1 and 4 define the scope of the setup() function.

The function under setup() is loop(). It is a function that will run repeatedly after the setup() function has run once at begin. Thus, the program flow is as below.

![Program Flow](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Program_Flow.jpg)

Now, you should know that Blue Pill will first execute the codes in setup() for one time, and then repeatedly execute the code in loop(). Yet, what code should be put in these functions? Here comes the answer.

### Section Check Box:
* What is Arduino IDE and its usage
* Meaning of buttons in IDE interfaces 
* `void setup() { … }`
* `void loop() { … }`

## Basic Arduino Code

The program in Arduino IDE is very similar to C++ codes, which will be introduced in the next tutorial. Let’s write a simple program that can control the LED to blink now.

### First program: Blink

Click `File` → `Examples` → `01.Basics` → `Blink`. Change `LED_BUILTIN` into `PC13`. It should look like:

```c++
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(PC13, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(PC13, HIGH);          // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(PC13, LOW);           // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```

* As said in the previous section, the program starts from the setup() function in line 1. ` { ` and ` } ` on line 1 and 3 defines the scope of setup() function.
* Inside this setup() function, on line 2, we have `pinMode(PC13, OUTPUT);` written there. This line is an instruction that tells the board to do something. 
    * This instruction is called a statement. Usually a statement fit in exactly one line. Each statement MUST end with a semicolon to denote that the statement has finished.
*. `pinMode(PC13, OUTPUT)`
    * It configures a pin as either an input or an output. 
    * Input means you use the pin to detect the voltage on the external circuit connected to that pin (will be discussed later), while output means you use the pin to output a voltage to control something. 
    * `PC13` and `OUTPUT` are the two parameters that are required by pinMode(). `PC13` means you are referring to pin PC13. Thus, you can change it to another pin, so that the pin mode of another pin can be defined. `OUTPUT` means you want the pin you have stated in the 1st statement to be in output mode. You can change it to `INPUT` so the pin is defined to be in INPUT mode. 
    * Noted that the `M` in pinMode must be in capital letter. The word OUTPUT and INPUT must be in capital letter too. 
* After the setup is finished, the program will then go into the loop() function. Line 6-9 are the 4 statements inside loop().
* `digitalWrite(PC13, HIGH)`
    * It means supplying 3.3 volts (HIGH logic signal) to pin PC13 from 0 volt (LOW logic signal) before this statement is executed. 
    * `digital` in digitalWrite means you can only write either HIGH or LOW value to the pin, corresponding to `1` or `0` for computer.
    * `Write` in digitalWrite means you are writing a value a pin, rather than reading a value. The opposite of this statement is digitalRead(pin) which will be discussed later.
    * Just like in `pinMode(PC13, OUTPUT);`, `PC13` and `HIGH` in `digitalWrite(PC13, HIGH)` are the two parameters for digitalWrites. 
    * The statement `pinMode(PC13, OUTPUT);` must be executed before `digitalWrite(PC13, HIGH);` , otherwise you may encounter unexpected situations. 
* `delay(1000);`
    * It pauses the program for the amount of time (in milliseconds) specified as parameter.
    * 1000 means 1000 milliseconds, which is equal to 1 second.
* `digitalWrite(PC13, LOW)`
    * Similar `to digitalWrite(PC13, HIGH);`, it controls the voltage output of pin PC13. This time, it assigns a `LOW`value to pin `PC13`, which means supplying 0 volt to the pin.
* Comment - useful but also useless
    * You may notice that there are sentences after the statements. They are comments.
    * Comments are lines in the program that are used to inform yourself or others about the way the program works. They are ignored by the STM32. Thus, comments are useful for yourself to view the code, but useless for the board to execute the code.
    * Comments only purpose are to help you understand (or remember) how your program works or to inform others how your program works. There are two different ways of marking a line as a comment.
    * Using double slash `//`
      * The comment starts after the `//`. It is a single line comment. Anything after the slashes is a comment to the end of the line.
    * Using `/*` and `*/`
      * The comment starts after `/*`, all the way through lines and sentences to `*/`. Unlike the double slash, it is multiline comment, using to comment out whole blocks of code.

### Section Check Box:
* `pinMode(pin , mode)`
* semi-colon `; `
* `digitalWrite(pin , value)`
* `delay(milliseconds)`
* Comment `//` or `/* … */`

### The more you know
`PC13` (and all other pins) are defined in `Arduino_STM32/STM32F1/variants/generic_stm32f103c/board/board.h`, which facilitate programmer to write codes.

`PC13` is defined as the integer `32`. The code still works if you replace `PC13` with `32`, but this is discouraged as it makes the code less readable.

## Upload the Code to board

You now have your code ready. It is time to put your code into the board to get the actual result. First of all, you should have your USB wire and the Blue Pill prepared.

1. Plug in the micro USB wire to Blue Pill and the other side to the computer. The red power LED on the board turned on shows that the it is working.
2. Return to the Arduino IDE. Before we upload the code to the board, we need to check that the IDE knows what development board we are using and which port is it connected to. 
    1. Press `Tools` → `Port`. If there are more than one choices of port, just randomly select one. If it is not the port you have you board connected to, error will be shown when you upload your code to the board. Just try and find out the one which gives no error and that is the desired port. 
    2. Press `Tools` → `Board: <something>`. Select `Generic STM32F103C series` under `STM32 Boards (STM32duino.com)`.
    3. Press `Tools` → `Variant: <something>`. Select `STM32F103C8 (20k RAM, 64k Flash)`.
    4. Press `Tools` → `Upload method: <something>`. Select `STM32duino bootloader`.
3. After the preparation steps done, we can now upload our code to the board. Click the upload button ![Upload](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Upload.png) and wait for it to compile…

![Done Uploading](https://github.com/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Done_Uploading.png)

When you see the words “Done uploading”, it means that your code has successfully uploaded to the board. You should now see another LED on the board blinking in a one second interval. Congratulation! You have learnt how to control your Blue Pill now.

### Section Check Box:
* Select the correct board from `Tools` → `Board`, `Variant` and `Upload method`
* Select the correct port from `Tools` → `Port`

### The more you know
Blue Pills just bought doesn't come with USB bootloader. USB port won't show up without USB bootloader. Standard method to flash programs is using a hardware programmer e.g. ST-Link, J-Link, or using a USB-TTL adapter along the built-in Serial bootloader on STM32. [Appendix below](#appendix-how-to-flash-usb-bootloader) will introduce the method to flash USB bootloader.

## Error and Debugging

Sometimes, a program will not compile and upload to the board successfully. Just like the one below. (To try it out, just randomly delete a semi-colon)

![Syntax Error](https://github.com/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Syntax_Error.png)

It shows that there is something wrong in the program. Go back to the highlighted line and find out what is wrong there. It is usually careless mistakes such as forgetting to add a semi-colon or capitalisation issue. If you don’t know what’s wrong with the code, you can ask us or copy the error message and search it on Google. 

![Problem uploading to board](https://github.com/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_Arduino_Upload_Fail.png)

If this error message – `Problem uploading to board` happens, there should be no error in your code. Problem arise from the data transfer between the board and computer. Simply unplug the board from the USB and plug it in again, and then reupload the code to the board can usually solve the problem. If it doesn't solve the problems, check the [FAQ below](#appendix-faq).  

### Try it yourself 01

**S.O.S. Signal**

Write an Arduino code to light up LED PC13 on the board, sending out the S.O.S. signal. As you should know, in Morse code, `S` can be represented by 3 short flashes, while `O` is represented by 3 long flashes.

Note that a `LOW` signal actually turn the PC13 LED on.

## pinMode(pin, INPUT) & DigitalRead(pin)

As mentioned before, apart from `digitalWrite(pin, value)` which controls the voltage output of the pin, we have `digitalRead(pin)` which reads the value from a specified digital pin. It will return either HIGH or LOW value, corresponding to 1 and 0.

The syntax `digitalRead(pin)` only take 1 parameter, which is the digital pin you want to read. 

Before you execute `digitalRead(pin)`, you need to set the pin to INPUT mode first.

Here is a code using both `digitalRead` and `digitalWrite`. It controls the LED on and off by pressing a button.

```c++
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(PC13, OUTPUT);
  pinMode(PB9, INPUT_PULLDOWN);
}

void loop() {
  digitalWrite(PC13, digitalRead(PB9));
  //delay(10);
}
```

* `pinMode(PB9, INPUT_PULLDOWN)`
    * As pin PC13 is used to output voltage to the LED, we will use another pin to read the input value, let’s say, pin PB9.
    * Thus, we type `pinMode(PB9, INPUT_PULLDOWN)` to tell the board that we want to set the mode of pin PB9 into reading inputs.
* `digitalWrite(PC13, digitalRead(PB9))`
    * You should remember that `digitalRead(pin, value)` takes 2 parameters. For value, you set it as HIGH or LOW. This value can also be provided by `digitalRead(pin)`, which give you exactly HIGH or LOW value.
    * Thus, you can type `digitalRead(PB9)` in the 2nd parameter directly. The board will get the value of 2nd parameter for digitalWrite automatically from digitalRead.

![Schematic](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_TIY02_schem.png)


We will setup a circuit like this to let pin PB9 to read the value. The circuit starts from the 3.3V pin which constantly providing 3.3V, to the GND pin. Apart from the button, A resistor is added into the circuit. Pin PB9 is connected between the button and the resistor, so it is at 3.3V a.k.a. HIGH signal when you press the button.

To implement this, you need a breadboard, a button, a resistor and several male-to-male Dupont wires prepared. Connect the components as bellows.

![Connection inside a breadboard](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_breadboard.jpg)

This is a breadboard, which has holes that let you easily insert electronic components to prototype an electronic circuit, and it has inter-connection between holes as above.

![Connection with a breadboard](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_TIY02_bb.png)

The circuit should be similar to the one above. Green dots on the above figure across a row in the graph above mean they are electrically connected to each other.

You can now upload your new code to the board. If it works correctly, the pin PC13 LED should turn off when you press the button and turn on when you release.

### Section Check Box:
* `pinMode(pin, INPUT);`
* `digitalRead(pin);`

### Try it yourself 02
**Send a signal by button**
1. Write an Arduino code which will light up LED 13 for 0.5 seconds when it button is pressed. The light turns off for 0.5 seconds afterwards, and then turn on again if the button is stilled pressed.
2. You should notice that a resistor is needed in the circuit. It is called a [pull-down resistor](https://en.wikipedia.org/wiki/Pull-up_resistor). Disconnect it and see if anything is changed.
  Now let's change `pinMode(PB9, INPUT_PULLDOWN)` into:
    * `pinMode(PB9, INPUT)`
    * `pinMode(PB9, INPUT_PULLUP)`

    Does it still work without the pull-down resistor?

    You may read [Arduino_STM32/io.h at master · rogerclarkmelbourne/Arduino_STM32](https://github.com/rogerclarkmelbourne/Arduino_STM32/blob/master/STM32F1/cores/maple/io.h) and [Input configuration on STM32F1 Reference Manual](https://www.st.com/content/ccc/resource/technical/document/reference_manual/59/b9/ba/7f/11/af/43/d5/CD00171190.pdf/files/CD00171190.pdf/jcr:content/translations/en.CD00171190.pdf#%5B%7B%22num%22%3A236%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C67%2C754%2Cnull%5D) to get to know more.

## Serial Write

We can also display some messages from the board to the computer through serial communication. Type the code below.

```c++
void setup() {
  // It takes some time before Blue Pill can reconnect to PC
  // Delay is needed when there is serial communication
  // Genuine Arduino does not require a delay here
  // add more if 1500 is not enough
  // Alternatively, while(!Serial); can be used after Serial.begin(9600); , which will block the program from execution until Serial Monitor is opened
  delay(1500);

  // Initiate Serial communication
  Serial.begin(9600);
}

void loop() {
  Serial.println("Hello, World!");
  delay(1000); // So that it wouldn't print crazily
}
```

* `Serial.begin(9600)`
    * For us to use the functions of the Serial library, we have to initiate serial communication – to do this we use the `Serial.begin()` function. `Serial.begin()` needs to go in the `setup()`.
    * Now for reasons beyond the scope of this discussion, it is convenient to use the number `9600` in the `Serial.begin()` function. The value `9600` specifies the [baud rate](https://en.wikipedia.org/wiki/Baud). The baud rate is the rate at which information will pass between Blue pill and the computer.
* `Serial.println("Hello, World!")`
    * This statement will print the words you type inside `"…"` to the computer.
* Upload your code to the Blue Pill. You cannot see the words printed on the computer yet. You will need to press Ctrl+Shift+M to open the Serial Monitor. The words are printed there.

### Section Check Box:
* `Serial.begin(9600);`
* `Serial.println();`

### Try it yourself 03
**Printing words**

* You should notice that the program above will print the words repeatedly. Write a code that will only print once only and at least two different sentences.
* Apart from `Serial.println()`, there is also `Serial.print()`. Try to find out the difference between them.

## Serial read
In order to send information to the Blue pill, we can use Serial read.
```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
}

void loop() {
  // Block the program from execution until there is serial input
  // Can be omitted if getting serial input is not necessary
  while (Serial.available() == 0);

  // Read until the serial buffer is empty
  while (Serial.available() > 0) {
    char reveived_byte = Serial.read();
    Serial.println(reveived_byte);
  }
}
```

Press Ctrl+Shift+M to open the Serial Monitor. Type some words and then press `Send`.

* `Serial.available()` returns the number of bytes in the serial buffer that send from PC to blue pill. Let's say if you send `Hello` in the Serial Monitor, the return value will immediately change from 0 to 6, as you have just typed 6 characters. ([Why 6 characters?](https://en.wikipedia.org/wiki/Null_character))
* `Serial.read()` returns the first character from the buffer.
* `Serial.println(reveived_byte)` prints the character that it just gets.

### Section Check Box:
* `Serial.available()`
* `Serial.read()`

### Try it yourself 04
**More than words**

In order to parse a integer from serial input, you may check the [ASCII table](https://en.wikipedia.org/wiki/ASCII#Character_set) and manipulate the input `char`. Alternatively, built-in function [Serial.parseInt()](https://www.arduino.cc/reference/en/language/functions/communication/serial/parseint/) can be used. Change the lines from
  ```
  char reveived_byte = Serial.read();
  Serial.println(reveived_byte);
  ```

  into

  ```
  int reveived_int = Serial.parseInt();
  Serial.println(reveived_int);
  ```

  How does it behave? What if your input is not an integer?

* What if the input I want is a
  * `float`?
  * `String`?

  Check [Serial - Arduino Reference](https://www.arduino.cc/reference/en/language/functions/communication/serial/) to find the see which function can get the job done.

## Appendix: FAQ
* **Q:** `STM32 Boards (STM32duino.com)` can not be seen under `Tools` → `Board: <something>`.
  * **A:** Make sure you folder is structured like this: `(Sketchbook location)\hardware\Arduino_STM32_master\(some folders e.g. drivers\ and tools\)`
* **Q:** `exec: '<something>\arm-none-eabi-g++': file does not exist`
  * **A:** Press `Tools` -> `Board: <something>` -> `Boards Manager`. Install `Arduino SAM Boards`.
* **Q:** `<some_path>/maple_upload: line 29: <some_path>/upload-reset: No such file or directory`
  * **A:** This is because `upload-reset` is a 32-bit executable. You may verify this through `file  some_path>/upload-reset`, which shows that it is a `ELF 32-bit LSB executable`.

    Install 32-bit shared libraries for AMD64: `sudo apt install libc6-i386`.

* **Q:** built-in LED keep flashing and unable to upload code
  * **A:** Press reset button and upload new code. Repeat a couple of times if it doesn't work. Notice that port name maybe changed after reset. If it still doesn't work, [flash the USB bootloader](#appendix-how-to-flash-usb-otloader) again.
* **Q:** `WARN src/stlink-common.c: unknown chip id! 0xa05f0000`
  * **A:** USB bootloader already uploaded. In order to overwrite the USB bootloader, press reset and immediately flash the new firmware with ST-Link.
* **Q:** `Access is denied` on Windows.
  * **A:** Don't install Arduino IDE from MS App Store. Install from [Arduino official website](https://www.arduino.cc/en/main/software) instead.

    _Reference: [windows 10 upload "Access is denied" error on bluepill - Arduino for STM32](https://www.stm32duino.com/viewtopic.php?t=3995#p48244)_
* **Q:** `fatal error: Arduino.h: No such file or directory`
  * **A:** Remove the folder `C:\Users\(your_user_name)\AppData\Local\Arduino15\`, then install `Arduino SAM Boards` from `Boards Manager` once again. If it still doesn't work, then try not to put you `Sketchbook location` under `\OneDrive`.
* **Q:** Blue Pill can not be found after being plugged, or `Access is denied`
  * **A:** Use another USB cable.
* **Q:** Still not found after replacing a USB cable.
  * **A:** There are other methods to upload program including using a USB-to-Serial adapter and using a ST-LINK, which can be found at M2. Check [Uploading a sketch - STM32duino wiki](https://wiki.stm32duino.com/index.php?title=Uploading_a_sketch) to read how it's done. Be aware that is the USB bootloader is erased after using either one of these method, so they can no longer be programmed via USB until USB bootloader is flashed to the board once again.
* **Q:** I found a typo in the article.
  * **A:** Please tell us.

## Appendix: how to flash USB bootloader

1. Download bootloader [generic_boot20_pc13.bin](https://github.com/rogerclarkmelbourne/STM32duino-bootloader/blob/master/bootloader_only_binaries/generic_boot20_pc13.bin). The version pc13 is used as the LED on blue pill is on PIN PC13.
2. Connect the ST-Link with the Blue Pill. `SWDIO`, `SWCLK`, `3.3V` and `GND` are the only pins required on ST-Link. Remember, **DON'T** provide power to the Blue pill from both ST-Link and USB.
3. Flash the bootloader.
    * (Linux)

      We may use `st-flash` that come along with Arduino STM32.
      ```bash
          cd /home/m2_is_neat/Arduino/hardware/Arduino_STM32-master/tools/linux64/stlink/ (Depends on where you put sketchbook folder)
          ./st-flash write /home/m2_is_neat/I_put_my_file_here/generic_boot20_pc13.bin 0x8000000
      ```

_Reference: [Flashing Bootloader for BluePill Boards · rogerclarkmelbourne/Arduino_STM32 Wiki](https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Flashing-Bootloader-for-BluePill-Boards)_

## References
* [Blue Pill - STM32duino wiki](https://wiki.stm32duino.com/index.php?title=Blue_Pill)
* [rogerclarkmelbourne/STM32duino-bootloader: Bootloader for STM32F103 boards, for use with the Arduino_STM32 repo and the Arduino IDE](https://github.com/rogerclarkmelbourne/STM32duino-bootloader)
* [Bootloader  rogerclarkmelbourne/Arduino_STM32 Wiki](https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Bootloader)

## Further Reading
* [STM32F103C8 Datasheet](https://www.st.com/resource/en/datasheet/stm32f103c8.pdf), which mentioned the physical properties of STM32F103, such as hardware specifications, ratings (e.g. supply voltage, output current), pinout, suggested circuit designs and many others.
* [STM32F1 Reference Manual](https://www.st.com/content/ccc/resource/technical/document/reference_manual/59/b9/ba/7f/11/af/43/d5/CD00171190.pdf/files/CD00171190.pdf/jcr:content/translations/en.CD00171190.pdf), which mentioned how a STM32F1 behaves and how to use STM32F1.

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))
* [Learning basic programming with Arduino](Arduino-Tutorials-(Learning-basic-programming-with-Arduino))