% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception AND CC-BY-SA 3.0
<!-- SPDX-License-Identifier: Apache-2.0 AND CC-BY-SA 3.0 
     Derived from Clang source and https://devtut.github.io/objectivec/properties.html -->


# Properties

## Syntax

### Property declaration and implementation
- @property (**[optional_attributes, ...](#attributes)**) **type** **identifier**;
- @synthesize **identifier** = **optional_backing_ivar**;
- @dynamic **identifier**;

### Dot syntax

Given an object **myObject** which has a property named **size**, you can:

| Dot syntax               | Traditional syntax            | Description                                     |
| ----------------------------- | ----------------------------- | ----------------------------------------------- |
| **myObject**.**size**         | [**myObject** **size**]       | Gets the value of the **size** of **myObject**. |
| **myObject**.**size** = **3** | [**myObject** set**Size**: 3] | Sets the **size** of **myObject** to **3**. |

The compiler translates the dot syntax to a method call.

## Description

A property represents a piece of state kept by an object, that is accessed using getters and setters. `@property` creates a **declared property**, which allows you to specify [various attributes](#attributes) about the property. However, the dot syntax is available even for undeclared properties, or even methods that aren't properties. All you need to read a property is a method with the name of the property, and to write, you just need a method named `set` + property name + `:`.

You can **synthesize** a property from an ivar using `@synthesize`. If you do not specify an ivar, it will automatically use an ivar with the same name as the property. If you are on a modern runtime with a non-fragile ABI (the Apple 64-bit runtime or GNUstep's `libobjc2`), then `@synthesize` will create the ivar for you if it is not already declared.

Instead of synthesizing the property from the ivar, you can write a getter and a setter method. Just implement the methods that the dot syntax converts to.

`@dynamic` tells the compiler that even though you didn't implement the property, some kind of dynamic mechanism that the compiler doesn't know about, such as message forwarding, swizzling, or categories in other files, will implement the property's required methods. Only use this keyword if you know that the methods will be available before somebody tries to access the properties.

(attributes)=
### Property attributes

2.0 are supported by GCC >= 4.6, Clang >= 1.0, and Xcode >= 3.0. 2.1 attributes are supported by Clang >= 3.0 and Xcode >= 4.2.

The following attributes are equivalent:

| 2.0      | 2.1                 |
| -------- | ------------------- |
| `assign` | `unsafe_unretained` |
| `retain` | `strong`            |

If you need to support GCC or older versions of Clang, use the 2.0 attribute names instead. However, if your code uses ARC, use the 2.1 attribute names as they are designed to match the ARC keywords.

|Attribute|Description|Since
|---|---|---
|`atomic`|**Implicit.** Enables synchronization in synthesized accessor methods.|2.0
|`nonatomic`|Disables synchronization in the synthesized accessor methods.|2.0
|`readwrite`|**Implicit.** Synthesizes getter, setter and backing ivar.|2.0
|`readonly`|Synthesizes only the getter method and backing ivar, which can be assigned directly.|2.0
|`getter=`**name**|Specifies the name of getter method, implicit is `propertyName`.|2.0
|`setter=`**name**|Specifies the name of setter method, implicity is `setPropertyName:`. Colon `:` must be a part of the name.|2.0
|`strong`|**Implicit for objects under ARC**. The backing ivar is synthesized using `__strong`, which prevents deallocation of referenced object.|**2.1**
|`retain`|Synonym for `strong`.|2.0
|`copy`|Same as `strong`, but the synthesized setter also calls `-copy` on the new value.|2.0
|`unsafe_unretained`|**Implicit, except for objects under ARC.** The backing ivar is synthesized using `__unsafe_unretained`, which (for obejcts) results in dangling pointer once the referenced object deallocates.|**2.1**
|`assign`|Synonym for `unsafe_unretained`. Suitable for non-object types.|2.0
|`weak`|Backing ivar is synthesized using `__weak`, so the value will be nullified once the referenced object is deallocated.|**2.1, ARC only**
|`class`|Property accessors are synthesized as class methods, instead of instance methods. No backing storage is synthesized.|???
|`nullable`|The property accepts `nil` values. Mainly used for Swift bridging.|???
|`nonnull`|The property doesn’t accept `nil` values. Mainly used for Swift bridging.|???
|`null_resettable`|The property accepts `nil` values  in setter, but never returns `nil` values from getter. Your custom implementation of getter or setter must ensure this behavior. Mainly used for Swift bridging.|???
|`null_unspecified`|**Implicit.** The property doesn’t specify handling of `nil` values. Mainly used for Swift bridging.|???

## Discussion

### GNUstep style

When declaring a property when contributing to GNUsteps, please define the getter and setter and use compilation guards to detect if the compiler supports properties. GNUstep needs to still work on very old versions of GCC.

**Header file:**

```objc
{ // ivars
    NSUserInterfaceLayoutDirection _userInterfaceLayoutDirection;
}

#if GS_HAS_DECLARED_PROPERTIES
@property NSUserInterfaceLayoutDirection userInterfaceLayoutDirection;
#else
- (NSUserInterfaceLayoutDirection) userInterfaceLayoutDirection;
- (void) setUserInterfaceLayoutDirection: (NSUserInterfaceLayoutDirection)dir;
#endif
```

**Implementation:**

```objc
- (NSUserInterfaceLayoutDirection) userInterfaceLayoutDirection
{
  return _userInterfaceLayoutDirection;
}

- (void) setUserInterfaceLayoutDirection: (NSUserInterfaceLayoutDirection)dir
{
  ASSIGN(_userInterfaceLayoutDirection, dir);
}
```

### Custom getters and setters


The default property getters and setters can be overridden:

```objectivec
@interface TestClass

@property NSString *someString;

@end

@implementation TestClass

// override the setter to print a message
- (void)setSomeString:(NSString *)newString {
    NSLog(@"Setting someString to %@", newString);
    // Make sure to access the ivar (default is the property name with a _ 
    // at the beginning) because calling self.someString would call the same
    // method again leading to an infinite recursion
    _someString = newString;
}

- (void)doSomething {
    // The next line will call the setSomeString: method
    self.someString = @"Test";
}

@end

```

This can be useful to provide, for example, lazy initialization (by overriding the getter to set the initial value if it has not yet been set):

```objectivec
- (NSString *)someString {
    if (_someString == nil) {
        _someString = [self getInitialValueForSomeString];
    }
    return _someString;
}

```

You can also make a property that computes its value in the getter:

```objectivec
@interface Circle : NSObject

@property CGPoint origin;
@property CGFloat radius;
@property (readonly) CGFloat area;

@end

@implementation Circle

- (CGFloat)area {
    return M_PI * pow(self.radius, 2);
}

@end

```



### What are properties?


Here is an example class which has a couple of instance variables, without using properties:

```objectivec
@interface TestClass : NSObject {
    NSString *_someString;
    int _someInt;
}

-(NSString *)someString;
-(void)setSomeString:(NSString *)newString;

-(int)someInt;
-(void)setSomeInt:(NSString *)newInt;

@end


@implementation TestClass

-(NSString *)someString {
    return _someString;
}

-(void)setSomeString:(NSString *)newString {
    _someString = newString;
}

-(int)someInt {
    return _someInt;
}

-(void)setSomeInt:(int)newInt {
    _someInt = newInt;
}

@end

```

This is quite a lot of boilerplate code to create a simple instance variable.  You have to create the instance variable & create accessor methods which do nothing except set or return the instance variable.  So with Objective-C 2.0, Apple introduced properties, which auto-generate some or all of the boilerplate code.

Here is the above class rewritten with properties:

```objectivec
@interface TestClass

@property NSString *someString;
@property int someInt;

@end


@implementation testClass

@end

```

A property is an instance variable paired with auto-generated getters and setters.  For a property called `someString`, the getter and setter are called `someString` and `setSomeString:` respectively.  The name of the instance variable is, by default, the name of the property prefixed with an underscore (so the instance variable for `someString` is called `_someString`, but this can be overridden with an `@synthesize` directive in the `@implementation` section:

```objectivec
@synthesize someString=foo;    //names the instance variable "foo"
@synthesize someString;    //names it "someString"
@synthesize someString=_someString;        //names it "_someString"; the default if 
                                           //there is no @synthesize directive

```

Properties can be accessed by calling the getters and setters:

```objectivec
[testObject setSomeString:@"Foo"];
NSLog(@"someInt is %d", [testObject someInt]);

```

They can also be accessed using dot notation:

```objectivec
testObject.someString = @"Foo";
NSLog(@"someInt is %d", testObject.someInt);

```



### Properties that cause updates


This object, `Shape` has a property `image` that depends on `numberOfSides` and `sideWidth`. If either one of them is set, than the `image` has to be recalculated. But recalculation is presumably long, and only needs to be done once if both properties are set, so the `Shape` provides a way to set both properties and only recalculate once. This is done by setting the property ivars directly.

In `Shape.h`

```objectivec
@interface Shape {
    NSUInteger numberOfSides;
    CGFloat sideWidth;

    UIImage * image;
}

// Initializer that takes initial values for the properties.
- (instancetype)initWithNumberOfSides:(NSUInteger)numberOfSides withWidth:(CGFloat)width;

// Method that allows to set both properties in once call.
// This is useful if setting these properties has expensive side-effects.
// Using a method to set both values at once allows you to have the side-
// effect executed only once.
- (void)setNumberOfSides:(NSUInteger)numberOfSides andWidth:(CGFloat)width;

// Properties using default attributes.
@property NSUInteger numberOfSides;
@property CGFloat sideWidth;

// Property using explicit attributes.
@property(strong, readonly) UIImage * image;

@end

```

In `Shape.m`

```objectivec
@implementation AnObject

// The variable name of a property that is auto-generated by the compiler
// defaults to being the property name prefixed with an underscore, for
// example "_propertyName". You can change this default variable name using
// the following statement:
// @synthesize propertyName = customVariableName;

- (id)initWithNumberOfSides:(NSUInteger)numberOfSides withWidth:(CGFloat)width {
    if ((self = [self init])) {
       [self setNumberOfSides:numberOfSides andWidth:width];
    }

    return self;
}

- (void)setNumberOfSides:(NSUInteger)numberOfSides {
    _numberOfSides = numberOfSides;

    [self updateImage];
}

- (void)setSideWidth:(CGFloat)sideWidth {
    _sideWidth = sideWidth;

    [self updateImage];
}

- (void)setNumberOfSides:(NSUInteger)numberOfSides andWidth:(CGFloat)sideWidth {
    _numberOfSides = numberOfSides;
    _sideWidth = sideWidth;

    [self updateImage];
}

// Method that does some post-processing once either of the properties has
// been updated.
- (void)updateImage {
    ...
}

@end

```

When properties are assigned to (using `object.property = value`), the setter method `setProperty:` is called. This setter, even if provided by `@synthesize`, can be overridden, as it is in this case for `numberOfSides` and `sideWidth`. However, if you set an property's ivar directly (through `property` if the object is self, or `object->property`), it doesn't call the getter or setter, allowing you to do things like multiple property sets that only call one update or bypass side-effects caused by the setter.




## Grammar (from Clang source)

```
///   property-attr-decl: '(' property-attrlist ')'
///   property-attrlist:
///     property-attribute
///     property-attrlist ',' property-attribute
///   property-attribute:
///     getter '=' identifier
///     setter '=' identifier ':'
///     direct
///     readonly
///     readwrite
///     assign
///     retain
///     copy
///     nonatomic
///     atomic
///     strong
///     weak
///     unsafe_unretained
///     nonnull
///     nullable
///     null_unspecified
///     null_resettable
///     class

///   property-synthesis:
///     @synthesize property-ivar-list ';'
///
///   property-ivar-list:
///     property-ivar
///     property-ivar-list ',' property-ivar
///
///   property-ivar:
///     identifier
///     identifier '=' identifier

///   property-dynamic:
///     @dynamic  property-list
///
///   property-list:
///     identifier
///     property-list ',' identifier
```

## Further reading

* [Declared Properties - The Objective-C Programming Language](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocProperties.html#//apple_ref/doc/uid/TP30001163-CH17-SW1) -- very similar to a reference manual, deprecated
* [Properties Encapsulate An Object's Values - Programming with Objective-C](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html#//apple_ref/doc/uid/TP40011210-CH5-SW2) - more of a tutorial