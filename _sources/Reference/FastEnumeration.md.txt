% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception
# Fast enumeration

## Availability
### Compilers

* Clang >= 2.0

    * Apple Clang >= 1.0
    * XCode >= 3.0

* GCC >= 4.6
* GCC forks:

    * Apple GCC 4.0 build >= 5493
    * Apple GCC 4.2 build >= 5577
    * LLVM-GCC 4.2 >= 5555

> **Note**: The feature might be available before the versions listed.

### SDKs / Targets

* Mac OS X >= 10.5 Leopard
* iOS >= 3.0 (first iOS version with an SDK)
* GNUstep Base Library >= 1.21.0

> **Important**: Fast enumeration will *not* work on operating systems whose collection classes do not conform to `NSFastEnumeration`.

## Feature description

New `for` loop syntax:

`for (` *init-clause* `in` *fast-enumerator* `)` *loop-statement*

*init-clause* is an expression or declaration.  
*fast-enumerator* is the object which is being iterated over which conforms to the `NSFastEnumeration` protocol.

For each object in *fast-enumerator*, the object is set equal to the object in *init-clause*.

An exception is raised if *fast-enumerator* is modified.

Otherwise, behavior is identical to a C for loop.

## References

[GCC documentation](https://gcc.gnu.org/onlinedocs/gcc/Fast-enumeration.html)

[*The Objective-C Programming Language*, "Fast Enumeration"](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocFastEnumeration.html)

[*Programming with Objective-C*, "Fast Enumeration..."](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/FoundationTypesandCollections/FoundationTypesandCollections.html#//apple_ref/doc/uid/TP40011210-CH7-SW30)

[Cppreference.com -- description of C for loops](https://en.cppreference.com/w/c/language/for)

[NSHipster article](https://nshipster.com/enumerators/)

## `NSFastEnumeration` protocol

```objc
typedef struct {
    unsigned long state;
    id *itemsPtr;
    unsigned long *mutationsPtr;
    unsigned long extra[5];
} NSFastEnumerationState;

@protocol NSFastEnumeration
    - (NSUInteger) countByEnumeratingWithState: (NSFastEnumerationState*) state
                                       objects: (id*) stackbuf
                                         count: (NSUInteger) len
@end
```

For more information, see [Mike Ash's blog post on implementing NSFastEnumeration](https://www.mikeash.com/pyblog/friday-qa-2010-04-16-implementing-fast-enumeration.html).

### References

[Apple documentation](https://developer.apple.com/documentation/foundation/nsfastenumeration?language=objc)

[GCC documentation](https://gcc.gnu.org/onlinedocs/gcc/Fast-enumeration-protocol.html)

[Mike Ash's blog post on implementing NSFastEnumeration](https://www.mikeash.com/pyblog/friday-qa-2010-04-16-implementing-fast-enumeration.html)