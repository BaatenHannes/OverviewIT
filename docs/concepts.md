
## SOLID

### Single Responsibility Principle

>Elke klasse of module heeft de verantwoordelijkheid over één functionaliteit. (Kan doorgetrokken worden tot methodes).

### Open/Closed

>“Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification”.
Je moet een klasse of module kunnen uitbreiden (open) zonder ze aan te passen (closed). Een klasse moet verschillende scenario’s aankunnen die nog niet nader gedefinieerd zijn, zonder ze hiervoor aan te passen.

Bvb: klasse rechthoek met lengte en breedte, en een klasse voor het berekenen van een totale oppervlakte van een verzameling van rechthoeken.

```
Public class Rectangle
{
    public double Width {get; set;}
    public double Height {get; set;}
}

Public class AreaCalculator 
{
    public double Area(Rectangle[] shapes)
    {
        double area = 0;
        foreach(var shape in shapes)
        {
            area += shape.Width*shape.Height;
        }
        return area;
    }
}
```

Bovenstaande klasse kan niet uitgebreid worden naar andere objecten, beter is dus de oppervlakte op object niveau, zodat de areacalculator kan uitgebreid worden naar andere objecten zoals cirkels, zonder deze te moeten aanpassen:

```
public double Area(Shape[] shapes)
{
    double area = 0;
    foreach (var shape in shapes)
    {
        area += shape.Area();
    }
    return area;
}
```

### Liskov Substitution

>Elke afgeleide klasse moet gesubstitueerd kunnen worden voor zijn base/parent. Wanneer een klasse dus in plaats van zijn parent gebruikt wordt, moet alle functionaliteit op dezelfde manier blijven werken.

*A great example illustrating LSP (given by Uncle Bob in a podcast I heard recently) was how sometimes something that sounds right in natural language doesn't quite work in code.
In mathematics, a Square is a Rectangle. Indeed it is a specialization of a rectangle. The "is a" makes you want to model this with inheritance. However if in code you made Square derive from Rectangle, then a Square should be usable anywhere you expect a Rectangle. This makes for some strange behavior.
Imagine you had SetWidth and SetHeight methods on your Rectangle base class; this seems perfectly logical. However if your Rectangle reference pointed to a Square, then SetWidth and SetHeight doesn't make sense because setting one would change the other to match it. In this case Square fails the Liskov Substitution Test with Rectangle and the abstraction of having Square inherit from Rectangle is a bad one.*


###Interface Segregation

>Elke implementatie van een interface of supertype moet gebruik kunnen maken van alle methodes die overgeërfd of geïmplementeerd worden. Er zou nooit een nutteloze methode mogen geërfd of geïmplementeerd worden. **Het opsplitsen van interfaces en klassen indien nodig.** *Hangt sterk samen met Single Responsibility Principle*

*Verschil tussen Single Responsibility Principle en Interface Segregation* is vanwaar je het bekijkt. Zelfde principe vanuit twee verschillende oogpunten:

SRP tells us that you should only have a single responsibility in a module.
ISP tells us that you should not be forced to be confronted with more than you actually need. If you want to use a print() method from interface I, you shouldn't have to instantiate a SwimmingPoolor a DriveThru class for that.
More concretely, and going straight to the point, they are different views on the same idea -- SRP is more focused on the designer-side point-of-view, while ISP is more focused on the client-side point-of-view. So you're basically right.


*The ISP was first used and formulated by Robert C. Martin while consulting for Xerox. Xerox had created a new printer system that could perform a variety of tasks such as stapling and faxing. The software for this system was created from the ground up. As the software grew, making modifications became more and more difficult so that even the smallest change would take a redeployment cycle of an hour, which made development nearly impossible.
The design problem was that a single Job class was used by almost all of the tasks. Whenever a print job or a stapling job needed to be performed, a call was made to the Job class. This resulted in a 'fat' class with multitudes of methods specific to a variety of different clients. Because of this design, a staple job would know about all the methods of the print job, even though there was no use for them.
The solution suggested by Martin utilized what is today called the Interface Segregation Principle. Applied to the Xerox software, an interface layer between the Job class and its clients was added using the Dependency Inversion Principle. Instead of having one large Job class, a Staple Job interface or a Print Job interface was created that would be used by the Staple or Print classes, respectively, calling methods of the Job class. Therefore, one interface was created for each job type, which was all implemented by the Job class.*

### Dependency Inversion
  
>High level modules should not depend upon low level modules. Both should depend upon abstractions.
Abstractions should not depend upon details. Details should depend upon abstractions.
Zorgt voor Decoupling - Loose coupling

In volgend voorbeeld is de hogere level klasse Notification afhankelijk van een lager level klasse Email. Bij nieuwe objecten moet de code gewijzigd worden.

```
public class Email
{
    public void SendEmail()
    {
        // code
    }
}

public class Notification
{
    private Email _email;
    public Notification()
    {
        _email = new Email();
    }

    public void PromotionalNotification()
    {
        _email.SendEmail();
    }
}
```

