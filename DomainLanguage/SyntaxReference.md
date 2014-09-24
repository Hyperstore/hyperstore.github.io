---
layout : base
title  : Hyperstore Domain Language Syntax Reference
---
## Syntax reference
Syntax reference uses an adapted [Backus Naur Form](http://en.wikipedia.org/wiki/Backus%E2%80%93Naur_Form) with the following conventions :

- `[]` means optional.
- Operators are enclosed by ''.
- Keywords are in **bold case**.
- | means alternative.
- Each BNF definition ends by ';'.
- `()*` multiple occurences allowed.
- `(..., 'x')*` multiple occurences separated by a 'x' character.
- (pname=type) means type with a property name.
- string are delimited by ".
***
HQL ::= `[Attributes]` **domain** (domainName=qualified_identifier)
`[`**extends** (extendedDomainPath=string)`]` '{' DomainBody '}';

Attributes ::= `'[' (name=identifier) [ '(' (parameters=string, ',')* ')' ]`;

DomainBody ::= UsingDeclarations | ExternDeclarations | MemberDeclarations;

UsingDeclarations ::= (**uses** (DomainReference=string) **as** (alias=identifier) ';')`*` ; 

ExternDeclarations ::= ( **extern** `[` **interface** | **enum** `]  (name=qualified_identifier) `[` **as** identifier `] ';')*`;

MemberDeclarations ::= (EnumDeclaration | ValueObjectDeclaration | EntityDeclaration | RelationshipDeclaration)`*`;

EnumDeclaration ::= `[Attributes]` **def enum** (name=identifier) '{' (values=identifier), ',')`*` '}' ';';

ValueObjectDeclaration ::= **def valueObject** (name=identifier) ':' (type=identifier) '{' ConstraintDeclarations '}';

EntityDeclaration ::= `[ Attributes ]` **def** `[` **partial** `]` **entity** (name=identifier)</br>`[` **extends** (superType=qualified_name) `] [`**implements** (interfaceNames=qualified_name, ',')`*]`</br>'{' PropertyDeclarations | ReferenceDeclarations | OppositeDeclarations | ConstraintDeclarations '}';

PropertyDeclarations ::= `[Attributes]` (name=identifier) ':' (type=qualified_name) `[= (defaultValue=csharpCode)]</br>[` ComputeConstraint | CheckOrValidateConstraint | ProjectionConstraint `]` ';' ;

ComputeConstraint ::= **compute** (code=csharpCode);

CheckOrValidateConstraint ::= (**check** | **validate**) (**error** | **warning**) (message=string) (code=csharpCode);

ProjectionConstraint ::= **where** (filter=csharpCode) ';' ;

ReferenceDeclarations ::= (name=identifier) `[ sourceCardinality='*' ]` ('->' | '=>') (target=qualified_name) `[targetCardinality='*']` ';' ;

OppositeDeclarations ::= (name=identifier) `[ targetCardinality='*' ]` ('<-' | '<=') (opposite=qualified_name) `[sourceCardinality='*' ]` ';' ;

ConstraintDeclarations ::= **constraints** ':' (CheckOrValidateConstraint)`*` ;

RelationshipDeclarations ::= `[Attributes]` **def** `[`**partial**`]` **relationship** (name=identifier)</br>'(' (source=identifier) `[sourceCardinality='*']` ('->' | '=>') (target=qualified_name) `[targetCardinality='*']` ')'</br>`[` **extends** (superType=qualified_name) `] [`**implements** (interfaceNames=qualified_name, ',')`*]`
</br>'{'PropertyDeclarations | ReferenceDeclarations | OppositeDeclarations | ConstraintDeclarations '}';

