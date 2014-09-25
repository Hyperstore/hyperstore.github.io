---
layout : base
title  : Hyperstore Domain Language
---

## Entity definition

Entities are declared using keywords **def entity**, as shown in the following example:

```csharp
def entity MyEntity
{
	// Properties, references and constraints definitions
}
```
If you want override another entity definition, use the **partial** keyword like this :

```csharp
def partial entity MyEntity
{
}
```

**Partial** allow an entity to be defined in multi .domain files like C#. It's really interesting when you extend a [domain](../Domain).

Unlike C#, inheritance and implementations use two distinct keywords :

* **extends** for inheritance
* **implementations** for interface implementations

| Inheritance | Example |
|:---------------------------------------- | ---- |
| Single| **def entity** MyEntity **extends** BaseEntity { } |
| Interface implementations | **def entity** MyEntity **implements** Interface1, Interface2 { } |
| Single with interface implementations | **def entity** MyEntity  **extends** BaseEntity **implements** Interface1, Interface2 { } |

- BaseEntity must be another entity or an [external definition](../Externals) referencing a type inheriting from **Hyperstore.Modeling.ModelEntity**.
- Interface implementation must be [external definition](../Externals) referencing an interface.

An entity can contains the following members :

* [Properties](../Properties)
* [References](../References)
* [Constraints](../Constraints)


#### Code generation attribute
By default, entity are generated as a *public partial class*. You can modify this declaration with the **modifier** attribute specifing the modifier C# code.

For exemple, to declare a **protected abstract** class, use the following syntax :

```csharp
[modifier("protected abstract")]
def entity MyEntity {

}
```