We lossen deze afhankelijkheid op door een abstractie te maken tussen de twee klassen. Nu is notificatie enkel afhankelijk van een abstracte messageservice, waardoor nieuwe implementaties eenvoudig toegevoegd kunnen worden.

```
public interface IMessageService
{
    void SendMessage();
}
public class Email : IMessageService
{
    public void SendMessage()
    {
        // code
    }
}
public class Notification
{
    private IMessageService _iMessageService;

    public Notification()
    {
        _iMessageService = new Email();
    }
    public void PromotionalNotification()
    {
        _iMessageService.SendMessage();
    }
}
```
 We moeten enkel nog via Dependency Injection de afhankelijkheid weghalen door het object via de constructor te injecteren zoals hieronder.

```
public class Notification
{
    private IMessageService _iMessageService;

    public Notification(IMessageService _messageService)
    {
        this._iMessageService = _messageService;
    }
    public void PromotionalNotification()
    {
        _iMessageService.SendMessage();
    }
}
```


###Dependency Inversion vs Dependency Injection

Als we in onderstaand voorbeeld geen Interface injecteren via Dependency Injection, maar een concrete implementatie via een object, dan doen we aan Dependency Injection maar niet aan Dependency Inversion: de klasse geeft de controle van het instantiëren af, maar is afhankelijk van een concrete implementatie ipv een abstractie waardoor de klassen nog steeds niet loosely coupled zijn.

```
public class Notification
{
    private IMessageService _iMessageService;

    public Notification(IMessageService _messageService)
    {
        this._iMessageService = _messageService;
    }
    public void PromotionalNotification()
    {
        _iMessageService.SendMessage();
    }
}
```

## Inversion Of Control

'Omdraaien van de controle'. The **division between the 'What' and the 'When'** parts of the code. 

Code that does not know and care when it will run, it only knows what it will do and depend on abstractions. 

In traditional programming, the flow of the business logic is determined by objects that are statically bound to one another. With inversion of control, the flow depends on the object graph that is built up during program execution. Such a dynamic flow is made possible by object interactions that are defined through abstractions. This run-time binding is achieved by mechanisms such as dependency injection or a service locator. 

* Factory pattern
* Service locator pattern
* Dpeendency injection
* Strategy pattern

---
## Patterns

### Factory method pattern

In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created, **by calling a factory method rather than by calling a constructor**.

>Puts the pieces together to make a complete object and *hides the concrete type* from the caller.

>"Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses." (Gang Of Four)

Why and when to use:
* A class cannot anticipate the type of objects it needs to create beforehand.
* A class requires its subclasses to specify the objects it creates.
* You want to localize the logic to instantiate a complex object.

### Abstract factory pattern

Variant on the factory method pattern, where instead of creating object in a factory method, you use a factory class to generate objects of a certain type. This is especially usefull when creating **complex objects** in multiple different classes. They can all share a factory class to generate objects, and if the composition of the object changes, only the factory class needs to be changed.

* the **Factory Method pattern** uses inheritance and relies on a subclass to handle the desired object instantiation.
* with the **Abstract Factory pattern**, a class delegates the responsibility of object instantiation to another object via composition.

**Factory method**
Creates object of a subclass of Foo. Which subclass depends on the subclass of A. Inheritence is used to decide the behaviour of f.whatever().
```
class A {
    public void doSomething() {
        Foo f = makeFoo();
        f.whatever();   
    }

    protected Foo makeFoo() {
        return new RegularFoo();
    }
}

class B extends A {
    protected Foo makeFoo() {
        //subclass is overriding the factory method 
        //to return something different
        return new SpecialFoo();
    }
}
```
**Abstract Factory**
Creates objects of a subclass of Foo. Which subclass depends on the specifiek factory that is injected. This way, the behaviour of f.whatever() depends on the type of factory you inject.
```
class A {
    private IFactory factory;

    public A(IFactory factory) {
        this.factory = factory;
    }

    public void doSomething() {
        //The concrete class of "f" depends on the concrete class
        //of the factory passed into the constructor. If you provide a
        //different factory, you get a different Foo object.
        Foo f = factory.makeFoo();
        f.whatever();
    }
}


interface IFactory {
    Foo makeFoo();
    Bar makeBar();
    Aycufcn makeAmbiguousYetCommonlyUsedFakeClassName();
}

//need to make concrete factories that implement the "Factory" interface here
```

### Strategy Pattern

The strategy pattern is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.

Strategy can be assigned to an object by composition, rather then inheritance, implementing an IStrategy interface where you call for example Strategy.DoSomething(). The type of IStrategy will decide the behaviour of the method, and the object can be switched at runtime with another strategy object with another implementation.

