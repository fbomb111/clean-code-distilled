[Previous: Introduction](index.md)

# Meaningful Names

### Intention revealing names:

Bad:

    int d; //elapsed time in days

Good:

    int elapsedTimeInDays;


Bad:

    If (x[0] == 1) { //…
  
Good:

    If (cells[STATUS_VALUE] == FLAGGED) { //…
  
Better:

    If (cell.isFlagged) { //…


### Avoid names which vary in small ways

It's too difficult to quickly see the difference

    XYZControllerForEfficientHandlingOfStrings
    XYZControllerForEfficientStorageOfStrings

### Inconsistent spelling leads to disinformation.  
Proper spelling, sorting similar things alphabetically, and using obvious differences helps when using autocompletion.

### Non-Noise words

Bad:
* `Product` and `ProductInfo` (indistinct)
* `getAccount()` and `getAccountInfo()` (indistinct)
* `ProductTable` (redundant)
* `NameString` (redundant)

### Make names pronounceable:
You need to be able to communicate like: “Hey why is the record’s generation time stamp wrong?”

Bad:

    Date genymdhms;
    
Good:
    
    Date generationTimeStamp;


### Make names searchable:
Single letter names and numeric constants are not easy to locate.  

Bad:
    
    if (count == MAX_CLASSES_PER_STUDENT) { // is easy to search and find
    
Good:

    if (count == 7) { // is not easy to search and find


### Avoid encodings
It just adds another layer of mental burden and way to mistype

#### Hungarian Notation
With compilers that enforce types it’s no longer necessary to name a variable `intProductCode` or `strUserID`.  

#### Member prefixes
Editors are good enough to highlight member variables, not necessary to prefix with `m_ProductCode`


### Class names
Should be noun or noun phrase, never verb

Bad:

* Manager
* Processor
* Data
  
Good:

* Customer
* Account 
* AddressParser

### Method names (verb or verb phrase)

Bad:

* Don’t be cute – `handGernade()` might be fun, but `deleteItems()` is more helpful.

Good methods:

* `postPayment()`
* `deletePage()`
  
Good accessors, mutators, predicates:

* `getName()`
* `setName()`
* `isPosted()`


### Consistency

* Don’t use `fetch`, `retrieve`, and `get` across different classes.  Pick one and stick with it.
*	`controller`, `manager`, and `driver` can also be equally confusing throughout a code base
*	If the code base uses `add` to always concatenate two values and you write a new method to put a parameter into a collection, you may be tempted to start the method with `add` for consistency.  But since the semantics are different, consider using `insert` or some other name for differentiation.

### Use solution domain names
We’re all programmers here so it’s okay to use CS terms, algorithm names, pattern names, etc.  

### Use problem domain names
If you can’t get it from the solution domain name, get it from the problem domain name.

### Add meaningful context
If you had a variable named `state`, the reader might not know what its for. You could call it `addrState` to clarify. Better though would be to name the class `Address` which provides context for `state`.

### Don’t add gratuitous context
If your app is called `Gas Station Deluxe` don’t prefix every class with `GSD`.  You’ll have a hard time everytime you want to use autocomplete.
  
### Miscellaneous 

* Lowercase l and uppercase o look too much like 1 and 0.
* Single letter names should only be used as local variable in short methods if at all.  This avoids mental mapping.  Using single letter naming just forces readers to have to map the concept anyway

Don’t be afraid to rename things for the better.  You might think developers will reject the idea but most of the time we don’t memorize and we use our modern tools to deal with those details.  Don’t let the idea of surprising someone stop you from leaving code better than you found it.


[Next: Functions](functions.md)
