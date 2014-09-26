---
layout : base
title  : Hyperstore Domain Language
---
## External references

If you domain element definitions need to reference external types (Types not defined in a domain file) like custom interfaces, specific types from external libraries, you must declare them in the domain file. This is necessary for the Hyperstore compiler to make semantic validations.

You can declare three externals types :

* **interfaces** which will be used for define element implementations with the **implements** keyword.
* **enum** wich will be used for property type definition.
* **classes** which be used for property type definition as value object.

#### Interface declarations

Interfaces are declared specifically to qualify them as implementation candidates.

The syntax is :

```csharp
	extern interface qualified_name [as alias];
```

* *qualified_name* is the full name of the interface including its namespace.
> There is no validation during the generation to verify that is a valid type. If not, there will be a c# compilation error when the project will be compiled.

* You can declare an alias to reference this interface. It must be unique. If you omit the alias, you must use the qualified_name to reference the interface.

#### Enum declarations

Enums are declared like interfaces but the **enum** keyword.

```csharp
	extern enum qualified_name [as alias];
```
The same rules apply :

* *qualified_name* is the full name of the enum including its namespace.
> There is no validation during the generation to verify that is a valid type. If not, there will be a c# compilation error when the project will be compiled.

* You can declare an alias to reference this enum. It must be unique. If you omit the alias, you must use the qualified_name to reference the enum.

Enum are used for property type only.

#### Value object declarations

If you want use a specific type for a property, you must define this type as a value object. A value object is an immutable object and can be any existing type.

When you use this sort of object, Hyperstore needs to know how to serialize/deserialize its state and if it owns specific constraints.

For that, you need to write some custom code excepts if it override a primitive type (string, int, double, datetime...). In this case, you must declare it as a domain valueObject not as an extern type. See [Value objects](/DomainLanguages/ValueObjects) to see how to do.

Else you can declare this type as an external type to use it with a very similar syntax then the two others but without a specific keyword :

```csharp
	extern qualified_name [as alias];
```
Obviously, the same rules apply :

* *qualified_name* is the full name of the type including its namespace.
> There is no validation during the generation to verify that is a valid type. If not, there will be a c# compilation error when the project will be compiled.

* You can declare an alias to reference this type. It must be unique. If you omit the alias, you must use the qualified_name to reference the type.

But declaring an external value object is not enough. You must provide some custom code to Hyperstore to define how the type is serialized/deserialized.

If you omit to provide this code, the project will not compile. To help you editing this code, Hyperstore generate a template as a comment near the compiler error. If you jump to the source code containing the error, you will see the template code.

For example, to declare a value object representing a **System.CultureInfo**. The syntax will be :

```csharp
	extern System.CultureInfo as CultureInfo;
```

You can now use the **CultureInfo** alias to declare a property type.

If you compile your project, the C# compiler will emit an error.
![](../img/CompilerError.png)

If you click on th error line, you jump to the source code containg a comment informing you how you can create the missing class.

![](../img/ValueObjectError.png)

See [Value object concepts](/Hyperstore/ValueObjects) to have more infos about value object in a Hyperstore context.


