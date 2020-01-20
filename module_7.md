## Module 7: Working with Methods and Encapsulation

### Lesson 1: Create methods with arguments and return values; including overloaded methods

#### 1.1 Creating methods

* The declaration has optional modifiers, e.g. `private`, `public`, `static`
* Followed by this, we have modifiers, the return type and the name, followed by an argument list

``` java
<modifiers> return-type name (<argument list>)
```

* Modifiers can include 
  * annotions, e.g. `@override` (beyond exam scope)
  * access control modifiers: `public`, `private`, `protected`
  * `abstract`
  * `static`
  * `final` (cannot be overridden)
  * `synchronized` (to do with threading, irrelevant)
  * `native` implementation will not be written in Java, also irrelevent
  * `strictfp` to do with floating points, do not need to know

* Return types
  * any primitive or Object
  * can only define one return for each method

* Name
  * any legal identifier
  * must not be the same name as any other method taking the same arguments   

* Arguments
  * 0 or more arguments are taken
  * `<type> <name>`
  * The names cannot conflict with each other, or other variables in method 

The method returns to it's caller either when `return` is stated in the method, or at the end curly brace. If a method is void, we can just use `return;`, but this is optional

#### 1.2 Code example for simple methods

``` java
public void simpleMethod() {
  if (Math.random() > 0.5) { 
    System.out.println("Large number");
    return;
  } else {
    System.out.println("Small number");
  }
  System.println("The end");
}
```

``` java
public String stringMethod(String initial) {
  return initial + "is now better";
}
```

#### 1.3 Understanding basic syntax of overloaded methods

These are methods defined in one class, which have the same basic name

* If we want to overload a method, we cannot just change the argument names, we must adjust the number of argument types, or the type of argument that's being passed in

``` java
class Thing {
  
  void doStuff() {...}
  void doStuff(String s) {...}
  void doStuff(float f, Date d) {...}
}
```

#### 1.4 Understanding rules and guidance for overloaded methods

* Overloading is based on coded form of list of argument types, not the return value
* It's bad practice to overload and change the return type as well

Example: Why do we have overloaded methods?

* We may want to do the same thing with different arguments

``` java
Cake makeCake(Eggs e, Flour f) {...}
Cake makeCake(PacketMix pm) {...}
```

#### 1.5 Code example for overloaded methods

``` java
class Thing {
  
  void doStuff() {...}
  void doStuff(String s) {...}
  int doStuff(int f, Date d) {...}
}
```

#### 1.6 Investigating variable length argument lists

We might sometimes want to define a method that can handle any number of something, not just a fixed number. In the JVM, we must take a predetermined number of arguments (of predetermined type). 

* The compiler wraps the arguments into an array for us 

``` java
public int addUpNumbers(int ... values) {...}
```

* We can pass either a list of values or an array 

``` java
addUpNumbers(1, 2, 3)
addUpNumbers(new int[]{1,2,3})
addUpNumbers()
```

The vararg form will take 0 or more arguments

* If we want to enforce at least one item: `addUpNumbers(int first, int ... rest) {...}`

### Lesson 2: Apply the static keyword to methods and fields

#### 2.1 Comparing class fields and object fields

``` java
Class Thing {
  private static int x;
  private int y;
}
```

* Each instance of `Object` of type `Thing` has a reference to type `Class` that it belongs to 

``` java
Thing t = new Thing();

t.x; // Reference to x variable in Class Object
t.y; // Reference to y variable in Thing object

Thing t1 = new Thing();

t1.x; // Same reference as x t.x above
t1.y; // New reference to y variable in Thing object
```

All objects in a class share the static fields of a class, but every object gets its own version of a non-static field.

#### 2.2 Using static on methods

``` java
Class Thing {
  
  private static int x;
  private int y;
  
  doStuff() {...}
  static doStaticStuff() {...}
}
```

* We usually invoke static methods with the classname, but it's legal to invoke with instance name
  * It ignores the instance it's invoked from 

``` java
Thing t = new Thing();

t.doStuff();

Thing.doStaticStuff();
t.doStaticStuff();
```

* We are not allowed to use `this` in static methods

Static should be used when we are dealing with properties or aspects of the thing we're modelling that relate to the concept as a whole, rather than individual things. 

#### 2.3 Code Example

