% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception
# Enumerations

An enumeration is a distinct type whose value is restricted to a range of values (see below for details), which may include several explicitly named constants ("enumerators"). The values of the constants are values of an integral type known as the underlying type of the enumeration. 

Enumerations have the C++ and C23 behavior, where they may have underlying types. Unlike C++ and C23, and since Clang 3.0.0, the underlying type may be specified in a `typedef` or variable declaration:

```objc
typedef enum X: unsigned int X;
enum X: unsigned int {
    ...
};

enum X: unsigned int myX;
enum X: unsigned int* myY; // myY is of type (enum X)*
```

The [`NS_ENUM` convenience macros](NS_ENUM/index.md) use this to make these kinds of declarations easier:

```objc
NS_ENUM(X, unsigned int) {
    ...
};
```

is equivalent to:

```objc
typedef enum X: unsigned int X;
enum X: unsigned int {
    ...
};
```

## References
* <https://en.cppreference.com/w/cpp/language/enum>
* <https://en.cppreference.com/w/c/language/enum>
* <https://github.com/llvm/llvm-project/issues/48757>
* [N3030: Enhancements to Enumerations](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3030.htm#design-overflow)
* [draft C23 6.7.2.2](https://open-std.org/JTC1/SC22/WG14/www/docs/n3054.pdf#subsubsection.6.7.2.2)