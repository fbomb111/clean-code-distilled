[Previous: Objects and Data Structures](objects-and-data-structures.md)

# Error Handling
* At times it's impossible to see what the code does because all of the scattered error handling.  
* Error handling is important but if it obscures logic it's wrong.

## Use exceptions rather than return codes

Bad:

    public void sendShutDown() {
      DeviceHandle handle = getHandle(DEV1);
      if (handle != DeviceHandle.INVALID) {
        ...
        if (record.getStatus() != DEVICE_SUSPENDED) {
          ...
        } else {
          logger.log("Device suspended");
        }
      } else {
        logger.log("Invalid handle");
      }
    }
    
Good:

    public void sendShutDown() {
      try {
        tryToShutDown();
      } catch (DeviceShutdownError e) {
        logger.log(e);
      }
    }
    
    private void tryToShutDown() throws DeviceShutDownError {
      ...
      throw new DeviceShutDownError("Invalid ID for...");
      ...
    }
    
It's easier to read and separates the algorithm for device shutdown and the error handling.

## Write your `try-catch-finally` statement first
* Exceptions define a scope within your program.
* Helps you define what the user of that code should expect.
* Try to write tests that force exceptions and then add behavior to your handler to satisfy your tests.  This will help you build the transaction of the `try` block first.

## Use unchecked exceptions
* Checked exceptions violate the Open/Closed principle.
* In the calling hierarchy of a large system high level functions call lower level functions and so on.  If a low-level function changes and now must throw a checked exception a `throws` clause must be added to it's signature, meaning every higher calling function now must catch the exception or append it's own `throws` clause.
* Checked exceptions break encapsulation becuase all functions in the path of a throw must know about details of that low-level exception.

## Provide context with exceptions
Create informative error messages, provide enough context to determine the source and location of the error.

## Define exception classes in terms of a caller's needs


[Next: Boundaries](boundaries.md)