```
   public static void Main(String[] args)
    {
        // Prepare strategies
        IBillingStrategy normalStrategy    = new NormalStrategy();
        IBillingStrategy happyHourStrategy = new HappyHourStrategy();

        Customer firstCustomer = new Customer(normalStrategy);

        // Normal billing
        firstCustomer.Add(1.0, 1);

        // Start Happy Hour
        firstCustomer.Strategy = happyHourStrategy;
        firstCustomer.Add(1.0, 2);

        // New Customer
        Customer secondCustomer = new Customer(happyHourStrategy);
        secondCustomer.Add(0.8, 1);
        // The Customer pays
        firstCustomer.PrintBill();

        // End Happy Hour
        secondCustomer.Strategy = normalStrategy;
        secondCustomer.Add(1.3, 2);
        secondCustomer.Add(2.5, 1);
        secondCustomer.PrintBill();
    }
}
```
[The entire code of above example can be found here](https://en.wikipedia.org/wiki/Strategy_pattern)


### Singleton Pattern

A singleton is a class which only allows one instance of itself to be created - and gives simple, easy access to said instance. This is achieved by:

* making the constructor private to prevent outside instantiation
* make a static property which contains one instance of itself

```
public class Singleton
{
    private Singleton()
    {
        // Prevent outside instantiation
    }

    private static readonly Singleton _singleton = new Singleton();

    public static Singleton GetSingleton()
    {
        return _singleton;
    }
}
```


---

##.NET 

###.NET Framework

The .NET Framework consists of the **common language runtime (CLR)** and the **.NET Framework class library**. De CLR (de runtime) is een virtuele machine en manages code at execution time:

* Geheugenbeheer
* Threadbeheer
* Exception handling
* Garbage collection
* Security

Ontwikkelaars die CLR gebruiken schrijven hun code in een zogenaamd hogere programmeertaal zoals C#, Scala of VB.NET. Tijdens het compileren zorgt een .NET-compiler ervoor dat de broncode wordt omgezet naar MSIL, welke tijdens het uitvoeren door de just in time compiler van de CLR wordt gecompileerd naar code die uitgevoerd kan worden op het systeem waar de CLR op dat moment op draait. Dit zorgt ervoor dat applicaties niet hardware afhankelijk zijn, en dus op elk willekeurig systeem kunnen worden uitgevoerd, zolang er een CLR voor is. Door dit proces zijn applicaties wel (iets) trager, omdat ze tijdens het uitvoeren gecompileerd worden.

![CLR](/img/CLRDiagram.png)

---

##REST 

Excellent overview here - [https://restfulapi.net/statelessness/](https://restfulapi.net/statelessness/)

**Representational State Transfer** is a software architecture used to create web services.
RESTful web services allow the requesting systems to access and manipulate textual representations of web resources by using a uniform and predefined set of **stateless** operations.


###Architectural properties

* **performance** in component interactions, which can be the dominant factor in user-perceived performance and network efficiency;[9]
* **scalability** allowing the support of large numbers of components and interactions among components.
* **simplicity** of a uniform interface;
* **modifiability** of components to meet changing needs (even while the application is running);
* **visibility** of communication between components by service agents;
* **portability** - usability of the same software in different environments
* **reliability** in the resistance to failure at the system level in the presence of failures within components, connectors, or data.[9]


###Constraints
CCCLUS - following these constraints ensures a RESTfull Webservice with the properties mentioned above.

####Client-server Architecture
* Separation of user interface and date storage
* Advantages:
    * Portability: Multi platform
    * Scalability: (only scale back-end for example)

####Statelessness
* No client context being stored on the server between requests
* Session state is held in the client
* Session state can be transferred by the server to another service such as a database to maintain a persistent state for a period and allow authentication
* Advantages:
    * Scalability: any server can handle any request
    * 

####Cacheability

####Layered System

####Code on demand

####Uniform interface

---

##Stateless

>Statelessness means that every HTTP request happens in complete isolation. When the client makes an HTTP request, it includes all information necessary for the server to fulfill that request. The server never relies on information from previous requests. If that information was important, the client would have sent it again in this request.

###Application State vs Resource State
Application state is server-side data which servers store to identify incoming client requests, their previous interaction details, and current context information.

Resource state is the current state of a resource on a server at any point of time – and it has nothing to do with the interaction between client and server. It is what you get as a response from the server as API response. You refer to it as resource representation.

REST statelessness means being free on application state.

###Advantages of Statelessness
There are some very noticeable advantages for having REST APIs stateless.

1. Statelessness helps in **scaling** the APIs to millions of concurrent users by deploying it to multiple servers. Any server can handle any request because there is no session related dependency.
1. Being stateless makes REST APIs **less complex** – by removing all server-side state synchronization logic.
1. A stateless API is also **easy to cache** as well. A specific software can decide whether or not to cache the result of an HTTP request just by looking at that one request. There’s no nagging uncertainty that state from a previous request might affect the cacheability of this one. It improves the performance of applications.
1. The server **never loses track** of “where” each client is in the application because the client sends all necessary information with each request.
