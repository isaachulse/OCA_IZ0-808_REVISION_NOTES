## Module 2: Java Basics

### Lesson 1: Define the scope of variables

#### 1.1 The meaning of scope, blocks and curly braces

* The scope of a variable is where about in the program source you can use this variable used by it's simple name (e.g. if you have a variable, `egg` in class `Food`, `Food.egg` would not be classed as a simple name, whereas being able to use jusrt `egg` would)
* Initially, the scope of the variable is defined within the curly braces where it's declared until those curly braces close.

#### 1.2 Special cases of scope

* These occur under simple `for loops`, and in the formal parameters of a method

### Lesson 2: Define the scope of a Java class

#### 2.1 Java class files:Contents and naming rules

* Structure is: `Package`, then `Import`, then `Class` (package and imports are optional)

``` java
package mypackage;

import myother.package.Here;
import ohlook.here.are.morepackages.*;

public class MyClass {...}

class MyOtherClass {...}

interface MyInterface {...}
```

* Having more than one class/interface in a source file is unusual, but can be used.
* It's only possible to have at most one `public` class or interface in one source file

#### 2.2 Java classes: The class, member variables, methods and constructors

* Within a class, we're allowed: (in any order, all elements in a class, unlike a method, are collected together, so order does not matter)
  * `Method(s)`, static or non-static
  * `Constructor(s)` (initialize objects of that class before they are made available for regular use)
  * `Class(es)`, yes, you can declare a class inside another class :o
* We can label as `public` so it's available from anywhere in our program, if we leave this off then we can only use this class from within the package it was declared in
* We can mark it as `abstract` which makes it impossible to create an instance of the class
  * It can still contain variables or methods to be referenced elsewhere
  * We usually use abstract classes as a base class for others to be derived

### Lesson 3: Create executable Java applications with a main method; run a Java program from the command line; including console output

#### 3.1 Creating executable Java applications with a main method

* When the JVM starts, it looks for a method: `main` as an entrypoint

``` java
public static void main(String[] args)

// alternatively, the following will function:

public static void main(String $woo[])
public static void main(String _Name[])
public static void main(String... args)
```

* `public` - accessible anywhere
* `static` - no instance needed
* `void` - nothing returned
* `main` - name of method, must be called this
* `String[]` - an array of string
* `args` - purely convention, this can be anything

* There are cases where we do not need to create a main method ourselves, e.g. when bootstrapping with a framework, this framework will often encompass a main method, we just provide plugin material
* Java does not send a status code from the main method
  * We can use `System.exit(status)` if we want to send a status code
