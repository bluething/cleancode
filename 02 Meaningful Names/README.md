![Meaningful Names mind map](https://drive.google.com/uc?export=view&id=1kwUSb31lv8OfShr17z4ciN4mX_Rch84v)

#### Use Intention-Revealing Names

How do we come up with a good name? Your name must answered this question:  
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
1. What exactly is lis1?
2. Why we need to compare first element of theList with 4? Why we use 4?
   
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
1. Use reserved word in the appropriate context.  
2. Avoid programmers specific word. Which one is better accountList vs accounts?
3. Consistent spelling.
4. Avoid lowercase L or O.

##### Make Meaningful Distinctions

Always remember, different name have different meaning!  
Then, do we need a prefix? No!. The same as the noise word, we don't need that also.  
Look at this code  
```java
public static void copyChars(char a1[], char a2[]) {
     for (int i = 0; i < a1.length; i++) {
       a2[i] = a1[i];
     }
   }
```  
Do you know the meaning of a1[] or a2[]?

Also don't use prefix in all your classes/methods to say that they belong to a specific context. For example, you have MailingAddress class in GSD’s accounting module, and you named it GSDAccountAddress.

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

##### Avoid Mental Mapping

Clarity is the king! People reading our code don't need to translate our name into something their know. Writers and readers must have the same understanding.

##### How do we name a class

1. Use noun or noun phrase.
2. Avoid using Manager, Processor, Data, or Info.

##### How do we name a method

Use verb or verb phrase.

##### Say what you mean. Mean what you say

Imagine you have a function to delete rows in a database. You name it holyHandGrenade() (you want to warn everyone). It's clear you use name like deleteItems(), everyone knows hat is delete means.

##### One word per concept

You want to get information to database, which one do you choose put, fetch, retrieve, or get?  
Put your attention to the semantic of the word!

##### Use Solution Domain Names

Our readers (mostly) will be programmers. We can use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth as long as the name not related to problem domain (business).

##### Use Problem Domain Names

When there is no “programmer-eese” for what you’re doing, use the name from the problem domain. Ask your domain expert!.

##### Add Meaningful Context

You need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces.  
For example, you have firstName, lastName, street, houseNumber, city, state, and zipcode. All of them form an address. Put them into a class called Address.  
When a method has a lot of variables with an unclear context you can try to break it into smaller functions.