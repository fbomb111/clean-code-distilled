# Functions

*"You know you are working on clean code when each routine turns out to be pretty much what you expected" - Ward Cunningham*

### Small, small, small
How short?  A function should be:
* Transparently obvious
* Tell a story
* Lead you to the next function in compelling order

### Blocks
* Blocks within `if`, `else`, `while` statements should be one line long and should probably be a function call.  Give the function a descriptive name for documentary value.
* Blocks should not hold nested structures, maximum indents should be one or two.

### Do one thing
*	You know a function is doing more than one thing if you can extract another function from it with a name that is not merely a restatement of its implementation.
*	If you have comments dividing up your function into sections, it’s probably not doing one thing.

### One level of abstraction per function

* Different levels of abstraction should not all be within the same function.
* You should be able to read the function top to bottom out loud like a paragraph and have it within the same level of abstraction 

High-level examples
* `getAllAccounts()`
* `loginUser()`

Mid-level examples
* `getPath()`
* `renderView()`

Low-level examples
* `append(“foo”)` 
* `sort()`


### Switch statements:
By nature switch statements don’t do one thing.  But we should put switches in a low level class and not repeat them.

Bad:

```
class Payroll

public double calculatePay(Employee e) {
  switch(e.type) {
    case SALARIED:
      return calculateSalariedPay(e)
    case HOURLY:
      return calculateHourlyPay(e)
    //...
    //...
  }
}
```

There are unlimited other functions that will have the same structure.  

```
class Payroll

public boolean isPayday(Employee e) {
  switch(e.type) {
    case SALARIED:
      return true
    case HOURLY:
      return false
    //...
    //...
  }
}
```

Good:

Create an `Employee` factory.

```
class EmployeeFactory

public Employee makeEmployee(EmployeeRecord r) {
  switch(r.type) {
    case SALARIED:
      return SalariedEmployee(r)
    case HOURLY:
      return HourlyEmployee(r)
    //...
    //...
  }
}
```

And then you can use it like:

```
class Payroll 
    
employee.calculatePay()
employee.isPayday()
//...
```
    
All switches and logic would be encapsualted in the `Employee` class instead of spread throughout the application in classes like `Payroll`

### Use descriptive names
* A long descriptive name is better than a short enigmatic name
* A long descriptive name is better than a long descriptive comment
* Modern IDE's make changing names trivial.  Don't fear trying out different ones until it makes sense to you.
* Be consistent.
  * Bad:
      
      `includeSuiteSetupPage()`
      
      `addTeardownPageForSuite()`
      
  * Good:
      
      `includeSuiteSetupPage()`
      
      `includeSuiteTeardownPage()`
      
### Function arguments
* The ideal number of arguments for a function is zero.  Followed by one (monadic).  Then two.  Three should require special justification.
* Arguments require a lot of conceptual power
* Arguments make testing harder (you have to test each combination of arguments)

### Common monadic form
Three reasons to pass a single argument to a function 

1. Asking a question about the argument 

    `fileExists("My File")`
    
2. Trasforms the argument and returns the result

    `fileOpen("My File") // Takes a String and returns a InputStream`
    
3. An event (less common).  Takes the argument but gives no output

    `passwordAttemptFailedNTimes(int attempts) // Alters the state of the system if needed`
    
Avoid monadic functions that don't follow these forms such as functions that use an output argument instead of return value

### Flag arguments

Passing a boolean as an argument complicates the method signature and shows the method does more than one thing.

Bad:

    render(true)
    
Good:
```
if (isSuite) {
  renderForSuite()
} else {
  renderForSingleTest()
}
```

### Dyadic functions (two arguments)
* Harder, less natural to understand than monadic
* Sometimes developers become conditioned to ignore certain arguments
* Easier to mix up the arguments if they're not naturally ordered
* Acceptable in cases where arguments have natural cohesion and ordering like `Point p = new Point(0,0)`
* Dyads aren't evil but if you're able to convert them to monadic the better

### Triads
* Even easier to mix up
* Most developers are conditioned to ignore the `message` or mix up the parameters in a function like `assertEquals(message, expected, actual)` 

### Argument Objects
It's okay to combine arguments into a class of it's own, often they may be part of the same concept

Bad:

  `Circle makeCircle(double x, double y, double radius);`
  
Good:

  `Circle makeCircle(Point point, double radius);`
  

### Argument lists
Variable number of arguments is generally equivalent to a single argument

    String.format("%s worked %.2f hours.", name, hours);

Is equivalent to it's declaration:

    String.format(String format, Object... args);. //Dyadic

### Verbs and keywords
Use verb/noun pairs

Good:

    write(name)
    
Better:

    writeField(name)
    
Keywords encode the arguments into the function name and help to remember ordering:

    assertExpectedEqualsActual(expected, actual)
    
### Remove side effects

```
boolean checkPassword(String username, String password) {
  boolean passwordIsValid = // do logic to check on password
  if (passwordIsValid) {
    Session.initalize();
    return true;
  }
  return false;
}
```
* `Session.initalize()` is the side effect because name of the function does not imply that it initalizes a session.
* This causes temporal coupling, the condition that `checkPassword()` can now only be called at certain times or the `session` may be reset.

### Output arguments
Output arguments should be avoided.  If the function changes the state of something have it change the state of it's owning object

Bad:

    appendFooter(s);
    
Does this append `s` as the footer to something or append a footer to `s`?

Checking the signature clarifies but is an extra congintive break

    public void appendFooter(StringBuffer report)
    
Good:

    report.appendFooter();
    
### Command query separation
Functions should either change the state of an object or return something, not both.

Bad:

    // returns true if successful and false if the attribute doesn't exist
    public boolean set(String attribute, String value); 
    
This leads to confusing readability and usage such as:

    if(set("username", "bob"))...
    
Good:

    if (attributeExists("username")) {
      setAttribute("username", "bob");
    }

### Prefer exceptions to returning error codes

Bad:

    if (deletePage(page) == E_OK) {
      if (goodSubCondition) {
         logger.log("page deleted");
      } else {
         logger.log("something failed");
      }
    } else {
      logger.log("delete failed")
    }
    
Leads to deeply nested structures because the caller must deal with the error immediately

Good:

    try {
      deletePage(page)
      //...
    } catch (Exception e) {
      logger.log(e.getMessage());
    }
    
### Extract Try/Catch blocks
Extract bodies of catch and try blocks out into functions of their own

### Error handling is one thing
If you're doing error handling, nothing should be above the `try` keyword nor after the `catch/finally` block

### Dependency magnet
Having some class or enum full of error codes requires the rest of the classes to import them to use them.  When the enum changes everything needs recompiled/redeployed.  Programmers avoid the extra work by reusing old error codes instead of adding new ones.  Just use exceptions.

### DRY (Don't Repeat Yourself)
Duplication may be the root of all evil in software.  Most principles, practices, concepts such as Object Oriented Programming are all strategies for eliminating duplication.

### Miscellaneous
It's okay if your first go at a function comes out long, complicated, nested, arbitrary, and with long argument lists.  Write the tests for the logic.  Then go at it again splitting out functions, changing names, eliminating suplication, shrinking and reordering methods, creating new classes.
