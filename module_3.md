## Module 3: Working with Java Data Types

### Lesson 1: Declare and initialize variables (including casting of primitive data types)

#### 1.1 Using the general form of simple declarations

* Variables can be declared as members declares as part of an object, or local variables in a method
  * Class variables can be called member variables, members, instance variables or attributes
  * Local variables are called method locals, stacked variables or automatic variables
* Everything applied to local variables is true of member declarations too, there are additional modifiers that may be applied to member declarations
* An identifier must start with a letter, an underscore or a dollar symbol, after the first character we can have any number of letters, numbers, underscores or dollar signs
* Member variables may also have an accessibility
  * Public
  * Private
  * Protected
  * No accessibility specified means default accessibility
* We can declare multiple variables of the same type on the same line

``` java
int x, y, a, b;
```

#### 1.2 Using the general form of initialized declerations

* Local variables in methods do not have values until we explicitly give them ones, member variables have default values
* numeric types -> 0; boolean -> false; reference types -> null
* We can assign multiple variables on one line

``` java
int x = 100, y = 99, z = x + y;
```

#### 1.3 Understanding integer primitive types, literal forms

* Java has 4 data types that are intended as integer types, to hold whole numbers
  * byte - 8 bits `-128 - +127`
  * short - 16 bits `-32,768 - +32,767`
  * int - 32 bits `+/- 2,147,000,000 ish`
  * long - 64 bits `+/- 9,223,000,000,000,000,000 ish`
  * char - 16 bits `65535 (stores no negative values)` - (intended to represent characters, but can perform arithmetic on it in calculations)
* The Java system allows an implementation to use more storage if desired, this is all hidden in the JVM though
* Integer numbers have other literal forms that can be used in code
  * decimal - regular numbers
  * octal - start with `0` (digits from 0-7 as octal)
  * hexadecimal - start with `0x`
  * binary literals - start with `0b` (only 1s and 0s)
  * long - append `L` to end (lowercase is permitted also)
* We can assign e.g. `short s = 1234;`, but not in a method call, or anything computed at runtime
* We can use seperators between digits, an underscore can be used, as long as it's between two digits, not next to a decimal point and not next to an `x` or `b` in hex and binary numbers

#### 1.4 Understanding floating point primitive types, literal forms

* Java provides `float` (32 bits) and `double` (64 bits), stored in a special way allowing a greater range than int but with less precision (e.g. large numbers or fractions) - floats are not always exact, and we can see rounding errors
* A number with a decimal point will always be stored by default as a `double` literal
* We can use scientific exponent notation, with a number and e and another number (positive or negative), e.g. `34.532E42` or `12e2`
* We can create a double literal by using a simple decimal literal integer type and adding a `D` or `d` to the end
  * This cannot be used with hex, octal or binary format
  * This is redundant as as soon as the compiler realises you have a floating point literal format it creates a double by default
  * We must explicitly append to a literal an `f` or `F` to make a float
  * We can represent NaN as a constant declaration in the Double with a capital D or Float with a capital F classes
  * These aern't the same and they don't even equal themselves
  * This can represent an incomputable result, e.g. positive/minus infinity

#### 1.5 Understanding logical and character primitive types, literal forms

* `boolean`
  * `true` or `false` values, these are not numbers and only these values can be used
* `char`
  * Single characters, using 16 bit UNICODE representation
  * we use single quote marks to surround the character
  * we can create a char literal using the unicode character number, e.g. `'\uXXXX'` - the characters are not case sensitive but the `u` is
  * These are converted by the input reading part of the Java compiler
  * We can use characters in the `\u` format as part of the sourcecode, e.g. as part of a variable name
  * Characters such as `\u000a`, the linefeed character, when used in source code will not make it compile
  * We can get around this by escaping this, e.g. `\n` makes a newline
  * `\b` or `08` backspace
  * `\t` or `09` tab
  * `\n` or `0a` linefeed
  * `\f` or `0c` form feed
  * `\r` or `0d` carriage return
  * `\"` or `22` double quote
  * `\'` or `27` single quote
  * `\\` or `5c` backslash
  * We can use octal representation for characters with not more than 8 significant bits, these don't have a leading zero, numbers followed by a backslash `\` are assumed to be octal
  * The largest octal escape value is `\377`

#### 1.6 Casting primitive types

* Casting is a version of polymorphism
* Java is a strongly and statically types language, meaning every item of data in our program has a precise type, known at compile time
* We can use cast operator, parentesies containing a type name, e.g. `(byte)`
* This only creates a new temporary value
* If we attempt to assign a value to a primitive type, which doesn't have enough bits, it'll only take the X most significant bits

### Lesson 2: Differentiate between object reference variables and primitive variables

#### 2.1 Using the == operator with primitives and references

* Primitive types are: `boolean, char, byte, short, int, long, float, double`
  * The variable stores the value that it represents (written directly to variable)
* Reference types are: `Float, Double` (all non primitive types start with uppercase letter)
  * We allocate at least 4 bytes that can point at/refer to an object location
  * When we cahnge the object, the object in memory is simply changed (the pointer is not)
* The `==` operator compares two values to see if they're equal or not
  * If we perform this on two primitives, it directly compares the values
  * If performed on reference types, it will only return true if they're pointing to the exact same reference in memory, even if both contents are the same

#### 2.2 Understanding method argument passing

* We always pass by value in methods
* When we pass primitive values into a method, we copy the value, any changes made in the method are only in the scope of the method, and do not change the initial variable
* When we pass an object, we pass a copy of the reference to the object, therefore any changes to the object in the method change the original object

