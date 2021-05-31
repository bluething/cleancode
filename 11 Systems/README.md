![System](https://drive.google.com/uc?export=view&id=1gL2azUT3S_etlGCxT-CY4tmgfAslcTbl)

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

```java
package com.example.banking;

   import java.util.Collections;
   import javax.ejb.*;   

   public abstract class Bank implements javax.ejb.EntityBean {

     // Business logic…
     public abstract String getStreetAddr1();
     public abstract String getStreetAddr2();
     public abstract void setStreetAddr1(String street1);
     public abstract void setStreetAddr2(String street2);
     public abstract Collection getAccounts();
     public abstract void setAccounts(Collection accounts);
     public void addAccount(AccountDTO accountDTO) {
       InitialContext context = new InitialContext();
       AccountHomeLocal accountHome = context.lookup(”AccountHomeLocal”);
       AccountLocal account = accountHome.create(accountDTO);
       Collection accounts = getAccounts();
       accounts.add(account);
     }

     // EJB container logic
     public abstract void setId(Integer id);
     public abstract Integer getId();
     public Integer ejbCreate(Integer id) { … }
     public void ejbPostCreate(Integer id) { … }

     // The rest had to be implemented but were usually empty:
     public void setEntityContext(EntityContext ctx) {}
     public void unsetEntityContext() {}
     public void ejbActivate() {}

   }
```  
The code is from original EJB1 and EJB2 architectures that did not separate concerns appropriately and thereby imposed unnecessary barriers to organic growth.  
The business logic is tightly coupled to the EJB2 application "container." Because of this coupling to the heavyweight container, isolated unit testing is difficult.

#### Cross-Cutting Concerns

Cross-cutting concerns is a concern that tend to cut across the natural object boundaries of a domain. For example, persistence.  
The solution, AOP.

### How to start the project?

1. Start small.  
2. Decoupled from any architecture concerns.  
3. Implement user stories quickly.  
4. Adding more infrastructure as we scale up

### Optimize decision making

The agility provided by a POJO system with modularized concerns allows us to make optimal, just-in-time decisions, based on the most recent knowledge. The complexity of these decisions is also reduced.

### Use standard wisely, when they add demonstrable value

Standards make it easier to reuse ideas and components, recruit people with relevant experience, encapsulate good ideas, and wire components together. However, the process of creating standards can sometimes take too long for industry to wait, and some standards lose touch with the real needs of the adopters they are intended to serve.

### System need domain-specific languanges

Domain-Specific Languages allow all levels of abstraction and all domains in the application to be expressed as POJOs, from high-level policy to low-level details.  
A good DSL minimizes the “communication gap” between a domain concept and the code that implements it, just as agile practices optimize the communications within a team and with the project’s stakeholders.

### Additional reading

[Abstract factory](https://refactoring.guru/design-patterns/abstract-factory)  
[IoC container](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans)  
[Dependency injection](https://martinfowler.com/articles/injection.html)  
[Spring AOP](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-api)  
[The complete guide to (external) Domain Specific Languages](https://tomassetti.me/domain-specific-languages/)