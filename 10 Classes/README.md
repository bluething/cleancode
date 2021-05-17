### Class Organization

Following the standard Java convention, a class should begin with a list of variables. Public static constants, if any, should come first. Then private static variables, followed by private instance variables. There is no place for public variables. Public functions should follow the list of variables.  
What about private functions called by public functions? For me there is no place for private functions.

### Encapsulation

Is all about [data abstraction](https://github.com/bluething/cleancode/tree/main/06%20Object%20and%20Data%20Structure#data-abstraction).  
Be careful with getter and (especially) setter, they break encapsulation. Encapsulation not only about data hiding but even further hide the implementation.  
Encapsulation also make our class loose coupling.

### Classes should be small

How do we measure class size? Using their responsibilities, yes we talk about SRP (Single Responsibility Principle).  
The Single Responsibility Principle (SRP) states that _a class or module should have one, and only one, reason to change_.

Naming is probably the first way of helping determine class size. If we cannot derive a concise name for a class, then it’s likely too large. The more ambiguous the class name, the more likely it has too many responsibilities.  
For example, Processor or Manager or Super.  
We can follow this rule when describing the class, write a brief description of the class in about 25 words, without using the words "if," "and," "or," or "but."

Why do we often create classes that have many responsibilities? We are too focused getting software to work than create maintainable software.  
Many of us have a fear that numerous small, single-purpose classes makes it more difficult to understand the bigger picture. The reality a system with many small classes has no more moving parts than a system with a few large classes.

### Low coupling, high cohesion

[Coupling](https://www.geeksforgeeks.org/software-engineering-coupling-and-cohesion/) is the measure of the degree of interdependence between the modules. A good software will have low coupling.  
Low coupling can be achieved by having fewer classes linking to one another. The best way to reduce coupling is by providing an API (interface).  
When we want to achieve low cohesion be careful about [destructive decoupling](https://enterprisecraftsmanship.com/posts/cohesion-coupling-difference/) (a situation where the code is indeed decoupled but at the same time doesn’t have a clear focus).

[Cohesion](https://www.geeksforgeeks.org/software-engineering-coupling-and-cohesion/) is a measure of the degree to which the elements of the module are functionally related. It is the degree to which all elements directed towards performing a single task are contained in the component.  
A highly cohesive class is easy to maintain and can evolve and grow in a controlled manner.
```java
public class Stack {

     private int topOfStack = 0;
     private List<Integer> elements = new LinkedList<Integer>();
     
     public int size() {
       return topOfStack;
     }
     
     public void push(int element) {
       topOfStack++;
       elements.add(element);
     }
     
     public int pop() throws PoppedWhenEmpty {
       if (topOfStack == 0)
         throw new PoppedWhenEmpty();
       int element = elements.get(--topOfStack);
       elements.remove(topOfStack);
       return element;
    }
   }
```  
You can see above code is an example of high cohesion class. All methods (except size()) use all variables.

### Favour composition over inheritance

Inheritance is when you design your types after what they are, while composition is when you design your types after what they can do. There is a simple but good [video](https://www.youtube.com/watch?v=wfMtDGfHWpA) explain about this.  
I prefer to use composition rather than inheritance. It's help us create loosely coupled class.

### Organization for change

For most systems, change is continual. Every change subjects us to the risk that the remainder of the system no longer works as intended.  
How do we make a safety changes? OCP, software entities (classes, modules, functions, etc.) should be open for _extension_, but closed for modification.

### Isolating from change

Needs will change, therefore code will change. A client class depending upon concrete details is at risk when those details change. We can introduce interfaces and abstract classes to help isolate the impact of those details.  
We know this as DIP (Dependency Inversion Principle)

###### Additional reading materials

1. [Composition vs. Inheritance: How to Choose?](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose)  
2. [Stackoverflow - Prefer composition over inheritance?](https://stackoverflow.com/questions/49002/prefer-composition-over-inheritance)  
3. [Prefer Composition Over Inheritance](https://betterprogramming.pub/prefer-composition-over-inheritance-1602d5149ea1)