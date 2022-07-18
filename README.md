# SOLID

SOLID Principles, a brief introduction with code examples in JavaScript/TypeScript

Software development is hard, really hard.  The reason is the intangible nature of the code. Especially the need to change the software frequently makes software engineering a really challenging job. Just take a look at the below image to have a better understanding.

![](https://lh6.googleusercontent.com/cvIeE8COkCvqpoKOBHJ7Mv7Z8gUa1aRFtNmBpIJAwyajSugnfptH04GWYcmptU5pIZmzWrFPcGmd2kaEx3WUmCTTNHB_W9nH0CDaDm6YBl17nrC43V5MQPxtUT2mAPEsaiV-0JKrYkyA0X1RJ7I)

It took about 6 billion dollars to create Windows Vista [1] while the tallest building in the world just took about 4.1 billion dollars [2] to be completed! 

Fortunately, over the last few decades, lots of computer scientists like Robert C. Martin created really great ideas and tools to make software development a feasible and manageable process. One of these concepts that are of the highest importance in software development is called SOLID principles. SOLID is an acronym for five design principles intended to make object-oriented designs more understandable, flexible, and maintainable [3]. In this article, we try to explain these concepts in plain and understandable words with code examples in JavaScript/TypeScript.

1\. Single-responsibility principle: Eunice

The Single Responsibility Principle appears to be easy but in the truest sense one of the hardest nuts to crack or basically one of the hardest things to get right. The single Responsibility Principle is very crucial for every developer or programmer because when this is upheld, it makes version control easier. It helps to resolve the problem of incompatible modules that teams are always experiencing. Merge conflicts are another example. if the SRP is strictly adhered to, files will have just one single reason to change, and conflicts will be easier to resolve.

For clarity and simplicity, what exactly is the Single Responsibility Principle? SRP ensures that a class or function should have only one reason to change. Each class should do one thing & do it well. When writing a code, we need to start thinking in terms of the responsibilities and who is going to use it because these are major determinants of change.

Few important things to note:

-   A class should do just one thing.

-   Remember some classes change for various reasons so avoid putting different functions in the same class, simply put find one reason to change, and take everything else out of the class or function

-   Everything should be in its place. Think responsibilities, think compartmentalization

-   Specific names should be given to different small classes and standard names to large classes

For instance in an auto company, where there exit different departments. It is advisable to split up tasks accordingly to avoid confusion, and how best can this be done?? Split based on responsibilities by assigning customer related responsibility to the same class, model related responsibility to the same class etc. see below codes for further clarification

👎Bad Practice:
```
|\
class  Auto {\
constructor(model: string) { }\
getCarModel() { }\
saveCustomerOrder(o: Auto) { }\
setCarModel() { }\
getCustomerOrder(id: string) { }\
removeCustomerOrder(id: string) { }\
updateCarSet(set: object) { }\
}
```
 |

From the code above, If the head of each department changes the logic, it will affect the the other department, or lead to bad nested statement.

👍Good Practice

|
```
class  Auto {\
 constructor(model: string) { }\
    getCarModel() { }\
    setCarModel() { }\
}

class  CustomerAuto {\
    saveCustomerOrder(o: Auto) { }\
    getCustomerOrder(id: string) { }\
    removeCustomerOrder(id: string) { }\
}

class  AutoDB {\
    updateCarSet(set: object) { }\
}
```
 |

The above structure obeys the Single Responsibility Principle and every class is responsible for one aspect. Great!

Importance of SRP

-   It helps to adhere to the coding tenet, do not repeat yourself. keep your code DRY. 

-   It helps to tackle the problem from the root. If the code is compartmentalized you can easily effect changes where necessary.

2\. The open-closed principle (Sarah Bühler)

The open-closed principle was first mentioned by Bertrand Meyer in his book Object-Oriented Software Construction (1994). It was picked up by Robert C. Martin and included in his package of principles SOLID (2000):

"Software entities (classes, modules, functions, etc) should be open for extension, but closed for modification."

OPEN for extension

This means, it should be easy to add new functionality to your code.

CLOSED for modification

This means, the code of existing classes should not be edited.

In a "bad" programmed application, it can happen that a small change to a variable (or class, ...) may result in many changes in the whole code, because the modules/classes/... depend on each other.

Think of an application with a bunch of if-else or switch statements.

The requirements of applications will change on time. The basic classes should not be changed and new features should only be added.

One technique is abstraction, which means to hide the implementation details and showing only the functionality to the users (private variables, classes, ...).

Another technique can be inheritance. A method of a basic class can be implemented by child classes. By overriding the method, the functionality can be adapted to the child class's purpose.

Conclusion:

The advantages of this principle are that the code becomes reusable, it can be better maintained, there are fewer side effects when extending, and it leads to fewer merge conflicts.

In one sentence: don't put too much code in a class and avoid too many if-else or switch statements.

👎Bad:

|
```
class Auto {\
constructor(public model: string) { }\
getCarModel() { }\
}

const shop: Array<Auto> = [\
new Auto('Tesla'),\
new Auto('Audi'),\
];

const getPrice = (auto: Array<Auto>): string | void => {\
for (let i = 0; i < auto.length; i++) {\
switch (auto[i].model) {\
case 'Tesla': return '80 000$';\
case 'Audi': return '50 000$';\
default: return 'No Auto Price';\
}\
}\
}

getPrice(shop);
```
 |

👍Good:

|
```
abstract class CarPrice {\
abstract getPrice(): string;\
}

class Tesla extends CarPrice {\
getPrice() {\
return '80 000$';\
}\
}

class Audi extends CarPrice {\
getPrice() {\
return '50 000$';\
}\
}

class Bmw extends CarPrice {\
getPrice() {\
return '70 000$';\
}\
}

const shop: Array<CarPrice> = [\
new Tesla(),\
new Audi(),\
new Bmw(),\
];

const getPrice = (auto: Array<CarPrice>): string | void => {\
for (let i = 0; i < auto.length; i++) {\
auto[i].getPrice();\
}\
}

getPrice(shop);
```
 |

3\. Liskov substitution principle (Ali Khaleghzadegan)

The Liskov substitution principle (LSP) was initially introduced by Barbara Liskov in 1988 [6]. In simple words, this principle says that the objects of a superclass should be replaceable with objects of its subclasses without breaking the application (we want the objects of our subclasses to behave the same way as the objects of our superclass). Just take a look at the below code snippet in TypeScript:

A bad design:

It is often said that inheritance is the ISA relationship. Clearly, a square "IS A" rectangle and since the ISA relationship holds, it is logical to model the Square class as being derived from the Rectangle class.

However, a Square does not need both height and width member variables. Yet it will inherit them anyway!

One solution is to override SetWidth and SetHeight so that when someone sets the width of a Square object, its height will change correspondingly. And when someone sets its height, the width will change with it. Thus, the invariants of the Square remain intact.

|
```
class  Rectangle {\
constructor(public width: number, public height: number) {}

setWidth(width: number) {\
this.width = width;\
}

setHeight(height: number) {\
this.height = height;\
}\
}

class  Square  extends  Rectangle {\
width: number = 0;\
height: number = 0;

constructor(size: number) {\
super(size, size);\
}

setWidth(width: number) {\
this.width = width;\
this.height = width;\
}

setHeight(height: number) {\
this.width = height;\
this.height = height;\
}\
}
```
 |

But what is wrong with the above code? Just take a look a the below function:

|
```
function  area(rectangle: Rectangle) {\
    rectangle.setWidth(5);\
    rectangle.setHeight(4);\
 return rectangle.getWidth() * rectangle.getHeight();\
}
```
 |

We expect to have 20 as the return value when we pass a Rectangle or Square object to the above function. Right? But the truth is that a Square object makes this function corrupted! (We assumed that the height and width vary independently. Which is not the case for a Square)

A good design:

A square might be a rectangle, but a Square object is definitely not a Rectangle object. Why? Because the behavior of a Square object is not consistent with the behavior of a Rectangle object.

The Solution is that all derivatives must conform to the behavior that they expect from the base classes that they use.

4\. Interface Segregation Principle  (ISP)

-  The Interface Segregation Principle is the fourth SOLID design principle represented by the letter "I".

It was Robert C Martin who initially defined the principle by saying that "clients should not be forced to depend on methods it does not use ", by clients, he means classes that implement interfaces, in other words, interfaces should not include too many functions.

Many client specific interfaces are better than one general purpose interface

-  ISP Diagram Explanation :

Figure1. shows a class with many clients, and one large interface to serve them all.

Note that whenever a change is made to one of the methods that ClientA calls, ClientB and ClientC may be affected.

It may be necessary to recompile and redeploy them. This is unfortunate.

![](https://lh6.googleusercontent.com/qCECBUyrldOphml2ypZDgRSzaH87ixD4zFmzIayQ7L1HqFxXBCpNx7y_7S-S1hBNTXDT1Metxab8SdTk4nK9TaIdrdyTRNlVg-1jXH-OA3xmi6O4ZDierHptzmBadAidGzPen8YsURawxlS0pXY)

Figure 2. The methods needed by each client are placed in special interfaces that are specific to that client. Those interfaces are multiply inherited by the Service class, and implemented there.

If the interface for ClientA needs to change, ClientB and ClientC will remain unaffected. They will not have to be recompiled or redeployed.

![](https://lh6.googleusercontent.com/Z6mt5TQKuPXX7FwmVUuhE5Gq7E2q8i9OV7SbQHrvlmwVpK7vmE34PsNtI713E7ESIkMacjqPWQ6_lwIcihL6Ed1QV7b0kP0x1sW30c05dbOTkEE5CQaFa726IcAPQfHrw5QnpILJz6ajE3qvR1I)

-  Example of ISP

Take a look at the following example .

|
```
interface ReportService {\
  createEmployeeReport(employeeId: string): Report;\
  createCustomerReport(customerId: string): Report;\
  createManagementReport(projectId: string): Report;\
}

class EmployeeReportService implements ReportService {\
  createEmployeeReport(employeeId: string): Report {\
return {\
 // ...\
    };\
  }\
  createCustomerReport(customerId: string): Report {\
throw new Error("Method not implemented.");\
  }\
  createManagementReport(projectId: string): Report {\
throw new Error("Method not implemented.");\
  }\
}
```
 |

there is an issue?

The class implementing the interface actually doesn't want to implement two of the three methods defined within the interface. But it is forced to do so.

-  Fixing the issue

One way to fix the issue can now be to actually pull the interface apart, as shown below.

|
```
interface EmployeeReportService {\
  createEmployeeReport(employeeId: string): Report;\
}\
interface CustomerReportService {\
  createCustomerReport(customerId: string): Report;\
}\
interface ManagementReportService {\
  createManagementReport(projectId: string): Report;\
}\
class EmployeeReportServiceImplementation implements EmployeeReportService {\
  createEmployeeReport(employeeId: string): Report {\
 // ...\
  }\
}

// and so on...            
```
 |

-  What are we trying to prevent?

Confusion among client developers

Lower maintenance costs

-  Conclusion

The Interface Segregation Principle (ISP) sets a pretty simple statement:

"No client should be forced to depend on methods it does not use."

------------------------------------------------------------------------------------------------------------------------------------

5\. Dependency inversion principle: (Paul Bauer)

![](https://lh6.googleusercontent.com/P7jLy_J1AIo3xRNqWZXohUwGX5xegP4i236BtpdZkN-pHFFh9bINkM54gSCFCl5LniL0ruflUTw5PI5_yIB1Di7lQ72vz-FYzddbklaQoMXJCKeVYFxJzezijx5wBZPbTaB0WbDucn-1uh7qJko)

The definition of the Dependency Inversion Principle (DIP) according to Robert C. Martin is -  modules that encapsulate high-level policy should not depend upon modules that implement details. Rather, both kinds of modules should depend upon abstractions [8].

"In simple English" it could be described as such:

1\. High-level modules should not depend on low-level modules. Both should depend on abstractions.

2\. Abstractions should not depend upon details. Details should depend upon abstractions.

In our case - an abstraction (abstract class or interface) should not depend on detail (particular class).

Following this rule leads to "inverted" dependencies compared to classical procedural approaches. The following diagram shows the classical approach. A high-level module A uses a low-level module B.

![](https://lh3.googleusercontent.com/QkmgeUqr31T0UzVORSZJXkXZh_97Ikwg_XGOr4GPofWZwODp5SbmKQfdrGohfqeYlqyjXr0I7o4TBSh1x5WPJjcP55yYKHqRprGBeaL7dFgEDNKvB_nsaMELx4hx7Hs0RJF3tBtFnAyC33GI2Smp8qM)

When applying DIP, both modules depend on the abstraction (note that in UML diagrams all arrows point into the direction of the dependency):

![](https://lh4.googleusercontent.com/WMp8LRfGMBha1wm3-M6L_Np9LjgwNqvsKg4lrfzkrmsaeWBDi_da3udp93srDU-vFd8BWa0NFSAWXd404lUVWiLFouETvma4bmEPK0nEIicrBv3pKjg7MBqoHkMOyt_hBzW2KjcL6z1ahi1PmtU)

B is not depended upon anymore but it depends on another module. This is the inverted dependency. [10]

Code example:

👎Bad:

|
```
class  xmlHttpRequestService { }

// Low level\
class  xmlHttpService  extends  xmlHttpRequestService {\
request(url: string, type: string) { }\
}

// High level module\
class  Http {\
constructor(private xmlHttpService: xmlHttpService) { }

get(url: string, options: any) {\
this.xmlHttpService.request(url, 'GET');\
}

post(url: string) {\
this.xmlHttpService.request(url, 'POST');\
}\
}
```
 |

👍Good:

|
```
class  xmlHttpRequestService {\
open() { }\
send() { }\
}

interface Connection {\
request(url: string, options: any): any;\
}

// Low level\
class  xmlHttpService  implements  Connection {\
xhr = new xmlHttpRequestService();

request(url: string, type: string) {\
this.xhr.open();\
this.xhr.send();\
}\
}

// High level module\
class  Http {\
constructor(private httpConnection: Connection) { }

get(url: string, options: any) {\
this.httpConnection.request(url, 'GET');\
}

post(url: string) {\
this.httpConnection.request(url, 'POST');\
}\
}
```
 |

Conclusion
----------

In this post, we started with the history of SOLID principles, and then we tried to explain each principle with a clear understanding of the why's and how's of each principle. We even refactored a simple example to show the bad and good SOLID principles.

For all programmers, we suggest keeping these principles in mind while designing, writing, and refactoring codes so that your code will be much more maintainable, scalable, testable, and reusable.  Practicing these principles can feel overwhelming at first, but regularly working with them and understanding the differences makes this principle more familiar. You don't want a situation where you work on a project and in the next few months or years you are struggling to understand your own code so for the sake of your code and for the sake of other coders learn these principles, love these principles, and live these principles.

REFERENCES:

[1] <https://news.softpedia.com/news/Vista-a-6-Billion-Dollars-Operating-System-44096.shtml>

[2] <https://www.webuildvalue.com/en/infrastructure-news/burj-khalifa-dubai.html>

[3] <https://en.wikipedia.org/wiki/SOLID>

[4] <https://en.wikipedia.org/wiki/Single-responsibility_principle>

[5] <https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle>

[6] <https://en.wikipedia.org/wiki/Liskov_substitution_principle>

[7] <https://en.wikipedia.org/wiki/Interface_segregation_principle>

[8] <https://en.wikipedia.org/wiki/Dependency_inversion_principle>

[9] <https://blog.knoldus.com/what-is-liskov-substitution-principle-lsp-with-real-world-examples/>

[10] DIP: <http://principles-wiki.net/principles:dependency_inversion_principle>

Code examples: ​​<https://github.com/YauhenKavalchuk/useful/blob/main/solid.md>
