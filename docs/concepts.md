
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