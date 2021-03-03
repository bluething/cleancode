![Meaningful Names mind map](https://drive.google.com/uc?export=view&id=1d4K0ktZwhiu2hNsAHiMsL_HfjE-dYpBA)

### What is a function?

1. Do something.  
2. Answer something.  
3. Not allowed to do both in one function.

Look at this code  
```java
public boolean set(String attribute, String value);
```  
What if we use like this  
```java
if (set(”username”, ”unclebob”))…
```  
Is there anything weird?

### Function should do one thing!

"one thing" mean one level of abstraction. You can keep your function small if you strict with this rule.  
Look at this code  
```java
public void sendMailToClients(string[] clients) {
    for (String client : clients) {
        // find client record in db (query)
        if (clientRecord.IsActive())
        {
            // sent mail
        }
    }
}
```  
We can explain what the function does like this "TO sendMailToClients, we iterate clients. In each iteration we query to db to get client record. Then we check if client is active, if true we sent the mail".  
Clearly this function talk too much (query data and check status). Compare with this  
```java
public void sendMailToClients(string[] clients) {
    List<Client> activeClients = getActiveClients(clients);
    for (Client client : activeClients) {
        // sent mail
    }
}

public List<Client> getActiveClients(string[] clients) {
    // find client record in db which status is active
}
```  
We can explain what the function does like this "TO sendMailToClients we get all active client, iterate it. In each iteration we sent the mail".  
The "thing" or abstraction is sent the mail.

If a function does only those steps that are one level below the stated name of the function, then the function is doing one thing.

How we can know if the function doing more than "one thing"? You can extract another function from it with a name that is not merely a restatement of its implementation.

Read the code from top to bottom. Top level is the abstraction, infer it from the name. Read down, and we can see one level (only one) decrease of abstraction. Sample from the book  
```text
To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.
    To include the setups, we include the suite setup if this is a suite, then we include the regular setup.
        To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page and add an include statement with the path of that page.
            To search the parent…
```

### Use descriptive names

Descriptive names can help us to clarify the design. The name should also be consistent.  
Look at this function name  
```text
includeSetupAndTeardownPages, includeSetupPages, includeSuiteSetupPage, and includeSetupPage.
```  
When we read those names, like reading a story.  
Is ok to have long descriptive name instead of short name with some comment to explain what the function does.  
We can use naming convention here.

Good function name is:  
1. Explain intent of the function.  
2. Explain intent and order of arguments.

Function names should say what they do!

### Function arguments

The ideal number of arguments for a function is zero (niladic).  
Next comes one (monadic), followed closely by two (dyadic).  
Three arguments (triadic) should be avoided where possible.  
More than three (polyadic) requires very special justification—and then shouldn’t be used anyway.  

Argument ara hard. Reader need to interpret it each time they saw it. They need to understand it inside the context of function.  
For example this method  
```java
includeSetupPageInto(Content newPage)
```  
What if the argument we move it to instance variable, like this  
```java
class Html {
    Content newPage;
    Html(Content newPage) {
        this.newPage = newPage;
    }
    includeSetupPageInfo() {
        // do something with newPage
    }
}
```  
The intent of newPage more clearly, we can get the context from the class.

###### Function with one argument

The function either:  
1. Asking a question about the argument. Example boolean isActive(User user).
2. Operating on the argument, return value. Example Entity getUser(String uid).

Less common use is as event, there is an input argument but no output argument. Use the argument to alter the state of the system.  
```java
void passwordAttemptFailedNtimes(int attempts)
```  
If you need to this kind of function, please choose names and contexts carefully.

What about output argument? Avoid this!  
Hard to understand. If a function is going to transform its input argument, the transformation should appear as the return value.  
```java
// bad
void transform(StringBuffer out)
// good
StringBuffer transform(StringBuffer in)
```

What about flag argument?  
Flag argument do more than one thing. Split the method into two if a boolean parameter adds multiple responsibilities to the method.

###### Function with two arguments

Hard to understand because the order.  
The order is ok if only if the order is natural.  
```java
// bad
writeField(Stream output, name)
// better
cartesian(int x, int y)

// bad but we use it
// how many times have you put the actual where the expected should be? 
assertEquals(expected, actual)
```  
Consider to wrap the arguments into object, becomes monadic.  

###### Function with three arguments

Same with dyadic, consider to wrap the arguments into object, becomes monadic.
```java
 // bad
Circle makeCircle(double x, double y, double radius);
// good
Circle makeCircle(Point center, double radius);
```  
x and y is a point.

###### Argument lists

All argument must be treated identically.  
```java
String.format(”%s worked %.2f hours.”, name, hours);
```

### Have no side effect

What is a side effect? Something that does a function other than take a value in and return another value or values. Hard to detect before it happen.  
```java
   public class UserValidator {
     private Cryptographer cryptographer;
     public boolean checkPassword(String userName, String password) {
       User user = UserGateway.findByName(userName);
       if (user != User.NULL) {
         String codedPhrase = user.
         getPhraseEncodedByPassword();
         String phrase = cryptographer.decrypt(codedPhrase, password);
         if ("Valid Password".equals(phrase)) {
           Session.initialize();
           return true;
         }
       }
       return false;
     }
   }
```  
Method checkPassword do a password matching between the input and data from a database. Inside that we can see the method not only work with a password but also with a session.  
This make a temporal coupling. If we call checkPassword carelessly, something unexpected about session data will happen.

### Switch statement

The problem:  
1. It's large. Do more than one thing.  
2. Violate SRP.  
3. Violate OCP.  
4. Unlimited function with same name.

```java
   public Money calculatePay(Employee e)
   throws InvalidEmployeeType {
       switch (e.type) {
         case COMMISSIONED:
           return calculateCommissionedPay(e);
         case HOURLY:
           return calculateHourlyPay(e);
         case SALARIED:
           return calculateSalariedPay(e);
         default:
           throw new InvalidEmployeeType(e.type);
       }
     }
```

How to solve?
Using abstract factory pattern. Switch statement to create a polymorphic object.  
```java
public abstract class Employee {
     public abstract boolean isPayday();
     public abstract Money calculatePay();
     public abstract void deliverPay(Money pay);
   }
   ////
   public interface EmployeeFactory {
     public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
   }
    ////

   public class EmployeeFactoryImpl implements EmployeeFactory {
     public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
       switch (r.type) {
         case COMMISSIONED:
           return new CommissionedEmployee(r) ;
         case HOURLY:
           return new HourlyEmployee(r);
         case SALARIED:
           return new SalariedEmploye(r);
         default:
           throw new InvalidEmployeeType(r.type);
       }
     }
   }
```  
It still violate OCP if we add new employee type then we change EmployeeFactoryImpl. We can make other implementation of EmployeeFactory, don't change EmployeeFactoryImpl.

### Don't repeat yourself

### Fail fast!

Functions should do one thing. Error handling is one thing. Just throw your exception to the outermost layer, let them handle it for yoy.

### Follow Edsger Dijkstra’s rules of structured programming.

Dijkstra said that every function, and every block within a function, should have one entry and one exit. Following these rules means that there should only be one return statement in a function, no break or continue statements in a loop, and never, ever, any goto statements.