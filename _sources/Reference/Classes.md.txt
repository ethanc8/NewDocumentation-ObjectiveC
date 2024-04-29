% SPDX-License-Identifier: CC-BY-SA-3.0 OR GFDL-1.3-or-later
# Classes

Classes allow the programmer to define types which can be messaged, can create objects, and are polymorphic.

> **Note**: This document quotes from https://en.cppreference.com/w/c/language/struct, dual-licensed under the CC-BY-SA 3.0 and unversioned GFDL with no invariant sections, front-cover texts, or back-cover texts.

## Forward declaration

### Grammar summary

`@class` *name* (`,` *name*)\*<sub>opt</sub> `;`

### Grammar

objc-class-declaration:
: `@` `class` objc-class-forward-decl (`,` objc-class-forward-decl)* `;`

objc-class-forward-decl:
: identifier objc-type-parameter-list<sub>opt</sub>

objc-type-parameter-list:
: `<` objc-type-parameter (`,` objc-type-parameter)* `>`

objc-type-parameter:
: objc-type-parameter-variance<sub>opt</sub> identifier objc-type-parameter-bound<sub>opt</sub>

objc-type-parameter-bound:
: `:` type-name

objc-type-parameter-variance:
: `__covariant`
: `__contravariant`

### Discussion

This declares that a class with the name *name* exists. After this, the type *name* is now defined.

This works similary to forward-declaring a struct, but you cannot use the keyword `@class` outside of a forward-declaration.

> The following are **invalid**:
> ```objc
> id greet(@class NSGreeter aGreeter)
> ```
> ```objc
> @class NSGreeter myGreeter = [[NSGreeter alloc] init]
> ```

## Class interface

[ *attr-spec-seq* ]  
`@interface` *name* [ `<` *generic-param* `>` ] [ `:` *superclass-name* ] [ `<` *protocol-list* `>` ]   
[ `{` *ivar-declaration-list* `}` ]  
[ *method-declaration-list* ]  
`@end`

> ***attr-spec-seq*** (optional)  
> Attributes to add to the class

> ***name***  
> The name of the class being defined

> ***generic-param*** (optional)  
> A typename parameter.
> See [Lightweight Generics](Lightweight%20Generics.md)

> ***superclass-name*** (optional)  
> The superclass of the class being defined

> ***ivar-declaration-list***  
> A list of variables that each instance of the class will have in their storage.

> ***method-declaration-list***  
> A list of method declarations. See [&sect; Methods](#More) for more information.

## Class implementation

## Methods

## Class names in code

You can use the name of a class in code as either a type name followed by a `*`, or an object to send a message to.

GOOD:

```objc
NSString* myString = [[NSString alloc] init];
```

You cannot use the class name as an object of type `Class`:

BAD:
```objc
Class myClass = NSObject;
```

Instead, send the method `+class` to the object:

GOOD:
```objc
Class myClass = [NSObject class];
```
