### Use Exceptions rather than return codes

```java
   public class DeviceController {
     public void sendShutDown() {
       DeviceHandle handle = getHandle(DEV1);
       // Check the state of the device
       if (handle != DeviceHandle.INVALID) {
         // Save the device status to the record field
         retrieveDeviceRecord(handle);
         // If not suspended, shut down
         if (record.getStatus() != DEVICE_SUSPENDED) {
           pauseDevice(handle);
           clearDeviceWorkQueue(handle);
           closeDevice(handle);
         } else {
           logger.log("Device suspended.  Unable to shut down");
         }
       } else {
         logger.log("Invalid handle for: " + DEV1.toString());
       }
     }
   }
```

vs

```java
public class DeviceController {
     public void sendShutDown() {
       try {
         tryToShutDown();
       } catch (DeviceShutDownError e) {
         logger.log(e);
       }
   }

    private void tryToShutDown() throws DeviceShutDownError {
      DeviceHandle handle = getHandle(DEV1);
      DeviceRecord record = retrieveDeviceRecord(handle);
      pauseDevice(handle);
      clearDeviceWorkQueue(handle);
      closeDevice(handle);
   }

     private DeviceHandle getHandle(DeviceID id) {
         //logic here
        throw new DeviceShutDownError(“Invalid handle for: ” + id.toString());
        //logic here
    }
   }
```

If we return error code, caller need to check the code. This will clutter the logic, buried in the error code check. It further causes the function [do more than one thing](https://github.com/bluething/cleancode/tree/main/03%20Functions#function-should-do-one-thing).

### Avoid using exception flow control

If we use exception as fow control, the exception will clutter the logic.  
```java
try {
     MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
     m_total += expenses.getTotal();
   } catch(MealExpensesNotFound e) {
     m_total += getMealPerDiem();
   }
```  
From the code above we see that if we can't find meal expense, we get meal pe diem amount for that day. This a normal situation (I think). So how do you locate unusual (ie exceptional) situations?  
The logic hard to understand.

When we can use exception as flow control? _Personally I won't use that_, but I found the answer at the internet (it's a rare condition):  
- You want to break out of a complex algorithm (as opposed to a simple block).  
- You can implement “handlers” to introduce behaviour into complex algorithms.  
- Those “handlers” explicitly allow throwing exceptions in their contracts.  
- Your use case does not pull the weight of actually refactoring the complex algorithm.

### Checked vs Unchecked exceptions, which one to use

#### Checked exception

The exceptions that are checked at compile time. If some code within a method throws a checked exception, then the method must either handle the exception, or it must specify the exception using throws keyword.  
The challenge using checked exception is they can violate OCP. Changes in lower level will force changes in higher level.

#### Unchecked exception

The exceptions that aren't checked at compile time. We can ignore it and never caught.

##### Which one to use

_Personally I chose checked exception_. This makes our code more visible. The caller know if our code toxic and unsafe, they can make decision if want to handle it or throw away into higher level.  
How about OCP violation? Simple, OCP stands for Open-Closed Principle, open for extension but closed for modification. So try not to change your code. If the changes make your code throw a new exception, don't change the caller, create a new one.  
I believe if my class obey SRP, and my function only do one thing, we can obey OCP too. 

### NULL is evil

#### Don't return NULL

```java
public String title(){
        if(/* some condition */){
            //some logic
         return /* some value */
        } else {
            return null;
        }
}
```

What's the problem from the code above? Trust.  
We can't simply call title() without check if the value null or not.  
Why this is bad? Trust will affect in maintenance. When you read the code, you need more time to understand how to treat returned value. The presence of null causes we need to check before use the value.  
Even farther it can affect working relationship in a team. Because we don't trust the code, every time our peer give use a new documentation about the code we will double check (read the code)  to make sure everything is right.  
Another problem is our code will get dirty. We will see checking if the value null in many places.

What is the solution?  
- I prefer to throw a (checked) exception. Mostly, returning null value caused by some exceptional condition.
- Return java.util.Optional. Incorrect semantic, Optional is not entity we're working on.
- Return a collection, empty collection instead of null. A little cleaner than returning null. The trust issue still exist, check if the collection is empty or not.
- Special case pattern.
- Null object design pattern. Like a normal object (same class type) but have different behavior in some method. For example, we're looking for a user by name "Fulan", and he is not found. We return an object that has this name (return "Fulan" via name()) but throw an exception in other method.

#### Don't pass NULL

What if someone give us a null object? Usually we will check for null present. Don't do this, it will make our code ugly.  
Don't do anything, let the program crash, the exception will throw to caller, your friend can fix it later. It's not our responsibility to handle null value.

If we don't want to receive null, don't pass them to someone. Unless we're working in API which expects you to pass null.

### Fail fast vs fail safe, which one to use

#### Fail fast

If something goes wrong, stop the execution immediately. Throw an exception as soon as we encounter the problem, don't catch it, let other layer handle it for you.

#### Fail safe

No matter what happens, the software should try to survive, keep the software running. We can do this by catch then handle the exception.  
Usually when we use fail safe, we forced to deal with returning null value.

#### Which one to use

I prefer to use fail fast. The idea is make our software as fragile as possible and then cover it with unit test. If it's fragile, and we counter an error our unit test will help us to reproduce the situation.  
If the error happens in production, create unit test to reproduce it.  
Don't be afraid of errors, don't hide them, show them to the world.  
Another reason is I don't have to deal with null.

### What if I want to catch the exception

###### Don't catch until you have to

Ideally, one catch per point entrance into the application (outermost layers).

###### Write try-catch-finally first

After handling an exception, we must ensure our program in consistent state.

###### Provide context with exception

- Exception message must contain useful information.  
- The context must be passed on to a higher level. Use exception chain, bring the low level root cause of problem to the highest level of the entire software.

###### Recovery only once

Answer this question before write the code, can we recover from the exception?

[Rare Uses of a “ControlFlowException”](https://blog.jooq.org/2013/04/28/rare-uses-of-a-controlflowexception/)  
[Special Case pattern](https://martinfowler.com/eaaCatalog/specialCase.html)