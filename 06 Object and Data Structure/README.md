![Object and Data Structures mind map](https://drive.google.com/uc?export=view&id=14nmFNDNnA6x03xLniZ5OAe79z4iyg00W)

### The difference between object and data structure

Object _hide_ their data behind abstraction and expose function that _operate_ on that data.  
Data structure _expose_ their data and have _no meaningful_ function.

### Object Oriented vs Procedural

Object oriented code makes it hard to add new function because all the class must change.  
Procedural code makes it _easy to add new function_ without changing existing data structure.

Object oriented code makes it _easy to add new classes_ without changing existing functions.  
Procedural code makes it hard to add new data structures because all the function must change.

###### Procedural code

```java
public class Square {
    public Point topLeft;
    public double side;
}
public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;

}
public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.141592653589793;
    
    public double area(Object shape) throws NoSuchShapeException {
        if (shape instanceof Square) {
            Square s = (Square)shape;
            return s.side * s.side;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle)shape;
            return r.height * r.width;
        } else if (shape instanceof Circle) {
            Circle c = (Circle)shape;
            return PI * c.radius * c.radius;
        } else {
            throw new NoSuchShapeException();
        }
    }
}
```

###### Object oriented code

```java
public class Square implements Shape {
     private Point topLeft;
     private double side;
     public double area() {
       return side*side;
     }
   }

   public class Rectangle implements Shape {
     private Point topLeft;
     private double height;
     private double width;
     public double area() {
       return height * width;
     }
   }
   
     public class Circle implements Shape {
       private Point center;
       private double radius;
       public final double PI = 3.141592653589793;
       public double area() {
         return PI * radius * radius;
       }

   }
```

### Everything is an object is a myth

Sometimes we want the flexibility to add a new data types, we choose an object. Sometimes we want the flexibility to add new behaviors, we choose data structure.  
I think the proper way to create an object is through DDD. At the end of strategic design we will have a bunch of objects from our problem space.

### Why our field must private?

```java
public class Point {
     public double x;
     public double y;
   }
```  
Class Point implemented in rectangular coordinates, and it forces us to manipulate those coordinates independently. This exposes implementation. We have a tight coupling with class Point internal structure. What if we set x value but ley y value empty (null or 0).

```java
public class Point {
     private double x;
     private double y;
     Point(double x, double y) {
         this.x = x;
         this.y = y;
     }
   }
```  
The new class Point, we ensure that both values are set as an atomic operation. The state of object going to be consistent. We don't have access to class Point internal structure, less coupling.

#### Data abstraction

Hiding implementation is about _abstractions_! A class does not simply hide the attribute with private access. Rather it exposes abstract interfaces that allow its users to manipulate _the essence of the data_, without having to know its implementation.  
```java
public interface Vehicle {
     double getFuelTankCapacityInGallons();
     double getGallonsOfGasoline();
   }
```  
This code force (from semantic) us to use gallon as a metric. The code expose implementation.  
Compare to this  
```java
public interface Vehicle {
     double getPercentFuelRemaining();
   }
```  
We abstract the implementation.

### Immutability, the answer why our field must final (and private)

Immutability helps us keep classes small, cohesive, decoupled from each other and maintainable. An object is immutable if it's state can't be modified after it' created.

```java
class Cash {
    private int dollars;
    Cash(int dollars) {
        this.dollars = dollars;
    }
    public void setDollars(int value) {
        this.dollars = value;
    }
}
```  
What if after we create an object of type Cash we call setDollars(10)? The dollars value will be change, not same as when we instantiate the object.

```java
class Cash {
    private final int dollars;
    Cash(int dollars) {
        this.dollars = dollars;
    }
    // will throw an error
    public void mul(int factor) {
        this.dollars *= factor;
    }
}
```  
With private (prevent direct access) and final (can't change after instantiate) we can ensure the object state will be consistent.  
The immutable version like below, always return a new object  
```java
class Cash {
    private final int dollars;
    Cash(int dollars) {
        this.dollars = dollars;
    }
    public Cash mul(int factor) {
        return new Cash(this.dollars * factor);
    }
}
```

##### Immutability will solve below problems:

###### Identity mutability

Happens when comparing two objects.  
```java
Map<Cash, String> map = new HashMap<>();
Cash five = new Cash(5);
Cash ten = new Cash(10);
map.put(five, "five");
map.put(ten, "ten");
five.mul(2);
```  
The map with key five will contain value 10 not 5 anymore.

###### Failure atomicity

We must ensure either have a complete and solid object or failure, nothing in between.  
```java
class Cash {
    private int dollars;
    private int cents;
    Cash(int dollars, int cents) {
        this.dollars = dollars;
        this.cents = cents;
    }
    public void mul(int factor) {
        this.dollars *= factor;
        if(/* something bad happens */) {
            throw new Exception("Error")
        }
        this.cents *= factor;
    }
}
```  
At method mul we try to update dollars and cent. What if something bad happens between them? Half of object will be modified (dollars), and the other half will be left intact (cent).

###### Temporal coupling

```java
Cash price = new Cash();
price.setDollars(10);
price.setCents(5);
System.out.println(price);
```  
It will print 10.05 (ignore the logic to print this).  
What if we change the order of statements.

```java
Cash price = new Cash();
price.setDollars(10);
System.out.println(price);
price.setCents(5);
```  
It will print 10.

The line of codes coupled to each other in chronological order. If I reorder like code above, the logic will be broken.

This is how mutable object usually instantiated and initialized. Set null when creating the object then set the value.

Immutable object eliminate coupling between instantiation and initialization process. Instantiation and initialization happens in one place (constructor).

###### No NULL references

```java
class User {
    private final int id;
    private String userName = null;
    User(int id) {
        this.id = id;
    }
    public void setName(String name) {
        this.userName = name;
    }
}
```  
What if our field doesn't set via setter?  
One more, every [presence of null](https://github.com/bluething/cleancode/tree/main/07%20Error%20Handling#null-is-evil) will cause a check (dirty code).

###### Thread safety

```java
class Cash {
    private int dollars;
    private int cents;
    Cash(int dollars, int cents) {
        this.dollars = dollars;
        this.cents = cents;
    }
    public void mul(int factor) {
        this.dollars *= factor;
        this.cents *= factor;
    }
}
```  
I use Cash object like below  
```java
Cach price = new Cash("$15.10");
// the next two lines in two threads
price.mul(2);
System.out.println(price);
```  
At some cases we will find output "60.20". Method mul run twice but somehow print.out run before set cents at 2nd mul call.

###### Smaller and simpler object

No more setter!

### Never use getter and setter

Setter and getter will bring or object into data structure.  
Setter introduce mutability, less maintainable.  
Getter force us to not using abstraction. If you want to create getter method, the getter will return a new object instead of object attributes.

### Law of Demeter (LoD)

The law said  
For all classes C, and for all methods M attached to C, all objects to which M sends a message must be instances of classes associated with the following classes:  
1. The argument classes of M (including C)  
2. The instance variable classes of C.

From Clean Code book, the Law of Demeter says that a method f of a class C should only call the methods of these:
- C.  
- An object created by f.  
- An object passed as an argument to f.  
- An object held in an instance variable of C.

The law can also be called principle of less knowledge. A services or components must have limited knowledge about other services or components. What is meant by 'knowledge' is internal structure.  
Some people call this law as "talk to friend, not to stranger". What is mean by talk is "tell don't ask" principle, we tell an object to do something. What is mean by "friend" is direct dependency.

The motivation behind the Law of Demeter is to ensure that the software is as modular as possible. The law effectively reduces the occurrences of nested message sendings (function calls) and simplifies the methods.

The law only applied to object, not data structure. Sometimes we get confused and end up with hybrid structure, half object and half data structure. They have functions that do significant things, and they also have either setter or getter.  
Avoid creating this! It hard to add new functions but also make it hard to add new data structures.

This law will prevent train wrecks  
```java
// ctxt.getOptions() return Options object that have getScratchDir()
// Option.getScratchDir() return File object that have getAbsolutePath()
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```  
Why this is bad? getOptions(), getScratchDir(),getAbsolutePath() return the attribute of the object.

Beware, in LoD dot counting is not the point. We can still have dot chain, as long as they not access internal structure.

Why accessing internal structure is a bad idea? Tight coupling and [null value problem](https://github.com/bluething/cleancode/tree/main/07%20Error%20Handling#null-is-evil).

##### The solution?

###### Return an object

getOptions(), getScratchDir(),getAbsolutePath() return an object.

###### Hiding the internal structure

getOptions(), getScratchDir(),getAbsolutePath() become getAbsolutePathOfScratchDirectoryOption(), ctxt object have dependency to Option and File. Be careful for the explosion of this method at ctxt class.

###### Return a data structure

getOptions(), getScratchDir(),getAbsolutePath() return a data structure.

[Law of Demeter](https://www2.ccs.neu.edu/research/demeter/papers/law-of-demeter/oopsla88-law-of-demeter.pdf)  
[The Paperboy, The Wallet,and The Law Of Demeter](https://www2.ccs.neu.edu/research/demeter/demeter-method/LawOfDemeter/paper-boy/demeter.pdf)