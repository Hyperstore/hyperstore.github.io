---
layout: base
title: Overview
---

### Hyperstore overview

Hyperstore is a modeling framework providing its own transactional in-memory database. To use it, you must define a domain model with a schema definition and load the domain into Hyperstore.
Many domains can be load in Hyperstore and you can create references between elements from different domains. Each element belongs to a specific domain.

Once a domain is loaded, every changes made on domain elements will raise events including property changes, reference changes and element lifecycles.

A domain is manipulating within a session. A session combines the properties of a transaction (Atomicity, Coherence, Isolation and Durability with persistance adapter) and the behavior of a Unit of Work pattern.

See [Observable domain sample](/Samples/Overview) to see how it's easy to create an observable domain for using data binding mechanism with desktop or mobile applications.

### Using hyperstore

This topic describes brievely the main Hyperstore features.


#### Schema definition

A schema describe a domain and is mandatory to begin manipulate elements in a domain.

> Hyperstore provides a simple schema definition but you can extends it and create your own schema with specific constraints, languages and concepts.

You can define a schema in many ways :

* Code first : Create a *SchemaDefinition* class containing element defnition with theirs properties and relationships.
* DSL Tools designer : Use the existing *DSL Tools designer* with specifics T4 templates to generate a *SchemaDefinition*.
* Textual DSL : Define the schema with a Domain Specific Language targeting Hyperstore concepts.

For example, the following textual DSL describes a simple Library domain with books, members and loans.

![](/GettingStarted/img/O-dsl.png)

* [Hyperstore Textual Language](/DomainLanguage/Introduction) is a very easy way to describe a domain. This sample generates the following C# code corresponding at something you can write if you use the code first way.

![](/GettingStarted/img/O-codefirst.png)

* Finally you can use the customized DSL Tools designer to define the same domain.

![](/GettingStarted/img/O-dsltools.png)

#### Using a schema to instanciate a domain model

To use this domain, you must load the schema in Hyperstore and create a domain instance.

```csharp
// Create the main Hyperstore container
var store = await StoreBuilder.New().CreateAsync();

// Load the schema by using its schema definition (generated previously)
var schema = await store.Schemas.New<LibraryDefinition>().CreateAsync();

// And create a domain instance
var domain = await store.DomainModels.New().CreateAsync("lib");
```

Since these previous steps are very common, you can use a more simple syntax :

```csharp
var domain = await StoreBuilder.CreateDomain<LibraryDefinition>("lib");
```

#### Populate the domain and manipulate elements

All changes with Hyperstore must be made in a session.

```csharp
using( var session = store.BeginSession())
{
    var lib = new Library(domain);
    lib.Name = "Library";
    session.AcceptChanges();
}
```
AcceptChanges method must be called before a session ends else it will be aborted.

All objects manipulations are enabled and domain elements work like any object. References are implemented as **`ICollection<T>`** (or **`IObservableCollection<T>`** if the domain is marked as **observable**)

```csharp
    using (var session = store.BeginSession())
    {
        var b = new Book(domain);
        b.Title = "Book 1";
        b.Copies = 1;
        lib.Books.Add(b);
        session.AcceptChanges();
    }
```

> Inside a session, you can not use the await/async paragdim.

#### Subscribe to domain events

A session is like an **Unit of Work** when it completes it raises many types of events :

* Session completed event : Inform a session is completed (even aborted)
* Domain events on elements, relationships created or removed and properties changes.
* Notification events like custom messages, constraint rule messages, exception in code.

If you subscribe to one or eitheir events you can know every things made in the Hyperstore context.

For example, you can subscribe to change events for the above code creating a library :

```csharp
  domain.Events.EntityAdded.Subscribe(o => Console.WriteLine(o.Event));
  domain.Events.PropertyChanged.Subscribe(o => Console.WriteLine(o.Event));
```

That will provide this output :

```
Add entity lib:9e01da5ebc13412b8c6e1bb61577124e [gettingstarted:library]
Change lib:9e01da5ebc13412b8c6e1bb61577124e.Name = "Library" (V635477562609637111)
```

Events driven is the core of Hyperstore and can allow very powerfull behaviors like :

* [Undo/Redo](/GettingStarted/Undo) manager
* Observable domain to easily implementing Data Binding for example.
* Ready to use with [Reactive Extensions](http://msdn.microsoft.com/en-us/data/gg577609.aspx)
* Collaborative mode sending events over local or remote channels like Azure Service Bus, SignalR, peer to peer and so on... *Hyperstore allows to define custom channel adapters*.
* Persistance capacity with persistance provider saving data when a session is completed.
* ...

Adding undo/redo behavior to a domain, it's very easy :

```csharp
  // Create the manager
  var undoManager = new UndoManager(store);
  // Take the domain into account
  undoManager.RegisterDomain(domain);

  Library lib;
  using (var session = store.BeginSession())
  {
    lib = new Library(domain);
    lib.Name = "Library";
    session.AcceptChanges();
  }

  // Undo previous session changes
  undoManager.Undo();
```

#### Adding constraints

A modeling framework must provide a way to declare easily constraints. **UML** provides **[OCL](http://en.wikipedia.org/wiki/Object_Constraint_Language)** (Object Constraint Language) to declare constraints. **OCL** is very limited and need to learn new language syntax.
Since we are in a C# environment, we can leaverage all the power of C# and specially of **LINQ**.

Constraints with Hyperstore are written in C# and can manipulate all the domain elements and theirs properties.

There are two kinds of constraints with Hyperstore :

* Explicit : Constraints are validated manually by calling the **Validate** method of a domain.
* Implicit : Constraints are validated **every time** a session is well completed and only on involved elements inside the session. This *on the flow* validation is more quicly than a whole validation and can abort a session if the constraint is not valid.

If you want to add a simple constraint validating the Book property *Copies* are always greater or equal than zero :

* In code first :

```csharp
LibraryDefinition.Book.AddImplicitConstraint<Book>(book => book.Copies >= 0, "Invalid value", "Copies");
```

* Or with the textual language :

![](/GettingStarted/img/O-constraint.png)

You can obviously add more complicated constraints calling complex code or containing  **LINQ* statement.