### Lesson 3: Know how to read or write to object fields

#### 3.1 Selecting a field from a reference expression

* To access a field in an object, we either need to have a variable that refers to the object directly, or an expression that resolves to a reference to the object

``` java
public class Thing {
  public int numOne;
  public long numTwo;
}

Thing t = new Thing(); // The variable t is an expression (value that has a type at compilation time, and has a value at runtime)

t.numOne = 7; // This is a L-value expression (can be put on the left hand side of an assignment)
int value = t.numOne // We can read value of t, and assign to value
```

* We can also assign and read using array notation

``` java
public class Box {
  public Thing getOneThing() {
  return new Thing();
  }
}

Box b = new Box();
b.getOneThing(); // Type Thing, produces reference to a Thing
b.getOneThing().numOne // L-value expression, accessing Thing object created in method

public Thing[] getManyThings() {
  Thing[] ta = // Partially initialise array with Things
  return ta;
}

Box b = new Box();
b.getManyThings(); // Type reference Array of Thing
b.getManyThings()[0]; // Type reference to a Thing
b.getManyThings()[0].numTwo; // Type long
```

#### 3.2 Using "this" to access fields

``` java
public class Thing{
  public int numOne;
  public void doStuff() {
  this.numOne = this.numOne + 10;
  }
}

Thing t1 = newThing();
Thing t2 = newThing();

// These point to two different Thing objects, each object has a field, numOne
```

* Methods e.g. `doStuff` are assumed to belong to an object and the method executes within the context of the object, known as `this`
* `this` is an expression of the type of the enclosing class
* Method code is read only, so it's easy to share with all objects which use that method when using `this`
* The `this` is just a copy of the object passed in to the method at invocation, and during the scope of `this`, the object cannot be mutated from outside the method
* A variable not prefixed with `something-dot`
  * A method local variable
  * Member variable with an implied `this`
  * It's a static variable in a class that encloses the variable
  * It's a static import from a class that does not enclose the variable

#### 3.3 Code examples

``` java
package fieldaccess;

public class Box {

  private Thing[] things = {
  new Thing(), new Thing(), new Thing()
  };

  public Thing getOneThing() {
  return things[2];
  };

  public Thing[] getManyThings() {
  return things;
  };

  public void showThings() {
  System.out.println("Things:");
  for (Thing t : things) {
  System.out.println(t.toString());
  }
  }
}

package mainmethod;

Box b = new Box();

System.out.println("numOne in a boxed thing is " + b.getOneThing().numOne);
b.getOneThing().numOne = 5432
System.out.println("numOne in a boxed thing is " + b.getOneThing().numOne);
```

Returns:

``` java
numOne in boxed thing is 0;
numOne in boxed thing is 5432
```

### Lesson 4: Explain an Object's Lifecycle (creation, "dereference by reassignment" and garbage collection)

#### 4.1 Understanding allocation and referencing

* The garbage collector is responsible for working out whether an object might be used again, and for releasing any objects back to the heap memory when they are identified as never to be used again
* The system can only determine if it is not possible to use an object again
* If an object can be used again, but is not used again, it has the possibility of memory leak
* An object is always accessible through a variable of reference type
  * When this variable goes out of scope, it can be garbage collected
  * If they can be used late,r they're called `reachable` and won't be subject to gc
  * Only unreachable objects are elegible for garbage collection
  * The risk of releasing an object prematurely is completely avoided

#### 4.2 Collecting Garbage

``` java
aMethod() {
  StringBuilder sb = new StringBuilder();
```

* If we set `sb = null`, the `StringBuilder` object becomes eligible for collection
* Every executing method, including methods further up the call tree have live stack frames
* Every variable in a live stack frame is a live variable
* Every object referred to by a live variable is a live object
* Every variable in a live object is a live variable
* Any other object referred to by that variable is a live object

### Lesson 5: Develop code that uses wrapper classes such as Boolean, Double and Integer

#### 5.1 Understanding and identifying wrapper objects, understanding autobixing

* Wrapper classes let us associate behaviour with data, with integrity protections
* We can use Java's (`Java.lang.Object`) generalization mechanisms, allowing any data looked at this way to be referred to as an object
* Primitives are not object, so we cannot use generalization
* Primitives do not have the same kind of data associated as with objects
* Wrapper classes let us see primitive types as objects, e.g. add numbers into lists
* The compiler generates code to automatically create a wrapper around the primitives, called autoboxing (or autounboxing in the context of unboxing primitives from objects)

``` java
boolean	-> Boolean
char -> Character
byte -> Byte
short -> Short
int -> Integer
long -> Long
float -> Float
double -> Double
```

#### 5.2 Investigating the API of Integer

``` java
Integer.BYTES		//	num of bytes used to represent int
Integer.MAX_VALUE	// max val can hold
Integer.MIN_VALUE	// min val can hold
Integer.SIZE			// num of bits used to represent int

Integer(int value) // converts int to Integer
Integer(String s)	//	converts string to int

Integer.byteValue()	// returns val as byte after primitive conversion
Integer.compareTo()	// compare objects numerically
Integer.doubleValue()	//	val of int as double
Integer.equals()		// compares to other object
```

* On the command line, we can define system properties with a `-D`, e.g.

``` java
java -Dmy.value=99

String val = System.getProperty("my.value"); // val is "99"
Integer value = Integer.getInteger("my.value"); // val is 99
```
