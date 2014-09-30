---
layout: base
title: Overview
---

### Hyperstore overview

Hyperstore is a modeling framework providing its own transactional in-memory database. To use it, you must define a domain model with a schema definition and load the domain into Hyperstore. Many domains can be load in Hyperstore and you can create references between elements from different domains. Each element belongs to a specific domain.

Once a domain is loaded, every changes made on domain elements will raise events including property changes, reference changes and element lifecycles.

A domain is manipulating within a session. A session combines the properties of a transaction (Atomicity, Coherence, Isolation and Durability with persistance adapter) and the behavior of a Unit of Work pattern.

#### Schema definition

You can define a schema in many ways :

* Code first : Create a *SchemaDefinition* class containing element defnition with theirs properties and relationships.
* DSL Tools designer : Use the existing *DSL Tools designer* with specifics T4 templates to generate a *SchemaDefinition*.
* Textual DSL : Define the schema with a Domain Specific Language targeting Hyperstore concepts.

For example, the following textual DSL describes a simple Library domain with books, members and loans.

![](/GettingStarted/img/o-dsl.png)

* [Hyperstore Textual Language](/DomainLanguage/Introduction) is a very easy way to describe a domain. This sample generates the following C# code corresponding at something you can write if you use the code first way.

![](/GettingStarted/img/o-codefirst.png)

* Finally you can use the customized DSL Tools designer to define the same domain.

![](/GettingStarted/img/o-dsltools.png)

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
var domain = await StoreBuilder.CreateDomainModel<LibraryDefinition>("lib");
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

> Inside a session, you can not use the await/async paragdim.






