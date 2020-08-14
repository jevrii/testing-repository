**Next Tutorial: [Timer, PWM and Interrupts](Arduino-Tutorials-(Timer,-PWM-and-Interrupts))**

## Objectives
At the end of this self-learning lab, you should be able to:
* Know the meaning and usage of different data types
* Know how to use arithmetic operators, comparison operators and boolean operators
* Know how to use if…else, for, while statements

## Things you need
* A computer with Arduino IDE
* Your brain, as you are going to learn a new programming language (C++)!

## Variables
A variable is a way of naming and storing a value for later use by the program, such as data from a sensor or an intermediate value used in a calculation. Before variables are used, all of them have to be declared.
* When declaring a variable, we can choose…
    * Variable type
    * Variable name (the identifier of the variable)
    * Initial value (optional)
* For example -> `int width = 5;`
    * `int` is the variable type
    * `width` is the identifier (variable name)
    * `5` is the assigned value
* Variable Type
    * It tells the Arduino board the size of storage needed to store the data
    * Some basic variable types in Arduino

|Name|Description|Range|
|----|-----------|-----|
|int|Integers|-32,768 to 32,767|
|long|extended size variables for integer storage|-2,147,483,648 to 2,147,483,647|
|boolean|Boolean value|0 or 1|
|float|A number that has a decimal point|3.4028235E+38|
|double|Same as float in Arduino UNO||
|string|storing text, enclosed by `"…"`||

* Identifier (variable name)
   * An identifier must start with either
     * a letter (i.e., A to Z, and a to z), or
     * the underscore symbol (i.e., _)
   * The rest of the character may be
     * letters (i.e., A to Z, and a to z),
     * digits (i.e., 0 to 9), or
     * The underscore symbol (i.e., _)
   * Arduino is case-sensitive so radius, RADIUS, Radius, etc., are different
* A declaration specifies a type, and contains a list of one or more variables of that type
   * Syntax:
     * variable type variable name;
     * variable type variable name1, variable name2, …;
   * Example:
     * int distance, weight;
     * String saysth;
     * boolean win;
* A variable may be initialized in its declaration by using an equal sign followed by a value
   * Example:
     * `int distance = 5, weight = 10;`
     * `String saysth = `"Hello"`;`
     * `boolean win = true;`
* A variable that has not been given a value is said to be uninitialized, and will contain some garbage value. Using uninitialized variables in computations will give unexpected results, and thus should be avoided.

Here is an example regarding variables.

```c
void setup() {
  delay(1500);
  Serial.begin(9600);
  String saysth = "Hello";
  int distance = 5, weight = 10;
  Serial.println(saysth);
  Serial.println(distance);
  Serial.println(weight);
}

void loop() {
}
```

1. In addition to a sentence enclosed by `"…"` in Serial.println(), variables can also place inside for output. It is very useful in debugging.

### Section Check Box:
* Variable type: `boolean`, `int`, `long`,`float` ,`double` , `String`
* Identifier (variable name)
* Initialization of variables

### Try it yourself 01
**Variable type – char**

![Char](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_char.jpg)

There is also a variable type called `char` which has not been introduced before. Try to find out yourself what is char and how to declare a variable of `char`. Try to print the word `Hello` in a line by using `char`.

### Try it yourself 02
**Constants**

Instead of variables, we may also want to assign a fixed value to an identifier and don’t want to change its value anymore. Please search from the web to find out how to declare the variable as a constant, what is the use of it and what will happen if you change the value of the constant.

## Operators
Sometimes we want to combine variables to product new values or solve mathematical formulas. This can be done in Arduino through operands and operators.

### Operand
* Data on which the computation is performed
* May be variable or constant

### Operator
* Specifies what is to be done on the operands
* Including arithmetic operators, relational operators, logical operators, increment and decrement operators, Assignment operators
* e.g. — radius + 2 x 3
    * radius, 2 & 3 are operands
    * + & x are opeartors

### Arithmetic operators
* Modulus operator produces the remainder
  * e.g. 10 % 3 equals 1
