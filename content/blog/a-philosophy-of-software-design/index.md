---
title: A philosophy of software design [Book Notes]
date: 2020-03-22 00:00:00
excerpt: Chapter notes
type: post
blog: true
tags:
    - book
    - notes
    - software design
---

## Chapter One - It's all about complexity

* The greatest limitation in writing software is our ability to understand the systems we are creating.
* Creating working software is expensive. It’s very much a cost and time optimisation game. More complex the software, the more time it takes and more expensive it becomes.
* Good designs are complexity reducers. They keep the complexity level in check.
* Two approaches to fight complexity
    * Eliminate complexity - remove special cases, consistent styling, etc.
    * Encapsulate complexity - so that we can work on a system without dealing with it's complexity  
* Software Design is not an upfront task we do. We have the ability, and we should continuously iterate and improve. 
* Incremental design also means continuous redesign
* The question to ask while making a design change is: Is the software less or more complex? Complexity is a very good measure of design. 


## Chapter Two - The nature of complexity

* Complexity is **anything** related to the **structure** of a software system that makes it hard to **understand and modify**
* Complexity is a point in time measure of the **piece** of code we are trying to understand or modify. for example, if a complex system is rarely used and doesn't need to be understood, it adds little value to the overall complexity
* Complexity is more apparent to readers :) 

**Symptoms of complexity**

* Change amplification - huge change radius for a visible small change.
* Cognitive Load - How much information do I need to know to complete a task. Too much information increases the chances of missing out on something crucial and hence bugs crop up. eg. global variables, API's with destructive side effects.
* Unknown Unknowns - not sure which piece of code to change for completing a task. This is a factor for every person who's new to the team


* One of the most import design goals is to make the system ***obvious***. Obvious systems reduce onboarding thereby reducing time, effort and money

**Causes of complexity**

Complexity is caused by two things - ***dependencies and obscurity***

**Dependencies**

* A *dependency* exist if a given piece of code cannot be understood in isolation.
* Too many dependencies lead to cascading changes which are harder to test and causes more bugs. Huge change amplification also causes high cognitive load

**Obscurity** 

* means unknowns. it happens when the important information is not obvious. for example, a variable with a very generic name that doesn't carry much useful information. example, time, date, etc.
* Hidden dependencies are also obscurities. Obscurities causes unknown unknowns


## Chapter Three - Tactical vs Strategic Programming

**Tactical Programming**

* short sighted
* increases complexity - introduces special cases, dependencies and obscurities one change at a time
* the current tactical implementation makes future tactical implementations harder. **Complexity is incrememntal!**

**Strategic Programming**

* *Most of the code in any system is written by extending the current system. Our jobs as system designers is to facilitate this*. 
* strategic programming is not to invest all the time upfront in design. Every change that goes through should improve the design or atleast not detoriate

## Chapter Four - Modules should be deep

**Modular Design** - design systems so that developers face only a fraction of the overall complexity at any given time

* Modules can take many forms - classes, subclasses, services, etc.
* The goal of modular design is to keep the dependencies between them minimal
* Understanding modules - Interface and Implementation. Interface is the *what* and Implementation is the *how*

Note:
A developer should not need to understand the implementations of modules. This also means that implementation should have minimal side effects. Side effects reduces readability and loosens the interface

**Interfaces**

Interface is the guide on how to use a module. The guide can be classified into two types - formal and informal. 

* Formal information are the method signatures and return types/values. They are checked and verified by the programming language. Either at run time or at compile time. 
* informal information is the information the developer needs to know in order to use the module appropriately. e.g. types of files allowed, number of files allowed, etc.

**Abstractions**

Abstraction is the simplified view of an identity. Interfaces are just abstractions. 

Simplification is tricky. You could add a lot of information but that results into increased cognitive load. Or, in order to give out less information, we might end up hiding something important to build a model of the entity. 

An abstraction that omits important details is a **false abstraction**

**Deep Modules**

powerful yet simple modules

A module might not have an interface at all. e.g. garbage collectors. Garbage collectors reduce the cognitive load of freeing up memory

**Shallow Modules**

one whose interface is complicated relative to the functionality it provides


*Providing choice is good, but interfaces should be designed to keep the common case as simple as possible*

*If an interface has many features, but most developers only need to be aware of a few of them, the effective complexity of that interface is just the complexity of the commonly used features.

## Chapter Five - Information Hiding (and Leakage)

- Information hiding is a way of achieving deep modules
- Information hiding helps in two ways
  - simplifies the interface into a more abstract view
  - makes it easier to evolve the system
- private variables with getters is not information hiding. The information should be completely unknown and irrelevant to the module user

### Information Leakage

If a piece of information is reflected in the interface for a module, then by definition it has been leaked

**Back Door Leakage**

Two or modules depend on a third common shared entity

