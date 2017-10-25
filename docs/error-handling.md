[Previous: Objects and Data Structures](objects-and-data-structures.md)

# Error Handling
* At times it's impossible to see what the code does because all of the scattered error handling.  
* Error handling is important but if it obscures logic it's wrong.
* Create separation between the business logic and error handling

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
Should be classified by how they are caught

Bad:

    AMCEPort port = new AMCEPort(12);
    
    try {
        port.open()
    } catch (DeviceResponseException e) {
        // report, log...
    } catch (AMT1212UnlockedExcpetion e) {
        // report, log...
    } ...
    
 Good:
 
     LocalPort port = new LocalPort(12);
     
     try {
        port.open()
     } catch (PortDeviceFailure e) {
        // report, log...
     } ...
     
`LocalPort` is just a wrapper that catches and translates exceptions thrown by the `ACMEPort` class.

Wrap third-party API's:
* Allows for cleaner and easily testable code
* Minimizes dependencies upon it
* Not tied to a vendors design choices

## Define the normal flow
Sometimes you don't want to abort in the `catch` statement and still want to continue business logic

Bad:

    try {
        MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
        m_total += expenses.getTotal();
    } catch (MealExpensesNotFound e) {
        m_total += getMealPerDiem();
    }
    
Good:
    
Make `getMeals` should always return a `MealExpenses` object.  Using a special case pattern could return a `MealExpenses` object subclass with a `getTotal` method that handles the per diem logic which would leave you with just:

    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
    
## Don't return null
* Returning null invites errors, i.e. 
* Returning null creates future work for all the times you then have to check for null with `if (item != null)`
* Consider throwing an exception or returning a special case object
* If it's a third-party API that returns null consider wrapping that method to throw an exception or return a special case object

## Don't pass null

Bad:

    public double xProjection(Point p1, Point p2) {
        return (p2.x - p1.x) * 1.5;
    }
    
If you pass null such as `xProjection(null, new Point(12, 13));` we'll have a runtime error
* Throwing an exception means you'll just have to define a handler anyway
* Assertions make the trouble understandable, but still causes a runtime error
* The rational approach is to forbid passing null by default

## Conclusion
We can write clean robust code if we see error handling as a separate concern, something that is viewable independently of our main logic.


[Next: Boundaries](boundaries.md)
