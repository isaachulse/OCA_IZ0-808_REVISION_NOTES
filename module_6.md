## Module 6: Using Loop Constructs

### Lesson 1: Create and use while loops

#### 1.1 Creating and using while loops

The basic while loop:

``` java
while(<boolean expression>) {
  <statement>
}	
```

* The boolean statement can be a `Boolean` type (object), it will be unboxed at runtime
* Test is made before entry, so loop body may never be executed

#### 1.2 Code examples of the while loop

We may accidentally make an infinite loop:

``` java
int x = 3;
while ( x != 0) {
  System.out.println("x is" + x);
}	
```

### Lesson 2: Create and use for loops including the enhanced for loop

#### 2.1 Understanding the simple use of the for loop

``` for
for(<setup>; <condition>; <prepare for next time>) {
  <programmer's code>
}
```

#### 2.2 Understanding the initialization section of the for loop

We can initialize many variables at a time:

``` java

for(int x=0, y=100, z; <test>; <next time>) {
}
```

All variables initialized in the initialization section have to be the same type, but we can use arrays:

 * We can only set variables of the same base type, other types are not allowed, as we are not allowed to use semi colons again in this section
 * We are allowed to **either** declare variables **or** perform an expression in this section, but **not** both

``` java
for(
  int x=0, y=100, z, a[] = {1,2,3}, b[][], c = new int[5]; 
  <test>;
  <ready for next>
  ){
  <code here
}  
```

#### 2.3 Understanding the test section of the for loop

* `Booleans` will be unboxed to get the `boolean` primitive
* This can result to false on first run, and never enter the loop

``` java
for(...; <test>; ...) {
  ...;
}
```

``` java
while(<test>) {
  ...;
}
```

#### 2.4 Understanding the increment section of the for loop

* The initialization section permits either declarations or expression statements
  * Expression statements are increment and decrement operations, assignments, invocations of methods, and construction calls using the keyword, `new`
  * We can provide a single expression or a comma seperated list of expressions

``` java
for(...; ...; x++, y = getValue(), doStuff(), new Banana()) {
  ...;
}  
```

#### 2.5 Omitting sectinos of a for loop

* Any section can be left blank, by changing the section for a semi colon `;`
* Leaving a test out makes it always true

* The following equals `while(true)`

``` java
for(;;) {
  ...;
}
```

* We can make a while loop from a for loop

``` java
for(; <test>;){
  ...;
}
```

#### 2.6 Code examples for basic for loops

``` java
int t;
int a = 0;
int b = 0;
int a = 0;

for(t = (int)(Math.random() ** 10.0); a < 5; a++, b += 10, sayHello()) {
  System.out.println("tick...");
}
```

#### 2.7 Understanding the simple use of the enhanced for loop

We use enhanced loop to iterate through collections of objects

``` java
ArrayList al = ...;

for(Object obj: al) {
  System.out.println(
    "Object is " + obj);
  al.add(new Date()); // We should not do this line
}
```   

* We should be careful to not change the collection while iterating through it, unless explicitly ok to do so


#### 2.8 Identifying the valid targets of the enhanced for loop

* We can use an enhanced for loop with anything that implements the interface `Iterable`, e.g. `Iterator iterator()` 
  * This interface provides 2 methods:
    * `hasNext()` tells us if the list has a next item (boolean)
    * `next()` gives us the next item
* We can't iterate through an `Iterator` object, only something that implements the `Iterator` interface    

#### 2.9 Using the enhanced for loop with generic collections

* The compiler knows what the object being used in the for loop's type is when we explicitly declare it

* If we have the following, the compiler knows the `obj` is a `String`
  * We will get a `ClassCastException` if we don't implement generics mechanism correctly 

``` java
ArrayList<String> als = ...;
for(String obj: als) {
  ...;
}
```

#### 2.10 Code examples for enhanced for loops

``` java
ArrayList<String> als = new ArrayList<>();
als.add("Hello");
als.add("Wilkommen");
als.add("Bienvenue");

for (String s: als) {
  System.out.println("String is " + s);
}
```

### Lesson 3: Create and use do/while loops

#### 3.1 Creating and using do/while loops

* The loop is controlled by a simple test condition, an expression that evaluates to a boolean result
* The test occurs after the body of the loop, rather than the top
* We can guarantee this is executed at least once

``` java
do {
  ...;
} while(<test>);
```

### Lesson 4: Compare loop constructs

#### 4.1 Comparing while and do while loops

* In a while loop, the code in the body is not guaranteed to run, a do loop ensures it's run at least once
* The while loop makes the test at the top, while the do while loop tests at the end

#### 4.2 Comparing while and simple for loops

* The for loop is essentially a while loop with some special extras
* In both the for loop and the while loop, there is a boolean test that controls the loop
* The loop will execute for as long as that test continues to evaluate to true
* The test is at the top of the body of the loop
* It's possible for the loop to execute zero times

The major difference is that the for loop adds the initialization section which is typically used for declaring variables but can be used for other kinds of setup, and the prepare for next time around section, which is effectively executed after the body of the loop before execution jumps back to the top to re-perform the test.

#### 4.3 Comparing while and enhanced for loops working on Iterables

* An enhanced for loop is just a more *intelligent* while loop, with boilerplate code behind the scenes

``` java
List<Thing> lt = ...;
for(Thing t: lt) { ... }

// Is equivalent to

Iterator it = lt.iterator()
while(it.hasNext()) {
  Thing t = it.next();
  ...;
}
```

#### 4.4 Comparing while and enhanced for loops working on arrays

* The enhanced for loop does not let us see the current index when inside the loop (or outside)

``` java
Thing[] ta = ...;
for(Thing t: ta) { ... }

// Is equivalent to

int idx = 0;
while(idx < ta.length) {
  Thing t = ta[idx++];
}
```  

### Lesson 5: Use break and continue

#### 5.1 Using break from a single loop

* The `break` statement lets us escape early from a loop
* We exit the loop without attention to the test 

``` java
while(test()){
  if(true) {
    break;
  }
}
```

#### 5.2 Using continue in a single loop

* The `continue` keyword jumps back up to the test in a while loop, or the third segment in a for loop (increment section)
* We still loop around, but abandon the current iteration

``` java
while(test()) {
  if(aaa()) {
    continue;
  }
}
```

#### 5.3 Using a labeled break from multiple loops

* We can use a label on any statement
* The name of any method or variable is an identifer, so this is similar
* Must begin with unicode of `_` or `$`

``` java
<identifier>:<statement>
```

This can be useful when trying to `break` inside multiple while loops:

``` java
while(<test>){
  ...;
  middle:while(<other test>){
    ...;
    break middle;
  }
}
```    

#### 5.4 Using a labeled continue from multiple loops

We use a labeled `continue` in the same way as `break`

* The label can be applied to a section of code by using indentation

``` java
while(<test>){
  ...;
  middle:
    while(<other test>){
      ...;
      continue middle;
    }
}
```    