* We can overload the main method (same name but different types of arguments), but there is no compelling reason and the JVM will not complain (but won't get treated as the entrypoint)

#### 3.2 Running Java from the command line

#### 3.3 Managing the classpath

* Directory structure must match package names
* If we build from command line with `javac`, we must use `-d` flag
* Supposing we have several classes (`.class` files, library classes are stored in `javalib` from root, application code is stored in respective package structure from root, we need to specify a classpath comprising of our libraries and the location of system libraries
* `java -cp /javalib:/home/example/things.Application` would find code both in `/javalib` and `/home/example/things.Application` (`-classpath` can also be used, or set as an environment variable `$CLASSPATH`), we can also use `/*` notation to include all files in directory
* On Mac/UNIX/Linux, we use colons and foward slashes, whereas on Windows, it uses backslashes and semi-colons

#### 3.4 Working with console output

* We can initialise a reference to the console by using:

``` java
Console c = System.console();

/*
* System.console method might return null.
* It usually does this if the program's I/O has been redirected
*/

if(c != null {
  c.printf();
  c.format();
}
```

* We can use this console object to instantiate a `PrintWriter object`:

``` java
Printwriter pw = c.writer();
pw.printf();
pw.format();
pw.append();
pw.print();
pw.println();
```

* The `Java.lang.system` class has a static member called out, that can be used to write to the system's standard output channel, this is sometimes the same destination as the `Console` object (If I/O redirection occurs, this redirection will be followed)
* `System.out` is actually a `PrintStream` (`PrintWriter` is from `Console`)
* `println()` is unique as there is an overload which doesn't take a parameter, and just prints a new line
* `println` methods will flush the output, that is, force the characters on the screen when they complete. The `print` methods probably won't do this until something more explicit causes them to do so (`flush()` can be called directly)
* `printf` and `format` work the same, taking template strings to concatenate text:

``` java
System.out.printf("The count is %d\n", x); 

// `\n` is a newline, we can also use `%n`
```

* All placeholders start with percent sign (`%d` = integer, `%f` = float, `%s` = string, `%b` = boolean, `%%` = percent sign (%))
* `append` methods take char or CharSequence (e.g. String, String Buffer or String Builder)
* Append returns the same object you started with

### Lesson 4: Import other Java packages and make them accessible in your code

#### 4.1 About packages and their purpose

* A package is a group of related classes
* We can either use the fully qualified name for the classes we wish to use, e.g.

``` java
package mypkg;

Date d = new java.util.Date();
```

* Alternatively we can import as the following

``` java
package mypkg;

import java.util.ArrayList;

Arraylist al = new ArrayList();
```

* JAR Classfiles always use fully qualified class names, so the import statement is just syntactic sugar

#### 4.2 Statement order, wildcard imports, importing sub-packages, and handling duplicate class names

* Heirachies in package names are purely for organisational purposes, and have no special meaning when importing
* We cannot use partial wildcard imports, we can only use an * in place of Classnames
* We should not have ambiguous imports, e.g:

``` java
import java.util.*;
import java.sql.*;

Date d = new Date() // this is ambiguous as both packages contain Date class
```

* We could mark one as explicit, e.g. `java.util.Date d = new java.util.Date()`
* we could explicitly import one using fully qualified name to take precidence

``` java
import java.util.Date;
import java.sql.*;

Date d = new Date() // this is unambiguous as it inherits from java.util.Date class
```

### Lesson 5: Compare and contrast the features and components of Java such as: platform independence, object orientation, encapsulation, etc

#### 5.1 Understanding Java's execution model

* The Java code is compiled to platform independent Java Bytecode to run on the JVM (Java Virtual Machine) which acts as a simulator
* Runtime optimisations can be made, e.g. identifying most frequent code regions (hotspots)
  * These hotspots can be converted from platform independent byte code into native machine code for the host CPU, this area of code will run at full speed of a compiled program

#### 5.2 Understanding the value of threading and garbage collection

* The garbage collector takes the responsibility for releasing memory
* Before Java, if two parts of a program share the same part of memory, where one part of the program uses the memory and passes the reference to another part of the program, seemingly uncoupled, then each sees the data written by the other as an inexplicable corruption of the data that should have been there (these bugs tend to be completely non-deterministic and incredibly difficult to debug)
* Each part of the program is only able to indicate whether it has any ongoing interest in using an area of memory. It does that by keeping a reference to the memory. The garbage collector will look around at intervals, and determine which areas of memory are still of interest to any part of the program. All the areas that are found to be of no interest to any part of the program, are reclaimed and added back to the free memory in the heap. This model completely prevents the accidental reuse of memory. If a part of a program can access memory, then that will not be reclaimed.
* Java does not protect us if we say we're finished with the memory when we're not, and in that case we'll get a `NullPointerException`

#### 5.3 Understanding the value of object orientation and encapsulation

* Classes are are intended to represent well-understood ideas in the problem domain that the program addresses
* Objects are things we build from the templates we call classes, they tightly associate the state that represents something with the behaviour that interacts with that state
* Encapsulation allows the state of the object, its variables, to be visible outside the class only indirectly through methods. And those methods can control whether changes are made or rejected
  * All variables must be private
  * Some methods accessible from outside
  * Methods protect internal state integrity
* Inheritance is a mechanism that lets us create a class from a template, and add extra features, with other changes
  * We cannot take away features, but we can change behaviour
  * We can allow different objects to share definitions of common behaviour
  * We can use association to share behaviours between classes
