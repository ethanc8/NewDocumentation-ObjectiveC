# `NS_ENUM`, `NS_OPTIONS`, and similar macros

## Availability

> **Important**: You must define `NS_ENUM`, `NS_OPTIONS`, etc. in your code, or import the correct header files.

### Required headers

> **Note**: The header file names are links to mirrors of the header files on GitHub.

To get `NS_ENUM` and others using an Apple SDK:

[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h) which is included in [`<Foundation/Foundation.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/Foundation.h)

To get `CF_ENUM` and others using CoreFoundation:

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)

## Feature description

> **Note**: The definitions shown here are not always the actual definitons and therefore might not compile.

> **Note**: This page uses a modified version of the documentation found in the CoreFoundation header files, which are licensed under Apache License v2.0 with Runtime Library Exception.

> **Note**: This page also uses a modified version of the documentation found in the Apple Foundation header files, which are proprietary.

> **Note**: This page also quotes from ["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants), which is proprietary.

> **Note**: This page also quotes from Swift Evolution proposals, which are licensed under Apache License v2.0 with Runtime Library Exception.

## `NS_ENUM` / `CF_ENUM`
Declares an enum.

### Declaration
```objc
#define NS_ENUM(...) CF_ENUM(__VA_ARGS__)
#define CF_ENUM(_type) /* ... */
#define CF_ENUM(_type, _name) /* ... */
```
### Parameters

> type name **`_type`**  
> The integer type used for the values of the enum.

> identifier **`_name`** *(optional)*  
> The name of the generated typedef.

### Equivalent attributes
[`__attribute__((enum_extensibility(open)))`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)
### Discussion

`CF_ENUM` and `NS_ENUM` support the use of one or two arguments. The first argument (`_type`) is always the integer type used for the values of the enum. The second argument (`_name`) is an optional type name for the macro.

When specifying a type name, you must precede the macro with `typedef`. This will result in a typename being produced with the type `_type` and the name `_name`. For example:
```objc
typedef NS_ENUM(NSInteger, NSComparisonResult) {
    ...
};
```
will produce a typedef:
```objc
typedef NSInteger NSComparisonResult;
```
If you do not specify a type name, do not use `typedef` before the macro. For example:
```objc
NS_ENUM(NSInteger) {
    ...
};
```
will not produce a type name.

The macro adds compiler attributes and the underlying integer type to the enum declaration when using a recent version of Clang.

### Importing into Swift

Use the `NS_ENUM` macro for simple groups of constants.

The example below uses the macro to declare a `UITableViewCellStyle `enumeration that groups several different view styles for table views:
```objc
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,
    UITableViewCellStyleValue1,
    UITableViewCellStyleValue2,
    UITableViewCellStyleSubtitle
};
```
In Swift, the UITableViewCellStyle enumeration is imported like this:
```swift
enum UITableViewCellStyle: Int {
    case `default`
    case value1
    case value2
    case subtitle
}
```
Enumerations imported using the `NS_ENUM` macro won't fail when you initialize one with a raw value that does not correspond to an enumeration case. This characteristic facilitates compatibility with C, which allows any value to be stored in an enumeration, including values used internally but not exposed in headers.

The `NS_ENUM` macro is the only enumeration macro that results in an actual enumeration type when imported to Swift. The other enumeration macros generate structures.

### References

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  
[Clang documentation for `__attribute__(enum_extensibility)`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)

["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants)

[NSHipster article](https://nshipster.com/ns_enum-ns_options/)

## `NS_CLOSED_ENUM` / `CF_CLOSED_ENUM`
Declares a frozen enum.

### Declaration
```objc
#define NS_CLOSED_ENUM(_type, _name) CF_CLOSED_ENUM(_type, _name
#define CF_CLOSED_ENUM(_type, _name) /* ... */
```
### Parameters

> type name **`_type`**  
> The integer type used for the values of the enum.

> identifier **`_name`**  
> The name of the generated typedef.

### Equivalent attributes
[`__attribute__((enum_extensibility(closed)))`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)
### Discussion

Use the `NS_CLOSED_ENUM` or `CF_CLOSED_ENUM` macro for a simple group of constants that you can never add new cases to. Closed enumerations are useful for representing a finite set of states that you expect people to switch over using a switch statement. Once an enum is marked as closed, it is a binary and source incompatible change to add a new value.

The three cases of `NSComparisonResult`—`NSOrderedAscending`, `NSOrderedSame`, and `NSOrderedDescending`—are an example of a finite set. They're the only logical cases for performing an ordered comparison during tasks like sorting.

Don't use the `NS_CLOSED_ENUM` or `CF_CLOSED_ENUM` macro if:

* You've ever added cases to an enumeration after its initial declaration

* You can think of additional cases you might add later

* The enumeration has any private cases

In these scenarios, use the `NS_ENUM` or `CF_ENUM` macro instead.

### Importing into Swift

* Swift >= 5

When used in a `switch` statement whose last case is `@unknown default:`, the Swift compiler will emit a warning if not all cases of a frozen enum have not been specified. For example:

```objc
typedef NS_CLOSED_ENUM(NSInteger, GregorianMonth) {
  GregorianMonthJanuary = 1,
  GregorianMonthFebruary,
  GregorianMonthMarch,
  GregorianMonthApril,
  GregorianMonthMay,
  GregorianMonthJune,
  GregorianMonthJuly,
  GregorianMonthAugust,
  GregorianMonthSeptember,
  GregorianMonthOctober,
  GregorianMonthNovember,
  GregorianMonthDecember
}
```

Can be switched on. For example:

```swift
var month: GregorianMonth = /* ... */
switch month {
    case .january: break
    @unknown default: break
}
```

will cause a compiler warning because not all cases of the closed enum have been specified.

### References

[Swift Evolution Proposal SE-0192 "Handling Future Enum Cases"](https://github.com/apple/swift-evolution/blob/master/proposals/0192-non-exhaustive-enums.md)

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  
[Clang documentation for `__attribute__(enum_extensibility)`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)

["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants)

[Article on SwiftjectiveC.com](https://www.swiftjectivec.com/ns_closed_enum/)

## `NS_ERROR_ENUM` / `CF_ERROR_ENUM`
Declares an error domain.

### Availability (SDKs)

* Mac OS X >= 10.13 High Sierra
* swift-corelibs-foundation >= 4.1

### Declaration
```objc
#define NS_ERROR_ENUM(...) CF_ENUM(__VA_ARGS__)
#define CF_ERROR_ENUM(_domain) /* ... */
#define CF_ERROR_ENUM(_domain, _name) /* ... */
```
### Parameters

> `NSString* `**`_domain`**  
> The name of the error domain.

> identifier **`_name`** *(optional)*  
> The name of the generated typedef.

### Equivalent attributes
```objc
__attribute__((ns_error_domain(_domain)))
```
where `_domain` is one of the paramaters.
### Discussion

`CF_ERROR_ENUM` and `NS_ERROR_ENUM` support the use of one or two arguments. The first argument (`_domain`) is always the domain specifier for the enum. The second argument (`_name`) is an optional type name for the macro.

When specifying a type name, you must precede the macro with `typedef`. This will result in a typename being produced with the type `NSInteger` or `CFIndex` and the name `_name`. For example:
```objc
typedef NS_ERROR_ENUM(MyErrorDomain, SomeErrorCodes) {
    ...
};
```
will produce a typedef:
```objc
typedef NSInteger SomeErrorCodes;
```
If you do not specify a type name, do not use `typedef` before the macro. For example:
```objc
NS_ENUM(NSInteger) {
    ...
};
```
will not produce a type name.

The macro adds compiler attributes and the underlying integer type to the enum declaration when using a recent version of Clang.

### Importing into Swift


### References

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  
