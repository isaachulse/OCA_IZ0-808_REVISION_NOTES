## Module 8: Working with Inheritance

### Lesson 1: Describe inheritance and its benefits

#### 1.1 Understanding interface and implementation inheritance

A key concept of Object-Orientated programming is `generalization`, allowing us to specify a base implementation, and allowing us to build on top of this with `specialization`. This refers to the concept that `specialization` is just `reverse-generalization`.

We can call this `specialization` two things:

* Interface Inheritance
  * Making or fullfilling a promise 
  * Contract to describe certain capabilities - subclass provides everything the parent has, and can be more
  * The parent class does not have to describe how each _capability_ is coded
* Implementation inheritance
  * We specify code about how methods work in the parent class
  * Anything we can do in parent, we can do in subclass
  * We can modify behaviour provided by parent

#### 1.2 Basic coding of implementation inheritance

``` java
class Thing {
  ... stuff ...
}

class SpecialThing extends Thing {
  ... stuff .. // from 'Thing'
  
  ... extended stuff of our own ...
```

#### 1.3 Changing inherited behaviour

We can annotate with `@override`

* To change behaviour, we need a class with the same name and an identical argument list
* We need to make sure to override, rather than overload

``` java
class Thing {
  
  public void sayHello() {
    System.out.println("Hello");
  }
}

class SpecialThing extends Thing {

  public void sayHello() {
    System.out.println("Bonjour");
  }
}
```

* The method cannot be _less_ accessible
* It must be available in all the places the original method _would_ have been accessible

* If we have a `primitive` return type, the override method must return the same type
* If we have an `Object` return type, we can return an object which is a subclass of the type originally defined

#### 1.4 Code examples

``` java
package m8l1b;

public class Person {

  private String name;
  
  public String getName() {
    return name;
  }
  
  public void greet() {
    System.out.println("Hello " + name);
  }
  
  public Person(String name) {
    this.name = name;
  }
}

public class Friend extends Person {

  private String relationship;
  
  public Friend(String name, String relationship) {
    super(name);
    this.relationship = relationship;
  }
  
  public void haveRelationship() {
    System.out.println("Relationship with " + name);
  }
  
  @Override
  public void greet() {
    System.out.println("Hello " + name + " my " + relationship);
  }
}
```

#### 1.5 Philosophy and terminology of inheritance (part 1)

Encapsulation means the data members of an object should be manipulated only by methods defined in that object. We do this by ensuring member variables are marked private. Data integrity should be codified. 

A well designed object should have carefully thought out paradigms, such as constructors and static values to ensure it can be created correctly. 

* Specialization is the opposite of Generalization
* Liskov Substitution is the idea that the specialized form must be a perfect substitute for a generalized form

Interface inheritance differs from implementation inheritance in that no implementation gets reused, the class implementing an interface does not gain member fields or methods as default. 

* Interfaces promise behaviours
* We are not allowed multiple implemetation inheritances, for sake of clarity

#### 1.6 Philosophy and terminology of inheritance (part 2)

While overriding can change semantics of a method, it's not possible to remove definition of a method. 

* Return type must be the same
* The method must not be less accessible
* The method may not throw checked exceptions that would have been illegal from the base class

Polymorphism is the ability to look at something in multiple ways. It describes the ability to look as the specialized form of something as the generalized thing. 

Sometimes when we create private member variables, we can use `getters` and `setter` to mutate these variables. 

``` java
class Thing {
  private String name;
  
  public String getName() {
    return name;
  }
  
  public void setName(String n) {
    name = n;
  }
}
```

### Lesson 2: develop code that demonstrates the use of polymorphism

#### 2.1 Understanding the concepts of polymorphism

``` java
class Animal
class Dog extends Animal

Animal a = new Dog();
Animal[] zoo = new Animal[100];

zoo[0] = new Tiger();
zoo[1] = new Lion();

public void feed(Animal a) ...

Dog d = new Dog();
feed(d);

for (Animal a: zoo) {
  feed(a);
}
```

#### 2.2 Code example

