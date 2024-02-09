# Language Versions

> **Note**: Versions before Stepstone 4.0 are missing.

> **Note**: In order to solve the confusing versioning scheme of Objective-C, this document uses its own versioning scheme which is similar to the current, extremely vague scheme.



## References

[Compiler release notes for NeXT Releases 3.0 to 3.3](http://www.cilinder.be/docs/next/NeXTStep/3.3/nd/ReleaseNotes/Compiler.htmld/index.html)

[Assembler, linker, etc release notes for Rhapsody 5.0 to Xcode 3.2.1](https://opensource.apple.com/source/cctools/cctools-845/RelNotes/CompilerTools.html)

[Objective-C Release Notes (Mac OS X Leopard)](https://developer.apple.com/library/archive/releasenotes/Cocoa/RN-ObjectiveC/index.html)

[*spec.language*, a Stepstone-internal document detailing planned user-visible changes to Stepstone Objective-C 4.0](https://dl.acm.org/doi/pdf/10.1145/3386332#page=53)

[Objective-C Feature Availability Index](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/ObjCAvailabilityIndex/index.html)

[What's new in Xcode 4](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/WhatsNewXcode/Chapters/xcode_4_0.html#//apple_ref/doc/uid/TP40016147-SW1)

(v0.8)=
## v0.8 - Lowest Common Denominator

### Availability

#### Compilers

* Stepstone >= 4.0
* Portable Object Compiler
* NeXT GCC fork
* GCC since around 1992
* Clang

#### Runtimes

* Stepstone >= 4.0
* Portable Object Compiler
* NeXT/Apple with 32-bit ABI
* Apple with modern ABI
* GCC runtime
* GNUstep runtime

### Available features

* `#import` preprocessor directive
* `@interface`...`@end` class declarations
* `@implementation`...`@end` class definitions

(v0.9)=
## v0.9 - Categories

### Availability
#### Compilers

* NeXT GCC fork (was available sometime before 3.0)
* GCC (all versions with documentation on the GCC website)
* Clang
* Portable Object Compiler

#### Runtimes

* NeXT/Apple with 32-bit ABI
* Apple with modern ABI
* GCC runtime
* GNUstep runtime
* Portable Object Compiler

### New features since v0.8

* **Major feature: Categories**

(v0.10)=
## v0.10 - NeXT Release 3.0

This release introduces some C99 features.

### Availability

#### Compilers

* NeXT GCC fork >= 3.0 (based on GCC 1.93)
* GCC >= 2.0
* Clang

#### Runtimes

* NeXT/Apple with 32-bit ABI
* Apple with modern ABI
* GCC runtime
* GNUstep runtime

### Major new features

* Protocols
    * `@protocol`...`@end`
    * `<`...`>` syntax for declaring classes
    * New qualifiers for types that can only be used in protocol declarations: `in`, `out`, `inout`, `bycopy`, `oneway`.
* Interface variable access control
    * `@public` variables are accessible by anyone
    * `@protected` variables are accessible by defining class and subclasses
    * `@private` variables are only accessible by the defining class
    * default is `@protected`

### Changes since v0.9

* Forward declaration of classes (`@class`)
* `#warning` directive to complement `#error`
* `__attribute__`:
    * `aligned`
    * `format`
* New `@encode` encodings for `in`, `out`, `inout`, `bycopy`, `oneway`, `const`.

(v1.0)=
## v1.0 - Exceptions and synchronization

### Availability

* GCC >= 4.0 with `-fobjc-exceptions` command line argument
* Clang

### New features

* Exceptions
* Synchronization

### Notes

* On GCC, you can use the `fobjc-std=objc1` to disable Objective-C 2.0.

### References

* [GCC Standards - Objective-C and Objective-C++](https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/Standards.html#Objective-C-and-Objective-C_002b_002b-Languages)

(v2.0)=
## v2.0 - 2007 - Leopard

### Availability

#### Compilers

* Xcode >= 3.0
* GCC >= 4.6
* Clang >= 1.0

#### Runtimes

* Apple runtime >= (10.5 Leopard)
* GCC runtime >= GCC 4.6
* Etoile runtime
* GCC runtime with `ObjectiveC2.framework`
* GNUstep runtime (all versions)

### Major new features

* Garbage collection
    * Didn't really take off, was replaced by Automatic Reference Counting
    * Not supported anywhere anymore
    * Different from GNUstep garbage collection
* Properties `@property`
    * Automatic getter and setter generation `@synthesize`
    * Dot notation `object.count` instead of `[object count]` and `[object setCount: ]`
* Non-fragile ivars (requires runtime support)
* Fast enumeration `for (id thing in myList)`
* properties (via the @property, @synthesize and @dynamic keywords), fast enumeration (not available in Objective-C++), attributes for methods (such as deprecated, noreturn, sentinel, format), the unused attribute for method arguments, the @package keyword for instance variables and the @optional and @required keywords in protocols

### Resources
* [Wikipedia article](https://en.wikipedia.org/wiki/Objective-C#Objective-C_2.0)
* [Xcode 3.0 Release Notes](https://web.archive.org/web/20080517213700if_/http://developer.apple.com/releasenotes/DeveloperTools/RN-Xcode/index.html#//apple_ref/doc/uid/TP40001051-DontLinkElementID_2)
    * Objective-C has been enhanced with support for properties, fast enumeration, optional methods in protocols, and garbage collection.
    * [What's New in Xcode 3.0 (PDF)](https://web.archive.org/web/20080303091141if_/http://developer.apple.com/documentation/DeveloperTools/Conceptual/WhatsNewXcode/WhatsNewXcode.pdf)
      * doesn't seem relevant
* [Objective-C 2.0 Release Notes](https://developer.apple.com/library/archive/releasenotes/Cocoa/RN-ObjectiveC/index.html#//apple_ref/doc/uid/TP40004309)
* [Xcode 3.0 Advertisement](https://web.archive.org/web/20070629103804if_/http://www.apple.com/macosx/leopard/developer/xcode.html)
    * Enjoy modern garbage collection, syntax enhancements, runtime performance improvements, and 64-bit support.
* [GCC 4.6 Release Notes](https://gcc.gnu.org/gcc-4.6/changes.html#objective-c)



(v2.1)=
## v2.1 - 2011 - ARC, Xcode 4.2

### Availability

#### Compilers
* Xcode >= 4.2
* Clang (upstream) >= 3.0
    * Apple LLVM Compiler >= 3.0

#### Runtimes
* Apple runtime >= (Mac OS X 10.7 / iOS 5)
* GNUstep runtime >= 1.6

### Major new features

ARC, `@autoreleasepool`, completely private ivars

* Automatic Reference Counting (ARC) and an improved memory model cleanly separating object and C memory.
* A migration tool for moving manual retain/release code to ARC
* Better support for data hiding, allowing instance variables to be declared in implementation contexts or class extensions
* Weak linking support for Objective-C classes
* Improved static type checking by inferring the return type of methods such as `+alloc` and `-init`.

### Resources

* [Clang 3.0 release notes](https://releases.llvm.org/3.0/docs/ClangReleaseNotes.html#objchanges)
* [LLVM 3.0 release notes](https://releases.llvm.org/3.0/docs/ReleaseNotes.html#clang)

(v2.1.1)=
## v2.1.1 - 2012 - Xcode 4.3

(v2.2)=
## v2.2 - 2012 - Literals, Xcode 4.4

* Literals syntax is supported for NSArray, NSDictionary, and NSNumber objects, using the same `@` operator as for NSString literals.
* Subscripting is enabled for Objective-C containers, including NSDictionary and NSArray. Use the `[ ]` syntax convention.
* Objective-C `@properties` are synthesized by default when not explicitly implemented.

(v2.3)=
## v2.3 - Xcode 6.3

### Availability

#### Compilers

* Xcode >= 6.3
* Clang >= 3.6.0
  * Apple LLVM Compiler >= 6.1.0, clang-602.0.49

#### Runtime

The new features require no runtime support.

### New features

* Nullability specifiers
  * [Clang documentation](https://clang.llvm.org/docs/AttributeReference.html#nullability-attributes)

### References

* [Xcode 6.3 release notes](https://developer.apple.com/library/archive/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html#//apple_ref/doc/uid/TP40001051-CH1-SW396)

(v2.3.1)=
## v2.3.1 - 2015 - Xcode 7.0

### Availability

#### Compilers

* Xcode >= 7.0
* Clang >=

#### Runtime

The new features require no runtime support.

### New features

* Nullability specifiers are now `_Nullable`, `_Nonnull`, and `_Null_unspecified` instead of `__nullable`, `__nonnull`, and `__null_unspecified`
* Lightweight generics
* Kindof types

### References

* [Xcode 7.0 release notes - Objective-C language changes](https://developer.apple.com/library/archive/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html#//apple_ref/doc/uid/TP40001051-CH1-SW343)

## Unknown 2.x

https://developer.apple.com/library/archive/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html

* instancetype
* Enumeration macros
* Designated initializers
* 