* Integer division will discard any fractional part
  * e.g. 10 / 3 equals 3

|Arthmetic Operators|Sign in the expression|
|-------------------|----------------------|
|Addition|+|
|Subtraction|-|
|Multiplication|*|
|Division|/|
|Modulus|%|

### Try it yourself 03
**Remainder**

We know that remainder is got from division of 2 integers. What will happen if we use the Modulus operators on the variable type `double`? Please try to find it out.  

### Relational operators
* If the relation is true between 2 values (E.g. 15 > 8), the numeric value of the relation is 1, and 0 if the relation is false.

|Relational Operators|Sign in the expression|
|--------------------|----------------------|
|Greater than|>|
|Greater than or equal| >= |
|Smaller than|<|
|Smaller than or equal|<=|
|Equal|==|
|Not equal|!=|

### Try it yourself 04
**Relations**

What are the outputs of the following programs?
* `Serial.println(19 >= 8);`
* `Serial.println(“str” == “str”);`
* `Serial.println(3.14*100 != 314);`

### Logical operators
* `x && y` is true if and only if both x and y are true
* `x || y` is true if either x or y is true
* `!x` is true if x is false originally, and is false if x is originally true

|Logical Operators|Sign in the expression|
|-----------------|----------------------|
|And|&&|
|Or| \|\| |
|Not| ! |

### Try it yourself 05
**How’s your logic**

What are the outputs of the following programs?
* `Serial.println(5 > 6 || 5 >= 3);`
* `Serial.println(5 + 6 && 5 / 6);`
* `Serial.println(!18); `
* `Serial.println(5 && 6 || 7 && 8);`

### Increment and decrement operators
* Increment operator ++ adds 1 to its operand
  * i.e. sth++ is equivalent to sth=sth+1 
* ecrement operator -- subtracts 1 from its operand
  * i.e. sth-- is equivalent to sth=sth-1 
* The operators ++ and -- may be used either as prefix (e.g., ++sth) or postfix (e.g., sth++) operators

|Assignment Operators|Sign in the expression|
|-------------------|----------------------|
|Increment|++|
|Decrement|--|

### Try it yourself 06
**sth++ & ++sth**
As said above, sth++ has the same effect as ++sth. However, there is one point that is different between them, which sometimes leads to unexpected results in the program. Try to find out what is the difference. You can test it through the following program.
```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
}
    
void loop() {
  int a=5, b=6;
  Serial.println(a++ >= 6);
  a = 5; // reset the value of a
  Serial.println(++a >= 6);
}
```
### Assignment operators
* Using `=` will assign the value on the R.H.S. to the variable in the L.H.S.
* `a += 2` is equivalent to `a = a + 2`, same as others

|Assignment Operators|Sign in the expression|
|--------------------|----------------------|
|Assignment|`=`|
|Addition|`+=`|
|Subtraction|`-=`|
|Multiplication|`*=`|
|Division|`/=`|

### Try it yourself 07
**a \*= 5 + 1**

What is the value of a after the operation above? Assuming original value of `a` is 10. Is it equal to `a=a*5 = 50`, then adding the 1 back? Or equals to `a=a*(5+1)` which is 60?

