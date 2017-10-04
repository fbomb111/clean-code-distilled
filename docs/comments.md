[Previous: Functions](functions.md)

# Comments

*"Don't comment bad code - rewrite it." - Brian W. Kernighan and P.J. Plaugher*

* Comments are our failure to express ourselves in code. When you need to write a comment see if there isn't another way to express yourself in code.
* Comments are difficult to maintain.  The older it is the more likely it is wrong (misinformation).

### Comments do not make up for bad code
Rather than spending time explaining the mess you made, clean up the mess

### Explain yourself in code

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
Going beyond information regarding the code and getting into the intent of the decision making.  

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
If writing a public API then you should certainly write good docs for it.  Keep in mind the rest of the advice and don't write comments/documentation that would end up misleading.

### Bad comments

#### 

[Next: Formatting](formatting.md)
