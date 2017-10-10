[Previous: Formatting](formatting.md)

*"In priority order, simple code:*

*- Runs all the tests;*

*- Contains no duplication;*

*- Expresses all the design ideas that are in the system;*

*- Minimizes the number of entities such as classes, methods, functions, and the like."*

*Ron Jefferies, author of Extreme Programming Installed and Extreme Programming Adventures in C#*

# Objects and Data Structures
Variables are kept private so we have the freedom to change their type or implementation on a whim without affecting dependencies.

## Data Abstraction
Hiding implementation is about abstractions.  A class should expose an abstract interface that allows its users to manipulate the essence of its data without knowing its implementation.

Bad:

    // Concrete Point
    public class Point {
       public double x;
       public double y;
    }
    
* Clearly rectangular only
* Forced to manipulate coordinates independently and exposes implementation
    
Good:

    // Abstract Point
    public class Point {
      double getX();
      double getY();
      void setCartesian(double x, double y);
      double getR();
      double getTheta();
      void setPolar(double r, double theta);
    }
    
* Might be rectangular or polar yet the interface is still a single data structure
* Enforces an access policy.  Can read coordinates separately but must set as an atomic operation.

Bad:

    public interface Vehicle {
        double getFuelTankCapacityInGallons();
        double getGallonsOfGasoline();
    }
    
Likely these are just accessors of variables.
    
Good:

    public interface Vehicle {
        double getPercentFuelRemaining();
    }
    
Does not expose details about the data and expresses it in abstract terms.

## Data/Object anti-symmetry
* Objects hide their data behind abstractions and expose functions that operate on that data.
* Data structures expose their data and have no meaningful functions.
* Procedural code (data structures) makes it easy to add functions without changing existing data structures but hard to add new data structures because all functions must change.
* Object Oriented code makes it easy to add new classes without changing existing functions but hard to add new functions because all classes must change.

**Procedural:**

    public class Square {
      public Point topLeft;
      public double side;
    }
    
    public class Circle {
      public Point center;
      public double radius;
    }
    
    public class Geometry {
      public double area(Object shape) {
        if (shape instanceOf Square) {
          return //area of square
        }
        
        if (shape instanceOf Circle) {
          return //area of circle
        }
      }
    }
    
* If adding a new function `perimeter()` to `Geometry` the shape classes and any classes that depend on shapes would all be unaffected.
* However, if adding a new shape, all functions in `Geometry` must change to support it.
    
**Polymorphic:**

    public class Square {
      public Point topLeft;
      public double side;
      
      public double area() {
        return //area of square
      }
    }
    
    public class Circle {
      public Point center;
      public double radius;
      
      public double area() {
        return //area of circle
      }
    }
    
* No `Geometry` class needed.
* If adding a new shape, no existing functions are affected.
* If adding a new function, all of the shapes must be changed.

## The Law of Demeter
- A module should not know about the innards of the object it manipulates.
- A method *f* of class *C* should only call methods of:
  - *C*
  - An object created by *f*
  - An object passed as an argument to *f*
  - An object held in an instance variable of *C*

### Train wrecks

Bad:

    final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
    
Better:

    Options opts = ctxt.getOptions();
    File scratchDir = opts.getScratchDir();
    final String outputDir = scratchDir.getAbsolutePath();
    
The containing module knows how to navigate through a lot of different objects.  Is this a violation of Demeter's Law?  
* If the variables are objects then **yes**.  The innards should be hidden.
* If the variables are data structures then **no**.  They naturally expose their internal structure.
* If they were data structures, it would be clearer if it is possible to write the line as

      final String outputDir = ctxt.options.scratchDir.absolutePath;

### Hybrids
* Half data structure and half object.  
* Make it hard to add new functions and hard to add new data structures.  
* Should be avoided and are indicative of muddled design.

### Hiding structure
So then how to get the absolute path from the example above?

Bad:

    ctxt.getAbsolutePathOfScratchDirectoryOption();
    
Leads to an explosion of objects in the `ctxt` object.

Bad:

    ctxt.getScratchDirectoryOption().getAbsolutePath();
    
Presumes `getScratchDirectoryOption()` returns a data structure not an object.

Good:

    BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
    
Further evaluating the code showed that `outputDir` was used to create a file stream with a given class name derived from the `outputDir`.  We want `ctxt` to *do* something, not get its internals.

### Data transfer objects
* A DTO is a data structure class with public variables and no functions.
* Often the first in a series of translation stages that convert raw data in a database into objects in application code.

### Active record
* Special form of DTOs.  A data structure with navigational methods like `save` and `find`.
* Developers often treat these as objects by putting business rule methods in them therefore creating a hybrid.
* Treat these as data structures.

## Conclusion
Good developers understand the differences between object oriented programming and data structures and procedures and choose the best approach for the job at hand.

[Next: Error Handling](error-handling.md)
