---
layout : base
title  : Hyperstore Domain Language
---
## Referencing other domains

You can reference other domains in order to use theirs definitions by using the **uses** keyword.

```csharp
	uses "domain file path" as alias;
```

* 'domain file path' references an existing domain file. The path is relative to the current domain and must be a valid file path containg the full file name of the referencing domain including the file extension '.domain'.
* *alias* will be used to qualify element in the referenced domain. It must be a simple identifier without '.'.

When a domain is referenced you can used its elements by prefixing theirs names with the alias name.

For example, to reference an element call **Email** in a domain with alias called **std**, use **std.Email** syntax.

> When you reference a domain only its definition is visible, if this domain contains some **uses** statement, they will not be taken into account.

You can have many **uses** statement declarations.


