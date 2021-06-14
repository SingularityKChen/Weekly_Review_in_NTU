---
layout: post
title: "[Glean] GoF Design Pattern Overview"
description: "The Overview of GoF Design Patterns: Creational Patterns, Structural Patterns, Behaviour Patterns and J2EE Patterns."
categories: [Glean]
tags: [GoF, Design Pattern]
last_updated: 2021-06-14 11:52:00 GMT+8
excerpt: "The Overview of GoF Design Patterns: Creational Patterns, Structural Patterns, Behaviour Patterns and J2EE Patterns."
redirect_from:
  - /2021/06/14/
---

* Kramdown table of contents
{:toc .toc}
# GoF Design Pattern Overview

## What is Gang of Four (GoF)?[^1]

In 1994, four authors Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides published a book titled Design Patterns - Elements of Reusable Object-Oriented Software which initiated the concept of Design Pattern in Software development.

These authors are collectively known as Gang of Four (GoF).

## Types of Design Patterns[^1]

there are 23 design patterns which can be classified in three categories: Creational, Structural and Behavioral patterns. We'll also discuss another category of design pattern: J2EE design patterns.

| S.N. | Pattern & Description                                        |
| ---- | ------------------------------------------------------------ |
| 1    | **Creational Patterns.** These design patterns provide a way to create objects while hiding the creation logic, rather than instantiating objects directly using new operator. This gives program more flexibility in deciding which objects need to be created for a given use case. |
| 2    | **Structural Patterns.** These design patterns concern class and object composition. Concept of inheritance is used to compose interfaces and define ways to compose objects to obtain new functionalities. |
| 3    | **Behavioural Patterns.** These design patterns are specifically concerned with communication between objects. |
| 4    | **J2EE Patterns.** These design patterns are specifically concerned with the presentation tier. These patterns are identified by Sun Java Center. |

## Creational Patterns[^2]

- Abstract Factory. Allows the creation of objects without specifying their concrete type.
- Builder. Uses to create complex objects.
- Factory Method. Creates objects without specifying the exact class to create.
- Prototype. Creates a new object from an existing object.
- Singleton. Ensures only one instance of an object is created.

## Structural Patterns[^2]

+ Adapter. Allows for two incompatible classes to work together by wrapping an interface around one of the existing classes.
+ Bridge. Decouples an abstraction so two classes can vary independently.
+ Composite. Takes a group of objects into a single object.
+ Decorator. Allows for an object’s behavior to be extended dynamically at run time.
+ Facade. Provides a simple interface to a more complex underlying object.
+ Flyweight. Reduces the cost of complex object models.
+ Proxy. Provides a placeholder interface to an underlying object to control access, reduce cost, or reduce complexity.

## Behaviour Patterns[^2]

+ Chain of Responsibility. Delegates commands to a chain of processing objects.
+ Command. Creates objects which encapsulate actions and parameters.
+ Interpreter. Implements a specialized language.
+ Iterator. Accesses the elements of an object sequentially without exposing its underlying representation.
+ Mediator. Allows loose coupling between classes by being the only class that has detailed knowledge of their methods.
+ Memento. Provides the ability to restore an object to its previous state.
+ Observer. Is a publish/subscribe pattern which allows a number of observer objects to see an event.
+ State. Allows an object to alter its behavior when its internal state changes.
+ Strategy. Allows one of a family of algorithms to be selected on-the-fly at run-time.
+ Template Method. Defines the skeleton of an algorithm as an abstract class, allowing its sub-classes to provide concrete behavior.
+ Visitor. Separates an algorithm from an object structure by moving the hierarchy of methods into one object.

## J2EE Patterns[^1]

+ MVC Pattern. Stands for Model-View-Controller Pattern. Used to separate application's concerns.
+ Business Delegate Pattern. Used to decouple presentation tier and business tier. 
+ Composite Entity pattern. Used in EJB persistence mechanism. A Composite entity is an EJB entity bean which represents a graph of objects.
+ Data Access Object Pattern or DAO pattern. Used to separate low level data accessing API or operations from high level business services. 
+ Front controller design pattern. Used to provide a centralised request handling mechanism so that all requests will be handled by a single handler.
+ Intercepting filter design pattern. Used when we want to do some pre-processing / post-processing with request or response of the application.
+ Service locator design pattern. Used when we want to locate various services using JNDI lookup. 
+ Transfer Object pattern. Used when we want to pass data with multiple attributes in one shot from client to server. Transfer object is also known as Value Object.

# Reference

[^1]: Design Pattern - Overview. https://www.tutorialspoint.com/design_pattern/design_pattern_overview.htm
[^2]: Gang of Four Design Patterns. https://springframework.guru/gang-of-four-design-patterns/