``` java
public class M7L2 {
  private int x;
  private static int y;
  
  public static void main(String[] args) {
    y = 99;
    System.out.println(y); // 99
    
    M7L2 object1 = new M7L2();
    M7L2 object2 = new M7L2();
    
    System.out.println(object1.y); // 99
    System.out.println(object2.y); // 99
    
    object1.x = 100;
    object2.x = 200;
    
    System.out.println(object1.x); // 100
    System.out.println(object2.x); // 200
    
    showY();
    
    showX(); // Rejected, as we can not call this from a non static context
    
    object1.showX(); // X = 100, Y = 99
    object2.showX(); // X = 200, Y = 99
    
  }
  
  static void showY() {
    System.out.println(y); // 99

    System.out.println(M7L2.y); // 99
    
    System.out.println(this.y); // Rejected, as there is no this in static methods
    
    System.out.println(x); // Rejected, as x is not a static variable
    
    System.out.println(this.x); // Rejected, as there is no this in static methods
  }
  
  void showX() {
    System.out.println(x);
    
    System.out.println(y);
}
```

### Lesson 3: Create and overload constructors; including impact on default constructors

#### 3.1 Creating and overloading constructors

What is a constructor?

``` java
class Thing {

  Thing() {...}
```

* Capitalization and spelling must be exact match  
* No return type (not even `void`)
* We can pass normal argument lists
* We can provide accessibility specification, e.g. `private`, `public`...
* We can provide a body if desired

To invoke the structure, we just use `new Thing(<argument list>);`

* When we call `new`, the code does not jump straight into the body, there are a few things first
  * An object is created (memory allocated)
  * If there is a parent class, the constructor for that will be called, this bubbles all the way up to the parent class: `java.lang.Object`
  * Instance initialisation is performed  
  * The constructor called runs

We are allowed to overload based on normal overloading rules

#### 3.2 Differentiating between default and user defined constructors

``` java
class Thing {
  public Thing() {
    super();
  }
}
```  

If we do not provide our own constructor, the compiler will create one for us, the `default constructor`

* Takes no arguments
* Body just calls superclass constructor, which takes no arguments
* If the superclass does not have this constructor, it errors
* This constructor accessibility is the same as the class
  * One exception: an `enum` always have `private` constructor (not on exam)

### Lesson 4: Apply access modifiers

#### 4.1 Using the access modifiers public and private

* `public` - accessible anywhere in program
* `private` - visible inside the top level enclosing curly braces that surround its declaration
  * Aceessible to other classes, where we have class nesting 

#### 4.2 Using default access and the protected modifier

 * `default` - visible inside package
   * we do not need to define, automatically default
 * `protected` - visible inside package _and_ and subclasses

### Lesson 5: Apply encapsulation principles to a class

#### 5.1 Designing for encapsulation

* Ensure all member variables of a class are `private`
* Best practice: allow modification of public variables, throwing Error if it's not updated correctly
* We use modifiers to seperate concerns between internal interactions and external APIs

#### 5.2 Implementing encapsulation

Encapsulation helps us to know ehre to go in order to debug. It should also help show to intended users to these classes that not part of the general API for use by everyone. 

* We can make immutable objects final, just for clarity

``` java
public class Thing {
  private int x;
  public static final int y = 2;
  public static final String s = "Hello";
  private static int z;
  
  private void modifyUtility() {
    ...;
  }
  
  void doStuff(AnyType a) {
    // validate here
    // throw error if invalud
  }
}
```

### Lesson 6: Determine the effect upon object references and primitive values when they are passed into methods that change the values

#### 6.1 Changing values through method local variables

``` java
int x = 99; // 99

StringBuilder sb = new StringBuilder("Hello");
addOne(x); // 99
System.out.println(x); // 99
addOne(sb); 
System.out.println(sb); // "Hello world"
```

``` java
void addOne(int x) { // x is copied to this variable, so the local variable ceases to exist
  x++; // 100
  System.out.println(x);
}

void addOne(StringBuilder sb) {
  sb.append(" world";
  System.out.println(sb);
}
```

* Primitive values contain the values they represent
* Non primitive (reference) values represent a reference to an object
  * We do not change the reference to the object, rather the object being referenced

#### 6.2 Changing the value of method local variables

* All method arguments are passed by value
* All method local variables are copied
  * Primitives have a completely independent copy of the value
  * Changing a local variable with primitive type cannot change the original
  * Objects pass the pointer to object
  * If we change this object referred to, it's visible through all references
* If we change the value in a method local variable to point to a different object, we do not see the change in the caller

#### 6.3 Code example

``` java
package m7l8;

public class M7l8 {

  public static void add(int x) {
    x++;
    System.out.println(x);
  }
  
  public static void add(StringBuilder sb) {
    sb.append(" world");
    System.out.println(sb);
  }
  
  public static void change(StringBuilder sb) {
    sb = new StringBuilder("Auf Wiedersehen!");
    System.out.println(sb);
  }
  
  public static void main(String[] args) {
    int x = 99;
    add(x);
    System.out.println(x); // 99
    
    Stringbuilder sb = new StringBuilder("Hello");
    add(sb);
    System.out.println(sb); // "Hello world"
    
    sb = new StringBuilder("Hello");
    change(sb); 
    System.out.println(sb); // "Hello"
  }
```
