[Previous: Comments](comments.md)

# Formatting

*"Clean code can be read, and enhanced, by a developer other than its original author.  It has unit and acceptance tests.  It has meaningful names.  It provides one way rather than many ways for doing one thing." - 'Big' Dave Thomas, founder of OTI, godfather of the Eclipse strategy*

* Consistently apply formatting rules
* If you're on a team agree to a single set of rules and comply
* Automatted tools can help apply formatting rules for you

## The purpose of formatting
Formatting is about communication.  Style and disclipline survives in the project even after your code has been refactored.

## Vertical formatting
How big should a file be?  In studies from *Clean Code*, average file size varied by enviornment and project, but showed that significant systems (50,000+ lines), can be built with small files (typically 200 lines long with an upper limit of 500.  This is not meant to be a hard rule, but demonstrates that smaller files are easier to understand than larger ones.

### The newspaper metaphor
In a well-written article, the top gives you an idea what the story is about, the first paragraph gives you a synopsis of the story, as you continue down details increase.  A source file should be like an article, the name telling whether we're in the right spot, the top giving high level concepts, details and low level functions toward the bottom.

### Vertical openess between concepts
A group of lines should represent a complete thought.  Use blank lines as a visual cue that identifies new and separate concepts.

### Vertical density
Lines of code that are closely related should appear vertically dense.  Useless comments can break up density.

Bad:

    public ReporterConfig {
    
      /**
      * The class name of the reporter listener
     */
      private String m_className;
    
      /**
      * The properties of the reporter listener
      */
      private List<Property> m_properties = new ArrayList<Property>();
      
    }
      
Good:

    public ReporterConfig {
      private String m_className;
      private List<Property> m_properties = new ArrayList<Property>();
    }
    
### Vertical distance
Within a file, concepts that are closely related should be kept vertically close to each other.  Avoid having the reader hop around the source file spending time and mental energy on trying to understand *what* the system does and not *where* the pieces are.

#### Variable declaration
Should be declared as close to their usage as possible.  Because our functions are short, varaibles should appear at the top.

#### Instance variables
Should be declared in a well-known place, conventionally the top of the file.

#### Dependent functions
If one function calls another they should be vertically close and the caller above the callee if possible.

#### Conceptual affinity
Groups of code that share a common naming scheme or perform variations of the same basic task should be vertically close together.

### Vertical Ordering
Dependencies should point downward.  
* Functions that are called should be below functions that do the calling.
* The most important concepts should come first and low-level details last.  This allows us to skim files without immersing ourselves in the details.

## Horizontal formatting
How wide should a line be?  In studies from *Clean Code*, 40% of lines length was between 20 and 60 characters.  30% are less than 10 characters.  There's a signifcant drop-off after 80 characters.  Again, this is no hard rule, but programmers clearly prefer shorter lines.

### Horizontal openess and density

Bad:

    int lineSize=line.length();
    
Good:

    int lineSize = line.length();
    
Assignment statments have two distinct elements, the left side and the right side and spacing makes that obvious.

Bad:

    private void measureLine (String line)
    
Good:

    private void measureLine(String line)
    
No space between the function name and the left parenthesis because the function and it's arguments are closely related.

Bad:

    return b * b - 4 * a * c;
    
Good:

    return b*b - 4*a*c;
    
Spacing can be used to accentuate the precendence of operators.

### Horizontal alignment



[Next: Objects and Data Structures](objects-and-data-structures.md)
