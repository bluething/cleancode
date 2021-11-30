![Meaningful Names mind map](https://drive.google.com/uc?export=view&id=1kwUSb31lv8OfShr17z4ciN4mX_Rch84v)

#### Use Intention-Revealing Names

How do we come up with a good name? Your name must answer this question:  
1. Why it exists?  
2. What it does?  
3. How it is used?

Look at this variable, does this variable have a name that can answer the question above?  
```java
int d; // elapsed time in days
```  
compare with  
```java
int elapsedTimeInDays;
```

Do you understand what this function does?  
```java
public List<int[]> getThem() {
     List<int[]> list1 = new ArrayList<int[]>();
     for (int[] x : theList)
       if (x[0] == 4)
         list1.add(x);
     return list1;
   }
```  
1. What exactly is `list1`?
2. Why we need to compare first element of theList with 4? Why we use 4?  
3. How would I use the list being returned?
   
We can make it more clearly with revealing naming  
```java
public List<int[]> getFlaggedCells() {
     List<int[]> flaggedCells = new ArrayList<int[]>();
     for (int[] cell : gameBoard)
      if (cell[STATUS_VALUE] == FLAGGED)
        flaggedCells.add(cell);
      return flaggedCells;
   }
```  
More clearly by extracting to class  
```java
public List<Cell> getFlaggedCells() {
     List<Cell> flaggedCells = new ArrayList<Cell>();
     for (Cell cell : gameBoard)
       if (cell.isFlagged())
         flaggedCells.add(cell);
     return flaggedCells;
   }
```

##### Avoid Disinformation

How we avoid leaving false clues that obscure the meaning of code:  
1. Use reserved word in the appropriate context. For example, `hp`, `aix`, and `sco` would be poor variable names because they are the names of Unix platforms or variants.  
2. Avoid programmers specific word. Which one is better `accountList` vs `accounts`?  
3. Beware of using names which vary in small ways. `XYZControllerForEfficientHandlingOfStrings` and `XYZControllerForEfficientStorageOfStrings`
4. Consistent spelling.
5. Avoid lowercase L or O.

##### Make Meaningful Distinctions

Always remember, _different name have different meaning!_  
Then, do we need a prefix? No! The same as the noise word, we don't need that also.  
Look at this code  
```java
public static void copyChars(char a1[], char a2[]) {
     for (int i = 0; i < a1.length; i++) {
       a2[i] = a1[i];
     }
   }
```  
Do you know the meaning of a1[] or a2[]? They provide no clue to the author’s intention.

Also don't use prefix in all your classes/methods to say that they belong to a specific context. For example, you have `MailingAddress` class in GSD’s accounting module, and you named it `GSDAccountAddress`.

##### Use Pronounceable Names

We discuss by talking, what if we need to think about what the meaning of word other people are saying? Instead of try to understand the context, we try to understand the spoken word.  
How do you spell this word?  
```java
private Date genymdhms;
```

##### Use Searchable Names

Look at this condition  
```java
if (x[0] == 4)
```  
What if this condition is wrong and get our program into trouble. Let say instead of 4, we need 5 there. How do you find this line of code?  
4 has no meaning, is only a number. How if we use 4 in some places in our code? how do we differentiate them?

How about single letter names? Use only for _local_ variables.  
The rule is _the length of a name should correspond to the size of its scope_.

##### Avoid Mental Mapping

Clarity is the king! People reading our code don't need to translate our name into something their know.  
Writers and readers must have the same understanding.

##### How do we name a class?

1. Use noun or noun phrase.
2. Avoid using Manager, Processor, Data, or Info. Usually this happens when our class is responsible for delegating tasks to other classes. Find name with narrower scope. 

Try to make class name specific. Imagine a class with a labeled box. What do you think is in there? It could be anything.  
What if we have many boxes with vague names, and we're searching for some stuff? It will take a time to find it.  
Why is it like that, we already labeled it? Because the label doesn't explain what's inside the box. Breaking SRP.

##### How do we name a method?

1. Use verb or verb phrase.  
2. The name should reveal intent.  
3. Functionality fully understandable from the name.

If we have to look inside the method to understand what it does then it's time to change the name.

For early stage we can use this template  
`Verb(do what) + Noun(to What) = name`  
load + customerDetails = loadCustomerDetails()

Method name anti-patterns  
- Method does more than the name says.  
- Name contains "and", "or", and "if".

##### How do we name a variable?

My personal rules:  
- Never use a single letter or an obscure abbreviation.  
- Always specific.  
- Ideally, one/two words.  
- Booleans should be prefixed with `is`.  
- Use camelCase.  
- Use all caps with underscores for constants.

##### Say what you mean. Mean what you say

Imagine you have a function to delete rows in a database. You name it `holyHandGrenade()` (you want to warn everyone).  
It's clear you use name like `deleteItems()`, everyone knows hat is delete means.

##### One word per concept

You want to get information to database, which one do you choose `fetch`, `retrieve`, or `get`?  
Put your attention to the semantic of the word!

##### Use Solution Domain Names

Our readers (mostly) will be programmers. We can use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth as long as the name not related to problem domain (business).

##### Use Problem Domain Names

When there is no “programmer-eese” for what you’re doing, use the name from the problem domain. Ask your domain expert!.

##### Add Meaningful Context

You need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces.  
For example, you have `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, and `zipcode`.  
All of them form an address. Put them into a class called `Address`.  
When a method has a lot of variables with an unclear context you can try to break it into smaller functions.

The function name provides only part of the context, the algorithm provides the rest.

```java
   private void printGuessStatistics(char candidate, int count) {   String number;
       String verb;
       String pluralModifier;
       if (count == 0) {
         number = ”no”;
         verb = ”are”;
         pluralModifier = ”s”;
       } else if (count == 1) {
         number = ”1”;
         verb = ”is”;
         pluralModifier = ””;
       } else {
         number = Integer.toString(count);
         verb = ”are”;
         pluralModifier = ”s”;
       }
       String guessMessage = String.format(
         ”There %s %s %s%s”, verb, number, candidate, pluralModifier
       );
       print(guessMessage);
     }
```

After given a context

```java
public class GuessStatisticsMessage {

     private String number;
     private String verb;
     private String pluralModifier;
     
     public String make(char candidate, int count) {
       createPluralDependentMessageParts(count);
        return String.format(
          "There %s %s %s%s",
          verb, number, candidate, pluralModifier );
     }
     
     private void createPluralDependentMessageParts(int count) {
       if (count == 0) {
         thereAreNoLetters();
       } else if (count == 1) {
         thereIsOneLetter();
       } else {
         thereAreManyLetters(count);
       }
     }
     
     private void thereAreManyLetters(int count) {
       number = Integer.toString(count);
       verb = "are";
       pluralModifier = "s";
     }

     private void thereIsOneLetter() {
       number = "1";
       verb = "is";
       pluralModifier = "";
     }
     
     private void thereAreNoLetters() {
       number = "no";
       verb = "are";
       pluralModifier = "s";
     }

   }
```