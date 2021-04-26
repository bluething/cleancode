![Boundaries](https://drive.google.com/uc?export=view&id=1Lqd2ZiwQBGdZFABX-IWzAukg0y4cUZuD)

How to integrate code with 3rd-party or open source software or in house solution made by other team. Avoid our code know too much about third-party code.

###### _Depend on something you control than on something you don't control._

### Using third-party code

There is a natural tension between the provider of an interface and the user of an interface. Providers of third-party packages and frameworks focus on wide audience. Users focus on their particular needs.

Example problem, let’s look at java.util.Map. They have a very broad interface with plenty of capabilities. What if we pass a Map in our method?  
Our design don't expect user to delete all entries in the map. Map in Java have a method named clear() to removes all the mappings from the map. Someone can make a mistake by accidentally calling this method.

The solution is wrap the interface into a class. Don't pass interface around your system.  
```java
public class Sensors {

     private Map sensors = new HashMap();
     
     public Sensor getById(String id) {
       return (Sensor) sensors.get(id);
     }
     //snip
   }
```  
The Sensors class can enforce design and business rules.

### How we start using third-party code?

##### Learning tests are better that free

Learning tests are an easy (and free) way to learn and future-proof use of the API. When there are new releases of the third-party package, we run the learning tests to see whether there are behavioral differences.  
Without these boundary tests to ease the migration, we might be tempted to stay with the old version longer than we should.

##### What if the interface not designed yet?

Sometimes you have boundaries in your system that represents things that had not been designed yet. To keep from being blocked, define your own interface. This was the interface we wished we had. The interface is under our control. This helps keep client code more readable and focused on what it is trying to accomplish.  
One the real interface was defined, we can create an [adapter](https://refactoring.guru/design-patterns/adapter) to bridge the gap.