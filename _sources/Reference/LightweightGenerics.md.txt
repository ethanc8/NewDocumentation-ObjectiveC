% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception
# Lightweight Generics

## Availability
### Compilers

"Objective-C 15 / 2.3"

* Clang >= 3.7

    * Apple Clang >= 7
    * Xcode >= 7

### Runtimes

No additional requirements.

## Overview

Generics allow you to define types that operate on other types. For example, `NSArray` uses generics so that you can create an array holding only one element -- for example you could create an array of cats.

## Using Generic Classes



## Compatibility Macros

Since GCC does not support lightweight generics, these macros are used in GNUstep to support lightweight generics.

These macros are provided by [`<Foundation/NSObjCRuntime.h>`](https://github.com/gnustep/libs-base/blob/9c129a1e7fb14d7fd352f45751a4c3379559bb97/Headers/Foundation/NSObjCRuntime.h#L208) on GNUstep. If you need to port to Cocoa, you can copy-paste these to a header in your code.

```objc
#if __has_feature(objc_generics)
# define GS_GENERIC_CLASS(clz, ...) clz<__VA_ARGS__>
# define GS_GENERIC_TYPE_F(typeRef, fallback) typeRef
#else
# define GS_GENERIC_CLASS(clz, ...) clz
# define GS_GENERIC_TYPE_F(typeRef, fallback) fallback
#endif
#define GS_GENERIC_TYPE(typeRef) GS_GENERIC_TYPE_F(typeRef, id)
```

| Compatibility macro                           | Meaning               | Meaning without generics |
| --------------------------------------------- | --------------------- | ------------------------ |
| `GS_GENERIC_CLASS(` *clz* `,` *typename...* `)`  | *clz* `<` *typename* `>` | *clz*                    |
| `GS_GENERIC_TYPE_F(` *typeRef* `,` *fallback* `)` | *typeRef*             | *fallback*               |
| `GS_GENERIC_TYPE(` *typeRef* `)`                | *typeRef*             | `id`                     |

## References

* [**Most in-depth article**](https://web.archive.org/web/20150805010143if_/http://drekka.ghost.io/objective-c-generics/)
* [Another article (bad grammar)](https://programmerall.com/article/1330578588/)
* [Explanation of `__covariant` and `__contravariant`](https://medium.com/ios-os-x-development/generics-in-objective-c-8f54c9cfbce7)
* [Covariance and contravariance (in other programming languages)](https://en.wikipedia.org/wiki/Covariance_and_contravariance_%28computer_science%29#Generic_types)
* [Using imported lightweight generics in Swift](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/using_imported_lightweight_generics_in_swift)
* [Introduction in cfe-dev (Clang mailing list) - most official](https://lists.llvm.org/pipermail/cfe-dev/2015-July/043821.html)