_example_: A module for writing file and a module for reading files depend on the type of file system. If file system were to change, both the modules will need to change. Although both system hid the information but both will need to change if the file system changes. Hence, the file system is not abstracted out clearly by either modules

**On Large classes**

- Information Hiding can often be improved by making a class slightly longer
- A larger class also allows us to raise the level of interface. Since it has a lot of information to operate on, it can club it and provide it as a single view.

**Information hiding within a class**

- Design the private methods within a class so that each encapsulates some information or capability and hides it from the rest of the class
- Try to minimize the number of places where each instance variable is used. Instance variables are shared across methods and hence increase dependency

## Chapter Six - General Purpose Modules are Deeper

- One of the goals in class design is to allow each class to be developed independently.
- API's should be general purpose. Usage can be specific
- General purpose interface reduces cognitive load
- _General purpose API doesn't mean an API doing too many things_

## Chapter Seven - Different Layer, Different Abstractions

**Pass through methods**

- are a smell. They represent poor class decomposition
- makes class shallower
- interface duplication is violation of SRP

**Decorators**

- A decorator object takes an existing object and extends its functioanlity. It provides an API similar or identical to the underlying object and it's methods invoke the methods of the underlying object
- The motivation for a decorator is to separate special purpose extensions of a class from a more generic core
- _decorators very rarely make sense and there is usually a better alternative_

**Interface vs Implementation**

- if the representation used internally is similar to the abstractions that appear in the interface, then the class is shallow.

**Pass through variables**

A variable that is passed down through a long chain of methods.

- They add complexity because they force all of the intermediate methods to be aware of their existence, even though the methods have no use of it themselves other than passing it down.
- There are multiple ways of eliminating pass through variables
  - shared objects between layers. not good enough to merit it's own existence
  - global objects. no race conditions please!. Never
  - using **context** to hold application's global state. Pass context along. Golang philosophy
    - How to not make context a pass through var? - pass it during object construction
    - If a new variable needs to be added, it can be added to the context
    - Contexts are not necessarily thread safe. The best possible way is to make variables/objects in context to be immutable

## Chapter Eight - Pull Complexity Downwards

_Nothing worthy of noting here_

## Chapter Nine - Better together or better apart

_Given two pieces of functionality, should they be implemented in the same place, or should their implementations be separated_

- large number of simpler components does not necessarily mean a better system. It is hard to keep track of all those components and even harder to find which one to use when the need arises
  - poor subdivision might even result into dependencies and duplication

**Bringing Together**

- bring together if the information is shared
- bring together if it will result into a better interface
- bring together to eliminate duplication
  - shared information is not necessarily repitition. A piece of information might be split but not repeated. We need to handle both cases

**Conjoined Methods**
it should be possible to understand each method independently. if you can't understand the implementation of one method without understand the implementation of the other, it's a red flag.

## Chapter Ten - Define errors out of existence

_Exception handling is one of the worst sources of complexity in software systems_

TLDR: exceptions happen because of special cases. There are not many good ways of handling exceptions. We should just strive to reduce them. In many cases, the semantics of operations can be modified so that the normal behavior handles all situations and there is no exceptional condition to report.

**Handling Exceptions**

- move forward and complete the execution in spite of the exception
- halt the execution and report the exception upwards
  - _aborting can be complicated because at the time of abort the system might have been in an incosistent state_
- Exception can occurs while handling exceptions :facepalm
- Exception handling is really hard to test because by definitions these are exceptions and hard to reproduce in a test environment
- since exception occurs very rarely, a large amount of production is never run. so the reliability of that code is always in execution.
- **The execeptions thrown by the class are a part of it's interace**
- classes with a lot of exceptions are complex that the ones with fewer and are shallower

### Reducing the number of exceptions

**Define errors out of existence**

- fail as less as possible. For example, idemptotent API's for creating resources, code to delete API's shouldn't throw an error if the file doesen't exist.

**Exception Masking**

- Handle exception where it occurs. Very often the upper layers are rarely aware of the downstream exception. Methods such as retries, eventual consistency helps.

**Exception Aggregation**

- The idea behind exception aggregation is to handle many exeptions with a single piece of code; rather than writing distinct handlers for many individual exceptions, handle them all in one place with a single handler

* if a system processes a series of requests, it's useful to define an exception that aborts the current request, cleans up the systems state, and continue with the next request. The exception is caught in a single place near the top of the system's request handling loop
* This exception can be thrown at any time in the processing of the request to abort the request. Different subclasses of this excpetion can be defined for different conditions.

* Exception aggregation works best if an exception propagates several layers up the stack before being handled. This allows more exceptions from more methods to be handled in the same place.

* Exception aggregation replaces several special-purpose mechanisms, each tailored for a particular situation, with a single general-purpose mechanism that can handle multiple situations
