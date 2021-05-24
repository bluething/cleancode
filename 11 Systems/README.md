### How a city works?

Cities have teams of people who manage particular parts of the city, the water systems, power systems, traffic, law enforcement, building codes, and so forth. Some of those people are responsible for the big picture, while others focus on the details.  
They have appropriate levels of abstraction and modularity that make it possible for individuals and the “components” they manage to work effectively, even without understanding the big picture.

Clean code helps us achieve this at the lower levels of abstraction.

### Separate Constructing a System from Using It

Software systems should separate the startup process, when the application objects are constructed and the dependencies are “wired” together, from the runtime logic that takes over after startup.  
```java
public Service getService() {
     if (service == null)
       service = new MyServiceImpl(…); // Good enough default for most cases?
     return service;
   }
```  
The problem?  
1. Hard-coded dependency? We need MyServiceImpl to compile the class.  
2. How to test? We need to test null scenario and if not null we need to inject a right mock. It's a sign of SRP violation.  
3. Whether MyServiceImpl is the correct object?

##### Separation of Main

![separation of main](https://drive.google.com/uc?export=view&id=1ooYsq5I6MHuIww4faZ7AEZ3T9d-Tmczq)  
The main function builds the objects necessary for the system, then passes them to the application. The application has no knowledge of main or of the construction process. It simply expects that everything has been built properly.

##### Factories

![separation with factories](https://drive.google.com/uc?export=view&id=1GuMe731oyYgPsmZSUciaL-PUHYfpAj_t)  
Sometimes, of course, we need to make the application responsible for when an object gets created. We can use the ABSTRACT FACTORY pattern to give the application control of when to build the LineItems, but keep the details of that construction separate from the application code.

##### Dependency Injection

The class takes no direct steps to resolve its dependencies; it is completely passive. Instead, it provides setter methods or constructor arguments (or both) that are used to inject the dependencies. During the construction process, the DI container instantiates the required objects (usually on demand) and uses the constructor arguments or setter methods provided to wire together the dependencies.

### How a city grows?

Cities grow from towns, which grow from settlements. Build a narrow street then expand wider street. Small buildings and empty plots are filled with larger buildings, some of which will eventually be replaced with skyscrapers.  
This growth is not without pain. How about we build the entire city at the beginning? We can, but maybe we don't need all of that in the end.

It is a myth that we can get systems "right the first time." Instead, we should implement only today’s stories, then refactor and expand the system to implement new stories tomorrow.  
This is the essence of iterative and incremental agility.  
Test-driven development, refactoring, and the clean code they produce make this work at the code level.

How about system level?  
Software systems are unique compared to physical systems. Their architectures can grow incrementally, _if we maintain the proper separation of concerns_.