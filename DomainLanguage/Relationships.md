---
layout : base
title  : Hyperstore Domain Language
---

## Relationship definition

Relationships are declared using keywords **def relationship** and must declare the relationship definition, as shown in the following example:

```python
def relationship MyRelationship (SourceEntityOrRelationship => TargetEntityOrRelationship*)
{
	// Properties, references and constraints definitions
}
```
If you want override another relationship definition, use the **partial** keyword like this :

```python
def partial relationship MyRelationship (SourceEntityOrRelationship => TargetEntityOrRelationship*)
{
}
```
Relationship definition is mandatory and define three properties :
1. The source and target type of the relationship.
2. If this is an embedded relationship or not with respectively the **=>** and **->** operators.
3. The relationship cardinality with the ***** operator.

| Definition | Example |
|:-|:-|
| OneToOne, not embedded | (source -> target)|
| OneToMany, not embedded | (source -> target*)|
| OneToOne, embedded | (source => target)|
| OneToMany, embedded | (source => target*)|
| ManyToOne, not embedded | (source* -> target)|
| ManyToMany, not embedded | (source* -> target*)|
| ManyToOne, embedded | (source* => target)|
| ManyToOne, embedded | (source* => target*)|

> An embedded relationship is like an **UML composition reference** which means source type 'owns' target type and is responsible for the creation and life cycle of the contained type.

**Partial** allow a relationship to be defined in multi .domain files like C#. It's really interesting when you extend a [domain](../Domain).

Unlike C#, inheritance and implementations use two distinct keywords :

* **extends** for inheritance
* **implemantations** for interface implementations

| Inheritance | Example |
|:-|-|
| Single| **def relationship** MyRelationship  (SourceEntityOrRelationship => TargetEntityOrRelationship*) **extends** BaseRelationship { } |
| Interface implementations | **def relationship** MyRelationship  (SourceEntityOrRelationship => TargetEntityOrRelationship*) **implements** Interface1, Interface2 { } |
| Single with interface implementations | **def relationship** MyRelationship   (SourceEntityOrRelationship => TargetEntityOrRelationship*) **extends** BaseRelationship **implements** Interface1, Interface2 { } |

- BaseRelationship must be another relationship or an [external definition](../Externals) referencing a type inheriting from **Hyperstore.Modeling.ModelRelationship**.
- Interface implementation must be [external definition](../Externals) referencing an interface.

An entity can contains the following members :

* [Properties](../Properties)
* [References](../References)
* [Constraints](../Constraints)


#### Code generation attribute
By default, relationship are generated as a *public partial class*. You can modify this declaration with the **modifier** attribute specifing the modifier C# code.

For exemple, to declare a **protected abstract** class, use the following syntax :

```python
[modifier("protected abstract")]
def relationship MyRelationship (SourceEntityOrRelationship => TargetEntityOrRelationship*) {

}
```

