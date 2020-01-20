## Module 5: Creating and Using Arrays

### Lesson 1: Declare, instantiate and use a one-dimensional array

#### 1.1 Understanding simple array declarations, and variables of array type

* Array declarations follow the basic variable initialisation structure:

``` java
<typename> <variable name>;
<typename> <variable name> = <initialisation>;
```

* So to declare an array, we use the following:

``` java
int arrayOfInt[]; // Refers to array of integers
int[] arrayOfInt; // Refers to array of integers
int arrayOfInt[...]; // Refers to single int
```

#### 1.2 Instantiating an array, array length

* `int[] aI;` -> Array of integers, but we still need to instantiate it with the assignment `aI  = new int[10];`
  * This creates an array of length 10
  * As this is an array of int, each value is defaulted to `0`
  * Max size is about 8GB (20,000,00,000 int objects)
* The length of array is determined by the number of elements in array (int expression)

#### 1.3 Initlaizing arrays by iteration, array indexes

``` java
int[] ai = new int[10];
```

* These items are indexed from aI[0] -> aI[9]
* Length of array is fixed at creation time and can *never be changed*

We can iterate through an array assigning values the following way:

``` java
int[] aI = new int[10];
for(int i=0; i<aI.length; i++) {
  aI[i] = getValue();
}
```

Arrays can also be used for Objects, e.g. `StringBuilder`

* Each element is a reference variable to an Object, meaning each has a default value (null for Objects)

``` java
StringBuilder[] sba = new StringBuilder[10]
for(int i=0; i<sba.length; i++) {
  sba[i] = new StringBuilder("Hello" + i);
}
```

#### 1.4 Using a combined declaration and initialization of arrays

We can create an arrays and add values to it using the following:

``` java
int[] aI = {1, 2, 3, 4};

// Java doesn't mind about trailing commas
int[] aI = {1, 2, 3, 4,};

Thing[] ta = {
  new Thing(),
  new Thing(99),
  existingThing
};
```

#### 1.5 Using immediate array creation not in a declaration

If we have a method:

``` java
public void doStuff(Thing[] ta) { ... }
```

We might like to provide an array to `doStuff()` without initialising a variable first

``` java
doStuff(new Thing[] { newThing(), newThing(99 });
```

* This makes it easy to tell the compiler what the base type should be
* We are allowed `literals`, `variables`, `expressions`, `object creation` (expression of the type object it creates) and `null`

#### 1.6 Initializing arrays by copying

* Remember, *arrays are fixed sizes*
* If we want a larger array, we can copy our array into another, larger, array
* We can use `System.arraycopy(<source array>, <starting index to copy>, <destination array>, <offset into destination array>, <num of items to copy>)`

``` java
int[] aI = {1,2,3,4,5};
int[] aI2 = new int[10];

System.arraycopy(aI, 0, aI2, 0, aI.length);
```

We can now point the original `aI` to the new array: `aI2` with `aI = aI2;`

### Lesson 2: Declare, instantiate, initialize and use multi-dimensional array

#### 2.1 Declaring multi-dimensional arrays

* These are constructed with `<typename> <variablename>;`

``` java
int[][] iaa;

where int[] iaa[x] refers to a single array inside iaa

where int iaa[x][y] refers to a single int
```

* This can be instantiated with `int iaa[][];`

#### 2.2 Using immediate initialization of multi-dimensional arrays

* We can initialise in the standard curly brace method as follows:
	* This creates 4 arrays, so 4 pointers (one for each inner array and a pointer for the outer)
		* 3x Array of Int, 1x Array of Array (of ints) 

``` java
int[][] iaa = {. // iaa.length -> 3
  {1,2,3},
  {2,4,6},
  {7,8,9}
};
```

* We can also just have basic array of arrays:

``` java
int[][] iaa = { // iaa.length -> 3
  {1}, // iaa[0].length -> 1
  {2,3}, // iaa[1].length -> 2
  {4,5,6} // iaa[2].length -> 3
};

// e.g. iaa[0][2].length -> 3
```

#### 2.3 Using iterative initialization of multi-dimensional arrays

``` java
int[] ia = new int[10];

// This creates one array, length 10

int[][] iaa = new int[2][3];

/*
 * This creates 3 arrays:
 * 2 Sub arrays of length 3
 * 1 Holder array of length 2
*/ 
```

We are able to iterate through these arrays:

``` java
for(int i=0; i<iaa.length; i++) {
  for(int j=0; j<iaa[i].length; i++) {
    iaa[i][j] = getValue();
  }
} 
```

And we're able to have arrays of different sizes, for example describe the size of arrays of arrays, but not define the size of internal arrays:

``` java
int[][] iaa = new int[4][];

/*
 * We have created an array of 4 pointers
 * These all contain null values
*/ 

for(int i=0; i<iaa.length; i++) {
  iaa[i] = new int[i+1];
}
```

This loop code will make an array as follows:

``` java
{ {0}, {0,0}, {0,0,0}, {0,0,0,0} }
```

* We are allowed to create `int[4][]`, but not `int[][4]`
* If we are performing a partial initialisation, we must add numbers to the brackets from left to right
* Arrays can be as deep as we want, e.g. `int[][][][] iaaaa;`

#### 2.4 Code examples for multi-dimensional arrays

The following forms are legal:

* `Thing[][] taa;` -> Pointer to Array of arrays
* `Thing[] taa[x];` -> Pointer to array of thing
* `Thing taa[x][y];` -> Pointer to individual thing
