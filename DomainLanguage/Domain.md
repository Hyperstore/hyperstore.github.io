---
layout : base
title  : Hyperstore Domain Language
---

## Domain definition

Domain definition must be the first keyword of the .domain definition file and only one domain can be defined in a file. However, you can have multi domain files for the same domain which will be aggregated during the generation phase.

Use the following syntax to define a domain :

```csharp
domain qualified_name
{
	// Domain element definitions
}
```

*qualified_name* is the domain ful name and it is composed of a namespace and domain name.

> The domain definition will be generated with the suffix *Definition*. For example, for a domain called *MyModel*, the generated class will be *MyModelDefinition*.

### Attribute

You can the **observable** attribut before the domain definition. This attribute defines the domain as *data binding ready*. This implies the following behaviors :

* All domain property changes will raise a **NotifyPropertyChangedEvent**.
* Collections will be generated as **ObservableCollection** and will raise collection change events.
* Entities implement **INotifyDataErrorInfo** and are enabled to provide error message when a constraint is not validate.

To add this attribute just add the word *observable* without parameters like this :

```csharp
[observable]
domain Demo.MyModel
{
}
```

### Domain extension

A domain extension give you the possibility to override an existing domain for adding new element or extending existing elements with new properties or new constraints. An extension can only add artefacts and can not remove or hide existing definition.

When a domain is extending only the project containg the extension is involved. The extendee domain is not updated.

To extend a domain, define a new domain file and define a domain with the same qualified name than the extendee domain and add the **extends** definition.

Example if the extendee domain is in the file named mymodel.domain and the extension in a file named mymodel-ext.domain.

```csharp
domain Demo.MyModel extends "mymodel.domain"
{

}
```

You can define new elements which will be added to *MyModel* domain. If you want override an existing element use the keyword **partial** (Only in the extension file). See [Entities](/DomainLanguage/Entities) for more infos.

