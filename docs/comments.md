[Previous: Functions](functions.md)

# Comments

*"Don't comment bad code - rewrite it." - Brian W. Kernighan and P.J. Plaugher*

* Comments are our failure to express ourselves in code. When you need to write a comment see if there isn't another way to express yourself in code.
* Comments are difficult to maintain.  The older it is the more likely it is wrong (misinformation).

#### Comments do not make up for bad code
Rather than spending time explaining the mess you made, clean up the mess

#### Explain yourself in code

Bad:

    // Check to see if the employee is eligble for full benefits
    if ((employee.flags == HOURLY_FLAG) && (employee.age > 65))
    
Good:

    if (employee.isEligibleForFullBenefits())
    
### Good comments
    
#### Legal comments
Copyright, authorship, etc, are reasonable comments

#### Informative comments

Good:

    // format matched kk:mm:ss EEE, MMM dd, yyyy
    Pattern timeMatcher = Pattern.compile("\\d*:\\d*\\d* \\w*, \\w* \\d*, \\d*");"
    
#### Explanation of intent
Get into the intent of the decision making.  Good comments explain **why** not **what**.

Good (In the example of a test case):

    // Creating a large number of threads in an attempt to simulate a race condition
    
#### Clarification
When part of a standard library or in code you cannot alter, claryfing comments can be helpful.  However, always verify the clarifying comment is correct and ensure there is not a better way to make the code more expressive.

Good:

    assertTrue(a.compareTo(a) == 0);    // a == a
    assertTrue(a.compareTo(b) != 0);    // a != b
    assertTrue(ab.compareTo(ab) == 0);  // ab == ab
    assertTrue(a.compareTo(b) == -1);   // a < b
    
#### Warning of consequences

Good:

    // SimpleDateFormat is not thread safe 
    // so we need to create instance independently
    
#### TODO Comments
A `TODO` is not an excuse to leave bad code in the system.  It's for tasks that need done but for some reason can't be done at the moment.  Most IDE's are good enough to locate all the `TODO` comments so they're not forgotten.  Don't litter the code with them, clean them up regularly.

Good:

    // TODO: these are not needed
    // We expect this to go away when we do the checkout model
    
#### Amplification
Amplify the importance of something that may otherwise seem inconsequential.

Good:

    // the trim is important because it removes the starting spaces that could cause
    // the item to be recognized as another list
    
#### Documentation
If writing a public API then write good docs for it.  But don't write comments/documentation that would end up misleading.

### Bad comments

#### Mumbling
Leaving a comment because it's policy or we're in a hurry

Bad:

    public void loadProperties() {
        try {
            String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
            FileInputStream propertiesStream = new FileInputStream(propertiesPath);
            loadedProperties.load(propertiesStream);
        } catch (IOException e) {
            // No properties files means all defaults were loaded
        }
    }
        
Who loads the defaults?  When were they loaded?  This comment doesn't mean anything to anyone besides the author, could have been a filler for leaving the `catch` block empty, or even possibly a reminder to come back later to work on this section further.
 
#### Redundant comments
When the comment details what should be fairly obvious just by reading the code.
    
Bad:

    /**
    * The Loader implementation with which this Container is associated
    * /
    protected Loader loader = null;
    
#### Misleading comments
Can sometimes be more misleading than the code itself when accepted as truth.

Bad:

    // Utility method that returns when this.closed is true.  
    // Throws an exception if the timeout is reached.
    public synchronized void waitForClose(final long timeoutMillis) throws Exception {
        if (!closed) {
            wait(timeoutMillis);
            if (!closed) 
                throw new Exception("MockResponseSender could not be closed");
        }
    }

The above is not only redundant but also misleading.  Because of the timeout the method only returns **if** `this.closed` is `true`, not **when**.  It may become `true` during the timeout and the caller would still have to wait for the timeout to complete.

#### Mandated comments
Not every function or variable needs a comment just because.  It generally just adds clutter.

#### Journal comments
This is what source control is for.

Bad:

    // 03-OCT-2012 : Fixed errors reported by CheckStyle
    // 05-JAN-2013 : Implemented Comparable
        
#### Noise comments
Redundant comments.  Personal comments.  Noise causes programmers to start glazing over comments as if they're not important and then you miss the important ones.

Bad:

    // Give me a break!
    
#### Scary noise
The comments here were worthless enough that they were copy and pasted as the variables were being created. If there's no care in creating comments readers won't care enough to read them.

Bad:

    /** The version */
    private String version;
    
    /** The version */
    private String info;
    
#### Dont use a comment when you can use a function or a variable

Bad:

    // does the module from the global list <mod> depend on the subsystem we are a part of?
    if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
    
Good: (no comments needed)

    ArrayList moduleDependees = smodule.getDependSubsystems();
    String ourSubSystem = subSysMod.getSubSystem();
    if (moduleDependees.contains(ourSubSystem));
    
#### Position markers
Sometimes it makes sense to use 'banners' but only sparingly when the benefit is significant.  Otherwise it adds noise.

Bad:

    // Actions ///////////////////////////////////
    
#### Closing brace comments
If your blocks are long enough that you need a comment to remember what block the closing brace is a part of, try shortening the function.

#### Attributions and bylines
Again, this is what source control is for.

Bad:

    /* Added by Rick */
    
#### Commented out code
* Normally gives no indication as to why it's commented out.
* Others are afraid to delete commented code thinking it's important so it never goes away.
* We have source control.
    
#### HTML comments     
Makes comments difficult to read.  If you're adding HTML comments because they'll be extracted by some tool like JavaDoc, it's the tools responsibility to adorn comments with the appropraite HTML, not the programmers.

#### Non local information
Don't offer systemwide information in the context of a local comment

Bad:

    // Port on which fitnesse would run.  Defaults to <b>8082</b>
    public void setFitnessePort(int fitnessePort)
    
#### Too much information
Don't put historical, irrelevant, or otherwise arcane information in comments.

#### Inobvious connection

Bad:

    // Start with an array that is big enough to hold all the pixels (plus filter pixles)
    // and an extra 200 bytes for header info
    this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
    
What is a filter byte?  Is a pixel a byte?  Why '200'?  The connections between the comment and code are not obvious

#### Function headers
A short well-named function probably doesn't need a comment

#### JavaDocs in nonpublic code
Generating documentation for classes and functions inside the system is not useful as they won't be publically consumed.

  
[Next: Formatting](formatting.md)
