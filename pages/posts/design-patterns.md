---
title: "Design Patterns Refresher"
description: "This is a refresher on Design Patterns"
tag: oops
coverImage: "/images/design-patterns/design-patterns.jpg"
date: 2024/04/20
author: 
---

There are three types of design patterns: Creational, Behavioral and Structural.

## Creational
### Singleton

The Singleton pattern is used to ensure that a class has only one instance, and it provides a global point of access to that instance.
```csharp
public class ConfigurationManager
{
  private static ConfigurationManager _instance;
  private ConfigurationManager() { } // Private constructor prevents external instantiation
  public static ConfigurationManager Instance
  {
    get
    {
      if (_instance == null)
      {
        _instance = new ConfigurationManager();
      }
      return _instance;
    }
  }
  // Configuration data and methods
}
```

### Factory
The Factory pattern is used to create objects without exposing the creation logic to the client. In .NET, the Factory pattern is implemented using a class or a method that creates objects based on a set of parameters. This pattern is useful when you want to create objects based on a particular set of conditions, and you want to hide the creation logic from the client.
```csharp
public interface  Shape
{
  void Draw();
}

public class Circle : Shape
{
  public void Draw() { ... }
}

public class Square : Shape
{
  public void Draw() { ... }
}

public class ShapeFactory
{
  public static Shape GetShape(ShapeType type)
  {
    switch (type)
    {
      case ShapeType.Circle:
        return new Circle();
      case ShapeType.Square:
        return new Square();
      default:
        throw new ArgumentException("Invalid shape type");
    }
  }
}

public enum ShapeType { Circle, Square }
```
### Builder
The Builder pattern is used to separate the construction of a complex object from its representation. In .NET, the Builder pattern is implemented using a separate class or method that constructs the object step by step. This pattern is useful when you want to create complex objects that have many components, and you want to separate the construction logic from the representation.

```csharp
public class Person
{
    public string? FirstName { get; set; }
    public string? LastName { get; set;}
    public string? Title { get; set; }

}

public class PersonBuilder
{
    private readonly Person person;
    public PersonBuilder()
    {
        person = new Person();
    }
    public PersonBuilder SetFirstName(string firstName)
    {
        person.FirstName = firstName;
        return this;
    }

    public PersonBuilder SetLastName(string lastName)   
    {
        person.LastName = lastName;
        return this;
    }
    public PersonBuilder SetTitle(string title)
    {
        person.Title = title;
        return this;
    }
    public Person Build()
    {
        return person;
    }  
}

PersonBuilder builder = new PersonBuilder();
Person person = builder.SetFirstName("John").SetLastName("Doe").Build();
```
### Dependency Injection
The Dependency Injection pattern is a creational pattern that provides a way to create objects without having to know the details of how they are constructed. This is useful when you need to create objects that have complex dependencies or are difficult to instantiate.
```csharp
public class OrderProcessor
{
  private DatabaseWriter _writer = new DatabaseWriter(); // Tight coupling

  public void ProcessOrder(Order order)
  {
    _writer.WriteToDatabase(order);
  }
}

public interface IOrderWriter
{
  void WriteToDatabase(Order order);
}

public class OrderProcessor
{
  private readonly IOrderWriter _writer;

  public OrderProcessor(IOrderWriter writer) // Dependency injected through constructor
  {
    _writer = writer;
  }

  public void ProcessOrder(Order order)
  {
    _writer.WriteToDatabase(order);
  }
}
```
## Structural
### Adapter
The Adapter pattern is used to convert the interface of a class into another interface that clients expect. In .NET, the Adapter pattern is implemented using a separate class that acts as a bridge between two incompatible interfaces. This pattern is useful when you want to use a class that is not compatible with the existing codebase, and you want to convert its interface to make it compatible.

```csharp
public interface IModernDocument
{
  void Print(string text);
}

public class LegacyDocument
{
  public void PrintText(string text)
  {
    // Legacy logic to format and print text
  }
}

public class LegacyDocumentAdapter : IModernDocument
{
  private LegacyDocument _document;

  public ModernDocumentAdapter(LegacyDocument document)
  {
    _document = document;
  }

  public void Print(string text)
  {
    // Convert modern document format to legacy format
	string legacy_text = text;
    _document.PrintText(legacy_text);
  }

}
```
### Decorator
The Decorator pattern is used to add functionality to an object dynamically. In .NET, the Decorator pattern is implemented using a separate class that wraps the original object and provides additional functionality. This pattern is useful when you want to add functionality to an object without changing its interface.
```csharp
public interface ICoffee
{
    public string GetDescription();
}

public class BasicCoffee : ICoffee
{
    public string GetDescription()
    {
        return "Basic Coffee";
    }
}

public class FancyCoffee : ICoffee
{
    private readonly ICoffee _coffee;
    public FancyCoffee(ICoffee coffee)
    {
        _coffee = coffee;
    }
    public string GetDescription()
    {
        return _coffee.GetDescription() + " but just more facncy";
    }
}

//Usage
ICoffee basiccoffee = new BasicCoffee();
ICoffee fancycoffee = new FancyCoffee(basiccoffee);
Console.WriteLine("Basic Coffee: " + basiccoffee.GetDescription());
Console.WriteLine("Fancy Coffee: " + fancycoffee.GetDescription());
```
### Facade
The Facade pattern is used to provide a simple interface to a complex system. In .NET, the Facade pattern is implemented using a separate class that provides a simplified interface to the existing system. This pattern is useful when you want to simplify the interaction with a complex system, and you want to hide its complexity from the client.
Bridge
The Bridge pattern is used to decouple an abstraction from its implementation, allowing them to vary independently. It provides a way to create a family of related classes with different implementations. This pattern is particularly useful when we have multiple variations of a class that we want to use interchangeably.

## Behavioral
### Observer
The Observer Pattern is a behavioral design pattern that allows an object, called the subject, to notify a list of observers when its state changes. The observers are then automatically notified and updated. A typical example of this pattern is a news agency that notifies its subscribers when a new article is published.
### Command
The Command Pattern is a behavioral design pattern that encapsulates a request as an object, thereby allowing for the parameterization of clients with different requests, queues, and log requests, and supports undoable operations. A typical example of this pattern is a remote control for a TV, where each button press corresponds to a different command.
### Strategy
The Strategy Pattern is a behavioral design pattern that allows a client to choose from a family of algorithms at runtime. This pattern is useful when there are multiple algorithms that can be used to solve a problem, and the choice of algorithm depends on the context. A typical example of this pattern is a sorting algorithm that can be chosen based on the size of the input data.
