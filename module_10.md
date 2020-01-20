## Module 10: Working with selected classes from the Java API

### Lesson 1: Manipulate data using the StringBuilder class and its methods

#### 1.1 Understanding the common StringBuilder constructors		   	  	
* String is a fundamental type built into a compiler, we can easily represent text which can change easily using a `StringBuilder` (`String` is immutable)

* StringBuilder Contructors
	* `public StringBuilder String s)`
	* `public StringBuilder(CharSequence cs)`
	* `public StringBuilder(int i)`
	* `public StringBuilder()`

The StringBuilder is able to automatically reallocate it's array, we should ensure the StrinbBuilder is instantiated with a large enough size of storage array (by passing an int) - start size is `16`

If looking at worst case scenarios, we should pick smaller sizes and let the compiler optimize

#### 1.2 Using methods that modify StringBuilders 

``` java
StringBuilder sb1 = new StringBuilder("Hello");
sb1.append(true);
sb1.append(234);

sb1.delete(7, 11); // First character to delete - First character after block to be deleted

sb1.reverse();

sb1.setCharAt(2, 'A');

sb1.setLength(12); // Can be used to trunkate if shorter, else it will be oadded with `0` characters
```

#### 1.3 Using methods that read and search in StringBuilders, and using methods that interact with the internal storage of StringBuilders

* We can use `substring` to read part of a StringBuilder
* These do not change the text in the string buffer
* They return new objects

``` java
StringBuilder sb = new StringBuilder("Yo");

sb.substring(0, 2); // Reads from 0 to 1
sb.charAt(3); // Returns char at index

sb.getChars(3, 7, dest, 4); // First 2 is region to extract, 3rd is char array for the characters to be placed, the fourth is an offset into that array. 

sb.indexOf('yo'); // Returns index of start of parameter passed in
sb.indexOf('yo', 7); // Same, but skips over leading characters
sb.lastIndexOf('woo'); // Starts from back

// These all return `-1` if value cannot be found

sb.length(); // Length of array
sb.ensureCapacity(int); // Grows the capacity to specified size (using multiple hops)
sb.trimToSize(); // Ensures no memory wasted, will reallocate buffer
```

### Lesson 2: Creating and manipulating Strings

#### 2.1 Creating Strings
