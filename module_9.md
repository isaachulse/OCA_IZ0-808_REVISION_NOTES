## Module 9: Handling Exceptions

### Lesson 1: DIfferentiate among checked exceptions, RuntimeExceptions and Errors

#### 1.1 Understanding exception types

* Checked Exceptions -> Problems with the environment, e.g. user input
	* We should write code to attempt to fix it 
* Unchecked Exceptions -> Should not happen
	* RuntimeException
* Errors (part of Unchecked) -> Problems external to the program
	* OOM (catastrophic)  

``` 
Throwable -> Error
Throwable -> Exception -> RuntimeException -> IndexOutOfBoundsException
```

### Lesson 2: Create a try-catch block and determine how exceptions alter normal program flow

#### 2.1 Coding try and catch

``` java
try {
  ...
  ...
} catch(IOException ioe) {
  ...
} catch(OtherException oe) {
  ...
}
```
 
#### 2.2 Passing an exception to our caller

We must admit to caller that checked exceptions can happen

``` java
public void authorizeCC(Card c, int amt) throws CCFailedException, OtherException {
  try {
    ...
  } catch( ____ ) {
    throw new CCFailedException();
  }
}
```

#### 2.3 Using finally to clean up resources

Finally block is always executed:

``` java
Socket s;

try {
  ...
} catch( ___ ) {
} finally {
  if (s != null) s.close();
}
```

* We might not complete execution of finally block if we encounter an unchecked exception
 
#### 2.4 Using the try with resources mechanism

``` java
try (
  Socket s = x.connect();
  InputStream is = y.open();) {
    ...
  } catch ( ___ ) {
    ...
  }
}
```

* All resources declared and initialised within the try block will be closed
* The system does all checking to see if variables are null or not
* We can still add a finally block of our own
* We must declare and initialise variables in this block (as many as we want), seperated by semicolons (last semicolon is optional)  

We are allowed the following normally:

* try + catch 
* try + finally
* try + catch + finally

And with `try with resources`:

* try
* try + catch 
* try + finally
* try + catch + finally

#### 2.5 Code example for try / catch / finally 

``` java
try {
  System.out.println("In try");
  if (Math.random() > 0.5) {
    throw new FileNotFoundException();
  }
} catch (FileNotFoundException fnfe) {
  System.out.println("In catch");
}
```

#### 2.6 Code example for try with resources

``` java
try (MyResource r = new MyResource();) {
  System.out.println("In try");
} catch (IOException ioe) {
  System.println("Close failure?");
} finally {
  System.out.println("Explicit finally");
}
```

### Lesson 3: Describe the advantages of Exception handling

#### 3.1 Investigating the philosophy of the exception mechanism

* Checked
	* Can and should repair in code
* Unchecked
	* Usually catastrophic
* RuntimeException
	* We should debug these

#### 4.1 Handling exceptions throw by called code

* To handle an unchecked exception, we should wrap the code in a try block catching either the specific exception or a parent class of that type   
* We should declare when exceptions are going to be throw in a method
 
#### 4.2 Code example

``` java
public static void broken() throws AskForNewFilenameException {
  System.out.println("Starting");
  boolean success = true;
  int retriesLeft = 2;
  while (!success) {
    try {
      System.out.println("In try");
      if (Math.random() > 0.5) {
        System.out.println("Failed");
        throw new FileNotFoundException();
      }
      success = true;
      System.out.println("Success");
    } catch (FileNotFoundException) {
      System.out.println("In catch");
      if (0 == retriesLeft--) {
        System.out.println("Too many retries");
        throw new AskForNewFilenameException();
      }
    } finally {
      System.out.println("In finally");
    }
  }
  ```
  
### Lesson 5: Recognize common exception classes (such as NullPointerException, ArithmeticException, ArrayIndexOutOfBoundsException, ClassCastException)

#### 5.1 Common Exception Classes

* AssertionError
	* Thrown by `assert` keyword (disabled by default for performance reasons)
* OutOfMemoryError
	* Run out of memory
* StackOverflowError
	* Result of runaway recursion 
* IOException
	* General input/output failures 
* SOAPException
	* SOAP based web services call
* SQLException
	* Problems with databases
* URISyntaxException
	* Using HTTP and make malformed representation of resource trying to reach
* XMLParseException
	* Processing XML document
* ArithmeticException
	* Integer division of divide by zero
* ArrayStoreException
	* Putting wrong type of object in array
* ClassCastException
	* Casting invalid class 
* IndexOutOfBoundsException
	* Tried to read non existant index
* NullPointerException
	* Object referred to as an unitialised object 