``` java
package m8l2;

public class Animal {
  public String getName() {
    return "Unknown";
  }
  
  public String eats() {
    return "Unknown";
  }
  
  public void eat(String food) {
    System.out.println("Nom");
  }
}

public class Lion extends Animal {
  private static final String FAV_FOOD = "Vegan";
  
  @Override
  public String getName() {
    return "Larry";
  }
  
  @Override
  public String eats() {
    return FAV_FOOD;
  }
}

public class Dog extends Animal {

@Override
  public String getName() {
    return "Woofles";
  }
  
  @Override
  public void eat(String food) {
    System.out.println("Nom Nom Nom Woof");
  }
}

public class ZooKeeper {
  public void feedAnimals(Animal [] animals) {
    for (Animal a : animals) {
      String name = a.getName();
      String food = a.eats();
      System.out.println(name + " likes " + food);
    }
  }
}
```

#### 2.3 Understanding the core teminology of polymorphism

A class can only have a single parent class, but that parent can have parents

* Parent classes
  * Base class
  * Super class
  * Generalization
  * Interface
  * Contract

* Child class
  * Derived class
  * Sub class
  * Specialization

We can either use `extends` for implementation inheritance, or `implements` for interface inheritance

#### 2.4 Understanding variable type and object type

The type of the object may be the same type as the variable, or the type of the object may be a  specialization of the type of the object (subclass or implementation of an interface). 

* If you invoke behavior on a variable, the compiler will use the type of the variable, or it might be an expression actually, to determine whether or not that method is possible for the things that variable might refer to

#### 2.5 Determining object type

We can use two methods to find out class of object at runtime:

* `.getClass()` method
* `instanceof` type

#### 2.6 Code examples

``` java
private static void showAnimalType(Animal a) {
  Class theClass = a.getClass();
  String className = theClass.getName();
  Class parentClass = theClass.getSuperClass();
```

### Lesson 3: Determine when casting is necessary

#### 3.1 Understanding the Liskov substitution principle and the "is a" relationship

``` java
SomeType t = aThing;
```

* The thing your assigning must substitute seamlessly into the thing you're assigning to
* If type of aThing is SomeType, we do not need to cast
  * If `SpecialSubtype` extends `SomeType`, we can say `SpecialSomeType` implements `SomeType`


``` java
Animal a = theDog;  // OK!

Dog d = a;  // ?

/*
 * This is not true if Animal a = theCat;
 * We use:
 * Dog d = (Dog) a; 
 *
 * This will compile OK, but throw a ClassCastException at runtime
*/
``` 	

#### 3.2 Recognizing impossible assignments

There are some circumstances where assignments are not possible, e.g:

``` java
Animal a = theCactus; // Animal and cactus have no relation to each other
Animal b = (Dog) theCat; // Sometimes allowed at runtime
```

* The assignment may be possible if the reverse assignment could have also been possible

#### 3.3 Understanding casting with interface types in assignments

If we have:
* `Animal` (class), with `Tiger` and `Puppy`
* `Pet` interface, with `Puppy` and `Rock`

``` java
Animal a = (Animal) thePet;
```

If `thePet` was `Puppy`, this assignment is ok, but if it is `Rock`, then this is cannot be done, and will throw a Runtime exception. 

``` java
Tiger t = (Tiger) thePet;
```

We do not need to cast this, but the assignment works anyway.

* Where interfaces are concerned, providing we use a cast, the compiler will almost always permit the assignment.
* When `final` is used, we cannot create any subclasses of this class, this will reject any assignments that would have been permitted without the `final` decleration.

### Lesson 4: Use super and this to access objects and constructors

#### 4.1 Understanding "this" for accessing object features

* When we declare a variable and use it to refer to an object, this is a pointer to an object. We can invoke a method by calling this reference. 
* Under these circumstances, code invoked by the instance of the object can be called with `this`

The keyword `this`, which provides an explicit reference to the current context object.

#### 4.2 Understanding "super" for accessing parent features

* Using `super` makes the code instantiation look inside the parent class for the scope

``` java
class Base {
  int x;
  
  String getMessage() {
    return "Hello";
  }
}

class Derived extends Base {
  int x;
  
  public void method(int x) {
    super.x = 100; // Base x
  }
  
  String getMessage() {
    return "Hello" + "everyone!";
  }
  
  // Instead of the above we can use:
  
  String getMessage() {
    return super.getMessage() + "everyone!";
  }
}
```

#### 4.3 Understanding "this()" for accessing overloaded constructors

