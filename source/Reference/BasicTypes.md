# Basic Types

## Non-arithmetic types

### `id` and `nil`

A pointer to an instance of a class.

#### Declaration
Apple and GNUstep libobjc2:
```objc
typedef struct objc_object {
    Class isa; // deprecated
} * id;
#define nil ((id) 0)
```
GCC:
```objc
typedef struct objc_object {
    Class class_pointer;
} * id;
#define nil ((id) 0)
```

#### Usage

All objects are passed and returned by pointer. This means you can do:
```
id myObject = [[NSObject alloc] init];
```

> **Warning**: Do *not* access `isa` or `class_pointer` directly!
> 
> Doing so is deprecated, and on some runtimes (such as Apple's), the `isa` pointer is not actually a pointer, but rather a bitmask which stores other information about the object.
>
> Instead, use the functions `object_getClass` and `object_setClass` to access the class.

## Integer types

In addition to the types shown here, C has [C fixed-width integer types](https://en.cppreference.com/w/c/types/integer) and [C variable-width integer types](https://en.cppreference.com/w/c/language/arithmetic_types).

### `BOOL`: `YES` and `NO`

`BOOL` is a Boolean type.

#### Declaration
```
typedef /* implementation-defined */ BOOL;
#define YES ((BOOL) 1)
#define NO ((BOOL) 0)
```
#### Usage

`BOOL` is an integer type, capable of holding at least the values `YES` (`1`) and `NO` (`0`).

#### Implementation details

With the Apple runtime and `__OBJC_BOOL_IS_BOOL` defined to `1`, `BOOL` is a C99 `_Bool`.

On 64-bit iOS, `BOOL` is a C99 `_Bool`.

With the Apple runtime and `__OBJC_BOOL_IS_BOOL` defined to `0`, `BOOL` is a `signed char`.

On Mac OS X, `BOOL` is a `signed char`.

With the GCC runtime, `BOOL` is an `unsigned char`.

With the GNUstep runtime and `STRICT_APPLE_COMPATIBILITY` defined before including any Objective-C runtime header, `BOOL` is a `signed char`.

With the GNUstep runtime on Windows, `BOOL` is an `int`.

Otherwise, the GNUstep runtime defines `BOOL` as an `unsigned char`.

Recent versions of Clang support the constants `__objc_yes` and `__objc_no`, and it is possible that `YES` and `NO` are defined to these instead of their normal values.

### `NSInteger`

## Floating-point types

### `CGFloat`

## Constants

### `YES`

### `NO`

### `NULL`

### `nil`

### `Nil`

