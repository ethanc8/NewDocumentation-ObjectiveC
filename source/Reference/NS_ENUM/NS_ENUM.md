> **Note**: The definitions shown here are not always the actual definitons and therefore might not compile.

# `NS_ENUM` / `CF_ENUM`
Declares an enum.

## Declaration
```objc
#define NS_ENUM(...) CF_ENUM(__VA_ARGS__)
#define CF_ENUM(_type) /* ... */
#define CF_ENUM(_type, _name) /* ... */
```
## Parameters

> type name **`_type`**  
> The integer type used for the values of the enum.

> identifier **`_name`** *(optional)*  
> The name of the generated typedef.

## Equivalent attributes
[`__attribute__((enum_extensibility(open)))`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)
## Discussion

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

## Importing into Swift

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

## References

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  
[Clang documentation for `__attribute__(enum_extensibility)`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)

["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants)

[NSHipster article](https://nshipster.com/ns_enum-ns_options/)

## Documentation copyright

> **Note**: This page uses a modified version of the documentation found in the CoreFoundation header files, which are licensed under Apache License v2.0 with Runtime Library Exception.

> **Note**: This page uses part of Clang's documentation, which is licensed under Apache License v2.0 with LLVM Exceptions.

> **Note**: This page also quotes from Swift Evolution proposals, which are licensed under Apache License v2.0 with Runtime Library Exception.