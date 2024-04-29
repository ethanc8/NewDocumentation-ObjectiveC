% SPDX-License-Identifier: CC-BY-SA-3.0 OR GFDL-1.3-or-later
# Attributes

## Attribute Syntax
> This section quotes from [Attribute specifier sequence (since C23)](https://en.cppreference.com/w/c/language/attributes)
### GCC attribute syntax

Introduces implementation-defined attributes for types, objects, expressions, etc.

`__attribute__((`*attr*`))` `__attribute__((`*attr1*`,` *attr2*`,` *attr3*`(`*args*`) ))`

Formally, the syntax is

`__attribute__((`*attribute-list*`))`

where *attribute-list* is a comma-separated sequence of zero or more *attribute-tokens*:

|                                             |     |
| ------------------------------------------- | --- |
| *attribute-name*                            | (1) |
| `__`*attribute-name*`__`                    | (2) |
| *attribute-name*`(`*argument-list*`)`       | (3) |
| `__`*attribute-name*`__(`*argument-list*`)` | (4) |

| | |
| --- | --- |
| 1, 2) | GCC or Clang attribute, such as `__attribute__((fallthrough))`
| 3, 4) | GCC or Clang attribute with arguments, such as `__attribute((deprecated("reason")))`

> **Note**: In this reference, GCC attributes are denoted by *gcc-attr-spec-seq* and C23 attributes are denoted by *c23-attr-spec-seq*

> **See also**: https://gcc.gnu.org/onlinedocs/gcc/Attribute-Syntax.html#Attribute-Syntax

### C23 attribute syntax

See https://en.cppreference.com/w/c/language/attributes.

## Function Attributes
### Syntax and placement
### List

## Variable Attributes
### Syntax and placement
### List



## Declaration attributes
### Syntax and placement
### List

## Label attributes
### Syntax and placement

### List

## Statement attributes
### Syntax and placement
### LIst

## Type declaration attributes
### Syntax and placement
### List

## `struct` declaration attributes
### Syntax and placement
### List

## `enum` declaration attributes
### Syntax and placement
### List

## Enumerator (within `enum` definition) attributes
### Syntax and placement
### List

## `union` declaration attributes
### Syntax and placement
### List


## Class declaration attributes
### Syntax and placement
### List
| GCC name | C23 name | GCC doc | Clang doc | Other |
| --- | --- | --- | --- | --- |
| `objc_root_class` | | [Yes](https://gcc.gnu.org/onlinedocs/gcc/Common-Type-Attributes.html#index-objc_005froot_005fclass-type-attribute) | Unknown | |

## References

[C23 attribute syntax](https://en.cppreference.com/w/c/language/attributes)

[GCC `__attribute__` syntax](https://gcc.gnu.org/onlinedocs/gcc/Attribute-Syntax.html#Attribute-Syntax)

[Lists of GCC attributes](https://gcc.gnu.org/onlinedocs/gcc/index.html#toc-Declaring-Attributes-of-Functions)

[List of Clang attributes](https://clang.llvm.org/docs/AttributeReference.html)