``` java
class Thing {
  private int x;
  
  public Thing() {
    x = 99;
  }
  
  public Thing(int x) {
    this.x = x;
  }
}
```

Intead of the above, we should use:

``` java
class Thing {
  private int x;
  
  public Thing() {
    this(99)
  }
  
  public Thing(int x) {
    this.x = x;
  }
}
```

_Transfer control to another method for this class that takes a matching argument_
* Instantiates method, calling default/overloaded constructors

#### 4.4 Understanding "super()" for accessing parent constructors

* A constructor is a special method to set up an object immediatlely after using `new` to instantiate an object

``` java
class Base {
  int x;
  Base(int x) {
    this.x = x;
  }
}

class Sub extends Base {
  int y;
  Sub(int x, int y) {
    super(x);
    this.y = y;
  }
  
  Sub(int y) {
    this(y, 100);
  }
}
```

* If we use `super()` to pass control to a parent class constructor, it must be the first thing that happens in the constructor
* We cannot construct a subclass without initialising the base class
	* It always assumes `super();` (no arguments)

#### 4.5 Understanding the underlying principles of "this" and "super" for invoking other constructors

* Each subclass if built omn top of the parent class (with `java.lang.Object` being the foundation)
* It implicitly runs `super()` going foundations-upwards - if no constuctor matching this signature is provided, we will receive a compiler error
* We must call `this` or `super` first, we can choose one or the other
	* `super()` always takes priority

#### 4.6 Code examples

``` java
Sub(int x, int y) {
  super(this.calcValue(y)); // This is not allowed, as we need to initialise base class first
}

Sub(int x, int y) {
  super(Sub.calcValue(y)); // This is allowed, as calcValue is static
}
``` 

### Lesson 5: Use abstract classes and interfaces

#### 5.1 Preventing instantiation

Once we have declared a class as being abstract, we will not be permitted to make an invocation of `new` on that class directly.

* `abstract` lets us define constructors for classes which are accessible but prevent those constructors from being called in any way other than by subclasses (by using `super()`)

#### 5.2 Marking behaviours abstract

``` java
abstract class Animal {
  abstract String likesToEat();
}
```

* Concrete classes must define implementation for this method `likesToEat`
* All subclasses must either be abstract or define constructors for all abstract methods

#### 5.3 Understanding the rules about abstract classes and methods

* We can define constructors in abstract classes, and although they will not be called by `new`, they can be called by `super` in subclasses
* An abstract method can never be defined as private (it needs to be visible to the subclass)
* We cannot label an abstraxct method as `final`, as we want it to be overloaded in subclasses to fulfill the promise of the contract
* Abstract class needs not have abstract methods (can have concrete methods)
* If we have a concrete class, we cannot declare abstract methods
* If we have an abstract class but cannot implement all abstract methods in parent class, this must be marked as abstract

#### 5.4 Understanding and defining interfaces

We can have an interface with zero methods in

``` java
public interface Photographer {
  Image takePhoto();
  
  int x = 99; // This does not create a variable
  int x; // Illegal, we need to define value
  
}
```

* All methods in an interface are `public abstract` (whether we say so or not)
* All fields are `public static final` (initialisation must be immediate)

#### 5.5 Implementing and using interfaces

* We can implement more than one interface for a class, by providing a comma seperated list
* The extends clause preceeds the implements clause
* Interfaces provide the ultimate in design for minimum knowledge

#### 5.6 Code example for interfaces

``` java
class Flyer {
  takeOff() 
}

class Photographer {
  takePhoto()
}

class SpySatellite extends Asset implements Flyer, Photographer {
  public void takeOff(){...}
  
  public Image takePhoto(){...}
}
```

#### 5.7 Understanding the rules about interfaces

* All methods in interfaces are `public` and `abstract`, but never `final`
* Fields in interfaces are `public`, `static` and `final`
* Constants in java should be `ALL_UPPER_CASE`

We are allowed interfaces which are specialisations of other interfaces by using `extends`

#### 5.8 Understanding static and default methods

* A default method is an inheritable overridable instance method defined in an interface, letting us have multiple implementation inheritance
* Any method defined in an interface creates an obligation for every class that implements that interface (that class must provide an implementation of the method)
* `default` should only be used as a fallback
