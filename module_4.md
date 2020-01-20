## Module 4: Using Operators and Decision constructs

### Lesson 1: Use Java operators; including parentheses to override operator precedence

#### 1.1 Using operators, operands and expressions

* Operators tell the compiler to run code, predefined in the system, more usually operating on primitive types
* Infix operator `expression <operator> expression` - operator in middle
* Prefix operator `<operator> expression`
* Postfix operator `expression <operator>`
* Java does not permit programmer defined operator overloading (but `+` and `-` invoke different code when working with floating point numbers, rather than integers)

#### 1.2 Using arithmetic operators + - * / %

* % operator is modulus
  * 5 % 3 = 2
  * 10 % 4 = 2
  * -10 % 3 = -1
  * -10 % -3 = -1
  * -10.5 % -3.1 = -1.2
* We can get overflow when we perform operands on numbers fitting in their respective types, but the result does not fit in
  * We get strange results, due to unsigned bits etc.
* Division by zero represents arithmetic exception
* Floating point overflow sets result to infinity (NaN)

#### 1.3 Using the plus operator with Strings

* The `plus()` operator can be used with either String or numeric types
  * `String + AnyType` => String concatenation (the other type is converted to string, possibly using the object's `toString()` method)
  * `NumericType + NumericType` => Addition
  * `AnyOtherCombination` => Compiler Error
* Java expressions are evaluated left to right
  * `"Hello" + 1 + 2` = `Hello12`
  * `1 + 2 + "Hello"` = `3Hello`

#### 1.4 Promoting operands

* Promotion means representing smaller numbers as larger ones

``` java
if one operand is a double, the other is promoted to double
else if one operand is a float, the other is promoted to float
else if one operand is a long, the other is promoted to long
else int
```

* If you add `short`, `char` or `byte`, we get an `int` result

``` java
short s = 99;
byte b = 10;
s = b + s;  // compiler error! (this is an int)
```

#### 1.5 Using increment and decrement operators

* We can increment or decrement `L-values` (values on left side of assignment)

``` java
int x = 99;
int y = ++x;  // Pre-Increment
// y => 100 & x => 100

int x = 99;
int y = --x;  // Pre-Decrement
// y => 98 & x => 98

int x = 99;
int y = x++;  // Post-Increment
// y => 99 & x => 100

int x = 99;
int y = x--;  // Post-Decrement
// y => 99 & x => 98
```

``` java
int[] xa = {0,1,2,3,4};
int idx = 0;
System.out.println("value is " + (xa[idx++] >= 0 && idx > 0));

// Value is true
```

``` java
int[] xa = {0,1,2,3,4};
idx = 0;
xa[++idx] = xa[idx] + 1000;
for (int v : xa) {
  System.out.println("> " + v);
}

/*
 * > 0
 * > 1001
 * > 2
 * > 3
 * > 4
```

* Increment and Decrement operators result in an expression the same size as the expression they're operating on (e.g. `short`, `byte` and `char` stay as they were)

#### 1.6 Using shift operators

* Binary numbers read from right to left

``` java
64 32 16 8  4  2  1

--------------------------

0  1  0  1  0	 1  0  =  42

42 >> 1

0  0  1  0  1  0  1  =  21
```

* We lose precision as bits can drop off the end
* The compiler optimises this for us
* We might only use this bit shifting for specific applications, e.g. network protocol packets
* We can use `>>>` for unsigned shifts, where extra bits don't come in (bringing in 0s from the left rather than 1s) - this only works with Int and Long types
  * If we try this with e.g. a short, we pad the left out with 0s, to the length of an Int

#### 1.7 Using comparison operators

* `<` less than
* `<=` less than or equal to
* `>` greater than
* `>=` greater than or equal to

* These can be used on primitive and wrapper types, but not `Number` type

* `==` equal
* `!=` not equal

* Strings that are the same share the same object, even if created differently, therefore will return `true` when used with comparison

#### 1.8 Using logical operators

* `!` boolean not
  * `!(x > 99)` == `(x < 99)`

* `~` tilde (bit pattern not)
  * `~000` == `111`

* `&` and
  * `1 & 1 -> 1`
* `|` or
  * `0 || 0 -> 0`
* `^` xor
  * `1 ^ 0 -> 1`
  * `0 ^ 1 -> 1`
  * `0 ^ 0 -> 0`
  * `1 ^ 1 -> 0`

#### 1.9 Using short-circuit operators

* `&&` - If first operand evaluates to false, second operand is never computed at all
* `||` - If first operand is true, then second is not calculated
  * Both of these are only applicable to boolean values

#### 1.10 Using assignment operators

``` java
int x, y, z;
x = y = z = 0;
```

* Arithmetic operators: `+=`, `-=`, `*=`, `/=`, `%=`
* Boolean operators: `&=`, `|=`, `^=`
* Bitwise operators: `<<=`, `>>=`, `>>>=`

These operators do not promote values to `int`, the assignment operator casts its computed result into whatever type the assignment is assigned to

* The left hand side is guaranteed to be evaluated once, and once only

#### 1.11 Understanding assignment compatibility

* A `char` is unsigned, so a `byte` or `short` cannot represent or be represented by a `char`

* The case operator has _HIGH_ precidence

``` java
int x = 99;
short s = x; // CompileError
short s = (short) x; // All ok using cast operation
```

* We cannot cast non numeric types to numeric types (we can convert, however)
* Autoboxing can create objects from primitive types, but these are conversions

#### 1.12 Understanding other elements of expressions

`<reference> instanceof <classtype>`

``` java
int x = 0;
if (x instanceof Object)  // Error, x is not reference
```

``` java
String s = "Hello";
Class cl = String.class();

if (s instanceof cl) // Error, must be class literal

if (s instanceof String) // Ok!
```

``` java
String s = "Hello";

if (s instanceof Number) // Impossible, error
```

``` java
null instanceof Object // Always false
```

``` java
String s = null;

if (s instanceof String) // Not executed, returns false
```

* The `instanceof` operator tells us if we can cast the left hand value to the value on the right

``` java
t -> reference to a thing
. -> dot operator selects a member
x -> member to be selected
```

``` java
String s = "Hello";

s -> expression of type String
. -> dot selects an element of expression
length() -> identifies element to select, () calls method
```

#### 1.13 Using parentheses and operator precendence

* The precedence of multiplicative (multiplication, division and modulo) operations is higher than that of additive operations

``` java
4 + 4 / 2 = 6
(4 + 4) / 2 = 4
(3 + 4 / 2) * 7 = 35
```

* Java evaluates from left to right
* We must match number of parentheses, otherwise we get compilation error

### Lesson 2: Test equality between Strings and other objects using == and equals ()

#### 2.1 Understanding the meaning of == and the intended meaning of equals ()

``` java
Date d1 = new Date();
Date d2 = new Date();
Date d3 = d2;

d2 == d3;
d1 != d2;
```

* Providing the `.equals()` method has been applied correctly to the class, we can check if the objects represent the same values
* For `StringBuilder()` objects, it does not have an equals method to compare it's contents, so will return false

#### 2.2 Determining if equals() is implemented, and implementing equals()

* All methods inherit the `equals()` method, even if they don't provide it explicitly
* As a default, it provides the same as `==` if not overriden

### Lesson 3: Create if and if/else and ternary constructs

#### 3.1 Understanding the basic form of if and if/else

* Most basic form:

``` java
if (<boolean expression>) {
  <statement>
} else {
  <other statement>
}
```

#### 3.2 Using braces with if/else. Effect of "else if"

* If we want multi-line statements, we must use braces to form a block (otherwise we may get a dangling else)
* Else statements must attach to closest if statement (unless blocks are used)

#### 3.3 Understanding the if / else if / else structure

``` java
if (<boolean expression>) {
  <statement>
} else if (<other boolean expression){
  <other statement>
} else {
  <other statement>
}
```

#### 3.4 Using the ternary operator

``` java
<boolean> ? <value A> : <value B>

if <boolean> then <value A> else <value B>
```

``` java
int result = doTheTest()
  ? getOneValue()
  : getOtherValue();
```

* If the second and third results (? and : results) are the same type, we will get the same type back, e.g:

``` java
<b> ? (byte) 10 : 10 + 23 => <byte>

<b> ? <byte> : <long> => <long>

<b> ? <Byte> : <int> => <int>

<b> ? <float> : <double> => <double>
```

### Lesson 4: Use a switch statement

#### 4.1 Using the general form of switch, case, break and default

``` java
int x = ??

switch(x) {

  case 10: {
  ...;
  break;
  }

  case 20:
  ...;
  break;

  default:
  ...;
  break;
}
```

* The case value must be computable at runtime (no method calls, like with what's allowed when using `if`)

#### 4.2 Code examples for the general form of switch

``` java
int day = 1

switch (day) { // Opening and closing braces necessary
  case 0: // Colon necessary
  System.out.println("Monday");
  break;
  case 1: // Needs to be a constant expression
  System.out.println("Tuesday");
  break;
  default:
  System.out.println("No Day!");
  break;
}
```

* We do not need break statements, but the program will loop through all options after matching case

#### 4.3 Understanding break

* If we omit the `break` statement, we will _fall through_ and execute statements that are otherwise associated with other cases

``` java
switch(x) {
  case 1:
  case 2:
  case 3:
  case 4:
  System.out.println("Weekday");
  break;
  case 5:
  case 6:
  System.out.println("Weekend");
  break;
}
```

* The program will print 'Weekday' for cases 1 through 4, and 'Weekend' for cases 5 through 6
* The order of the cases matters to the logic of the code, the compiler works top down
* Falling through just lets us hand-optimise code

#### 4.4 Identifying switchable types

* We're allowed to switch on: `int`, `short`, `byte`, `String`, `char` and `enum` (set of named values)
* We're *not* allowed to switch on: `long`, `float`, `double`, `boolean`
  * Optimisation is built into the implementation of switching on Strings
