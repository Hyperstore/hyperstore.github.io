---
layout : base
title  : Hyperstore Domain Language
---
## Hyperstore Domain Language

Hyperstore Domain Language (HDL) is a textual DSL (Domain Specific Language) allowing to define an Hyperstore Domain in a '.domain' file.

When a .domain file is included in a *Visual Studio Project* and if the **Hyperstore Domain Language Editor** nuget package is added to the project, it will be translated into C# code which will be integrated into the C# compilation process.

> If the project contains many .domain files, they will be aggregated into one unique C# generated code file.

A .domain file can be edited with a specific Visual Studio extension providing syntax highlighting, colorization and completion available on [Visual Studio Gallery](http://visualstudiogallery.msdn.microsoft.com/7243e6ca-e7bd-44a6-92a5-50b0083f6287).