### Assign a name to a Pin
Usually, we would like to give names to the pins we use in Arduino board to avoid confusion. In Lab 1, we have learnt to use the pin by typing the pin number directly. We can also give a name to the pin before we use it. There are mainly 3 methods of naming a pin and the naming should be placed before the setup() function.
```c++
#define FIRST_PIN PA0
int SECOND_PIN = PA1;
const int THIRD_PIN = PA2;
void setup() {
  pinMode(FIRST_PIN, OUTPUT);
  pinMode(SECOND_PIN, OUTPUT);
  pinMode(THIRD_PIN, OUTPUT);
}

void loop() {

}
```
Can you figure out the differences among them?
* [Constants - C++ Tutorials](http://www.cplusplus.com/doc/tutorial/constants/)
* [#define versus const dataType dataName \[ - C++ Forum](http://www.cplusplus.com/forum/beginner/28089/)

It is encouraged to named constants with `UPPER_CASE_WITH_UNDERSCORES`. Read [Naming conventions](https://github.com/m2robocon/m2_wiki/wiki/Writing-readable-programs#naming-conventions) for more details.

### Section Check Box:
* Operand & Operator
* Arithmetic operators +, -, *, /, %
* Relational operators >, >=, <, <=, ==, != 
* Logical operators &&, ||, !
* Increment and decrement operators ++, --
* Assignment operators =, +=, -=, *=, /=
* Naming a pin

### Try it yourself 08
**Precedence & Associativity**

In the exercise above, you should find out that what operator is carried out first will dictate the value. There are a list showing which operation has a higher priority. In evaluating an expression with mixed operators, those operators with a higher priority will be carried out before those with a lower priority. How about operations with the same priority? It is determined by the Associativity rule. Yet, the order of evaluation may be overridden by inserting parentheses `()` into the expressions.

|Operators|Associativity|Precedence|
|---------|-------------|----------|
|!||Highest|
|*, /, %|left to right||
|+, -|left to right||
|<, <=, >, >=|left to right||
|==, !=|left to right||
|&&|left to right||
|\|\||left to right||
|=, +=, -=, *=, /=, %=|right to left|Lowest|

To ensure that you understand this, let’s try to do some exercise. What is the value of the programs below?
* 6 * 3 /  4 * 5
* 2 * ( 3 + 4 )
* 1 + 2 / 3 * 4 + 5 

## Flow of Control
In programs, it is often necessary to alter the order in which statements are executed and determine whether a statement should be executed or not. The order in which statements are executed is often referred to as flow of control. Flow of control can be divided into branching and looping. Branching includes if-else statement and switch statement. Looping includes while statement and for statement. 

### If-else statement
```c++
if (boolean_expression)
  yes_statement;
else
  no_statement;
```
* Boolean expression is an expression in a programming language that produces a Boolean value when evaluated, i.e. one of true or false.
* When an if-else statement is executed, the boolean_expression will be first evaluated
  * If it is true, yes_statement will be executed
  * If it is false, no_statement will be executed

```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
}

void loop() {
  int a = 5;
  if (a > 2)
    Serial.println("a is larger than 2");
  else
    Serial.println("a is not larger than 2");
}
```

### Compound Statements
* It is possible to execute more than one statement in each branch of an if-else statement by using compound statements
* A compound statement is simply a list of statements enclosed within a pair of braces {}

![example of compound statements](/m2robocon/m2_wiki/blob/master/images/(Arduino_Tutorials)_compound_statements.png)

Here is an example in Arduino:

```c++
void setup() {
  delay(1000);
  Serial.begin(9600);
  
  int a = 4, b = 8, c = 4;
  if (a > 2)
  {
    Serial.println("a is larger than 2");
    b = -b - sqrt(b*b-4*a*c) / (2*a);
    Serial.println(b);
    Serial.println("Peter is cool");
  }
  else
  {
    Serial.println("a is not larger than 2");
    b = -b - sqrt(b*b-4*a*c) / (2*a);
    Serial.println(b);
    Serial.println("Peter is an idiot");
  }
}

void loop() {
}
```

### Try it yourself 09
**If-else problem**

Consider the following program: 
```c++
void setup() {
  delay(1000);
  Serial.begin(9600);
  
  int a = 5;
  if (a >= 5)
  {
    a--;
    Serial.println("write sth");
  }
  else
  {
    Serial.println("write sth else");
  }
}

void loop() {
}
```

As a>=5 originally, if statement will be executed, which is `a--`. Now a is not larger than or equal to 5. Will the else statement be executed?

### Try it yourself 10
**Multi-way if-else Statement**

Sometimes we want to make several choices in one if-else statement. For example, if a<=5, then do something. If 5<a<8, do another thing. If a>=8, do other tasks. This can be done using “else if” statement. Find out more about it by yourself.
Also, consider the following case:

```c++
void setup() {
  delay(1500);
  Serial.begin(9600);

  int a = 6, b = 1;
  boolean result;
  if (a > 3)
    result = true;
  else if (a % 2 == 0)
    b--;
  else
    result = false;

  Serial.println(result);
}

void loop() {
}
```
As a > 3, if statement will be executed. Yet, else if statement is also satisfied in this case. Will the else if statement be executed? Try to find out the answer by yourself.

### Switch statement
A multi-way branching action can also be achieved using a switch statement
```c++
switch (controlling_variable){
  case value1:
    statements;
    break;
  case value2:
    statements;
    break;
  …
  default:
    default_statements;
}
```
The value given after the case keywords are checked in order until the first that equals the value of the controlling variable is found, and then the following statement(s) are executed

If none of the constants matches the value of the controlling variable, then the default statement(s) are executed

The controlling variable in a switch statement must be an integer (`int`), boolean (`bool`), or a character (`char`)

### Try it yourself 11
**Multi-variables in controlling variable**

Sometimes, we want to determine the relation between 2 variables. Can we write a code like this? If not, how to fix it?

```c++
void setup() {
}

void loop() {
  int a = 25, b = 14;
  int result = 0;
  switch (a * b){
    case (a * b > 100):
      result = 111;
      break;
    case (a % b == 0):
      result = 101;
      break;
    default:
      result = 100;
  }
}
```
You should notice that there is a statement `break;` after the statement of each case. Find out what is it and why the line after default statement do not need to add the break statement? 

### while loop
```c
while (boolean_expression) {
  statement_1;
  statement_2;
  ...
  statement_n;
}
```
* When a while statement (aka while loop) is executed, the boolean_expression is evaluated. 
  * If it is true, the loop body is executed once (i.e., one iteration)
  * If it is false, the loop ends without executing its body
* After each iteration, the boolean_expression will be evaluated again and the process repeats.
  * While loop is used when we want to repeat some statements again and again as long as the specified condition is true.
```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
  int a = 5;
  while (a < 10) {
    Serial.println(a);
    a++;
  }
}

void loop() {
}
```
As long as a < 10, the value of a will be printed.

### Try it yourself 12
**Recursion using while**

* Write a program that prints the first 100 odd numbers starting from 1. 
* Write a program that prints the first 20 square numbers.

### For loop
We will usually want to end the loop after several iterations or after a specific condition. Apart from counting the iterations in while loop, we can also use for loop which is more convenient to use.
```c++
for ( initialization ; condition ; updating ) {
  statement_1;
  statement_2;
  ...
  statement_n;
}
```
When a for loop is executed
* The initialization is performed to set the initial value of the loop variable
* The initialization is executed only once
* The condition is checked
  * If it is true, the loop body is executed once (i.e. one iteration)
  * If it is false, the loop ends without executing its body
* After each iteration, the updating of loop variable is performed and the condition is checked again to determine whether a new iteration starts
```c++
void setup() {
  delay(1500);
  Serial.begin(9600);

  int a = 5;
  for (int i = 0; i < 20 ; i++) {
    a *= 2;
  }

  Serial.println(a);
}

void loop() {
}
```
In the program above, the value of a will be doubled for 20 times. It will end when the value of i reaches 20.

### Try it yourself 13
**Recursion using for**

* Write a program that calculates the sum of odd numbers between 1 and 20
* Write a program to find those numbers which are divisible by 7 and multiple of 5, between 1500 and 2700 (both included).

### Loop inside a loop 
Sometimes, we many want to perform iterations inside a loop. It is allowed in Arduino, just like the program below.
```c++
void setup() {
  delay(1500);
  Serial.begin(9600);

  int a = 0;
  for (int i = 0; i < 10; i++) {
    for (int j = 10; j > 0; j--) {
      a += i + j;
    }
  }

  Serial.println(a);
}

void loop() {
}
```
### Try it yourself 14
**Recursion using for (2)**

Write a program to construct the following pattern, using a nested for-loop.
 
    * 
    * * 
    * * * 
    * * * * 
    * * * * * 
    * * * * 
    * * * 
    * * 
    *

Write a program to print the Fibonacci Sequence starting from 1 till the maximum value smaller than 1000.

### Section Check Box:
* if-else statement
* switch statement
* while statement
* for statement

### Try it yourself 15
**break & continue **
We may want to exit a loop or start a new iteration immediately without executing the remaining statements in the loop in some situations. This can be done by using `break` and `continue`. Try to find out how to use them and what is the difference between them.

Write the following program using `break` and `continue`.
* Print the first 50 even numbers using `continue`
* Initiate `a` to be 5. Add 1, then 2… until the value of `a` exceed 1000 by using` break`.

## Scope of variables & global variables
The scope of a variable is the portion of a program in which the variable can be used. A variable cannot be used beyond its scope.
* The scope of a local variable starts from its declaration up to the end of the block (delimited by a pair of braces { }).
* Variables can be declared with the same identifier as long as they have different scopes. Variables in an inner block will hide any identically named variables in outer blocks. However, this is confusing and should be avoided.

Global variable is a variable declared outside all functions. Local variable is a variable declared within a function.
* Global variables can be accessed by all functions.
* Global variables remain in existence permanently.
* Global variables should be avoided in ordinary programming as they can be changed by several functionsare and hence very hard to trace. However, it is inevitable in Arduino as it is the only way to store states across loop().
* It is encouraged to named a global variables with `under_scored` and a leading `g_` added, e.g. `g_my_variable`. Read [Naming conventions](https://github.com/m2robocon/m2_wiki/wiki/Writing-readable-programs#naming-conventions) for more details.


### Try it yourself 16
What's the output of the program? Why it behave like that?

```c++
int first_int = 1, second_int = 1; // Global variables
void setup() {
  delay(1500);
  Serial.begin(9600);

  {
    int first_int = 2; // Local variable in a inner block
    second_int = 2;
    Serial.print("inside a block: ");
    Serial.print(first_int);
    Serial.print(" ");
    Serial.println(second_int);
  }

  int first_int = 3; // Local variable
  second_int = 3;
  Serial.print("inside setup(): ");
  Serial.print(first_int);
  Serial.print(" ");
  Serial.println(second_int);
  
}

void loop() {
  Serial.print("outside setup(): ");
    Serial.print(first_int);
    Serial.print(" ");
    Serial.println(second_int);
  delay(10000);
}
```

## Array
Array a collection of data of the same type. Similar to list in Python but with a fixed size.
* Syntax: `dateType identifier[array_size];`
  e.g. `int my_int_array[5];`
* Array can be initialized in its declaration by using an equal sign followed by a list of values enclosed within a pair of braces.
  e.g. `int my_int_array[5] = {1, 2, 3, 4, 5};`
* Each element of an array can be regarded as a variable of the base type, which can be accessed through `identifier[index]`.
  e.g. `my_int_array[0] = 100;`, `Serial.println(my_int_array[1]);`
* Array indexes always start from 0 and end with the integer that is one less than the size of the array.
*. **NEVER** access beyond the size of the array.
  e.g. if the size of  `my_int_array` is 5, reading or writing `my_int_array[5]` and beyond has a unpredictable behaviour. 

```c++
void setup() {
  delay(1500);
  Serial.begin(9600);

  int int_list[5] = {1, 3, 5, 7, 9};
  for (int i = 0; i < 5; i++)
    Serial.println(int_list[i]);

  int_list[0] = 2;
  int_list[4] = 11;
  for (int i = 0; i < 5; i++)
    Serial.println(int_list[i]);
}

void loop() { 
}
```

### Try it yourself 17
* Initialize a `char` array with `Hello, World!` as its content and print it.
  Note: A `char` array is also called a "C-style string", as there's no `String` in C.

* Read and print serial input using a C-style string.
  Note: serial input (and generally every C-style string) end with a '\0'.


```c++
void setup() {
  delay(1500);
  Serial.begin(9600);
}

void loop() { 
  char input_string[20];
  int input_string_idx = 0;
  while (Serial.available() == 0);
  
  while (Serial.available() > 0) {
    input_string[input_string_idx] = Serial.read();
    input_string_idx++;
    if (input_string_idx >= 20)
      break;
  }

  for (int i=0;i<20;i++){
    if (input_string[i] == '\0')
      break;
    Serial.print(input_string[i]);
  }
  Serial.println();
}
```

### 2D Array
It is possible to have an array of array (a.k.a. 2D array).

```c++
void setup() {
  delay(1500);
  Serial.begin(9600);

  int always_equal_15[3][3] = {{2, 7, 6},{9, 5, 1},{4, 3, 8}};
  for (int i=0; i<3; i++){
    for (int j=0;j<3;j++){
      Serial.print(always_equal_15[i][j]);
      Serial.print(' ');
    }
    Serial.println();
  }
}
void loop() {
}
```

### Try it yourself 18
* Try to flip `always_equal_15` over its diagonal when printing it out.

## Functions
In Arduino, sub-tasks can be implemented as functions. A function is a group of statements that is executed when it is called from some point in program. A program is composed of a collection of functions. When a program is put into execution, it always starts at the setup function and loop function, which may in turn call other functions. Check out the following code:

```cpp
int my_function(int my_arg) {
  if (my_arg >= 0) {
    return my_arg;
  } else {
    return 0;
  }
}

void setup() {
  delay(1500);
  Serial.begin(9600);
}

int a = 5;

void loop() {
  Serial.println(my_function(a));
  a--;
  delay(1000);
}
```

Function appears in the following form:

```cpp
return_type function_name ( type1 parameter1, type2 parameter2, ... ) {
  // variable declarations ...
  // executable statements ...
}
```

* In the above code, “my_function” is a function. It has return type “int”, one parameter which is “my_arg”, “my_arg” has a variable type “int”.
* Function Call
  * A function call (i.e., the process of calling a function) is made using the function name with the necessary parameters.
  * A function call is itself an expression, and can be put in any places where an expression is expected.
  * The statement: function(a) is a function call.
* return
  * The value returned by the function is determined when the function executes a return statement
  * A return statement consists of the keyword return followed by an expression
  * When a return statement is executed the function call ends
  * Thus, in the above code, if a>100, then the return statement is executed.
* Parameters and Arguments
  * Parameters are variables that are part of the function definition
  * The expressions used to pass values to a function during a function call are referred to as arguments
  * When a function is called the computer substitutes the first argument with the first parameter, the second argument with the second parameter, and so forth
  * The arguments used in a function call can be constants, variables, expressions, or even function calls
  * In above code, “my_arg” is the parameter and “int” is its type.
  * Thus, digitalWrite(pin,value), pinMode(pin, value) etc. are all functions, though the function itself is hidden from you.
* void Functions
  * In some situations, a function returns no value
  * In this case, we use the void type specifier
  * A function with no return value is called a void function
  * The return statement in a void function does not specify any return value. It is used to return the control to the calling function
  * If a return statement is missing in a void function, the control will be returned to the calling function after the execution of the last statement in the function
* Read more from [Function Declaration - Arduino](https://www.arduino.cc/en/Reference/FunctionDeclaration)

### Section Check Box:
* function
* function call
* return 
* parameter
* void

### Try it yourself 19
**Functions**

Try to write a function that can calculate addition, subtraction, multiplication and division at once.

```cpp
double calculator(double a, char sign, double b){
  // write you code here...
}

void setup() {
  Serial.begin(9600);
  Serial.println(calculator(3.14,'+',3.14)); // 6.28
  Serial.println(calculator(3.14,'-',3.14)); // 0
  Serial.println(calculator(6,'*',7)); // 42
  Serial.println(calculator(1,'/',0)); // what will happen?
}

void loop() {
}
```

What is the problem of the following code? Can you correct it?

```cpp
void setup() {
  Serial.begin(9600);
  double a = 15.1;
  func(a);
}

double func(int val){
  return a * 13.1;
}

void loop() {
}
```

## See Also
* [Arduino Tutorials (Index)](Arduino-Tutorials-(Index))
* [Arduino Tutorials (Learning basic control)](Arduino-Tutorials-(Learning-basic-control))