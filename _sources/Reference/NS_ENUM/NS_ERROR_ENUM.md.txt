> **Note**: The definitions shown here are not always the actual definitons and therefore might not compile.

# `NS_ERROR_ENUM` / `CF_ERROR_ENUM`
Declares an error domain.

## Availability (SDKs)

* Mac OS X >= 10.13 High Sierra
* swift-corelibs-foundation >= 4.1

## Declaration
```objc
#define NS_ERROR_ENUM(...) CF_ENUM(__VA_ARGS__)
#define CF_ERROR_ENUM(_domain) /* ... */
#define CF_ERROR_ENUM(_domain, _name) /* ... */
```
## Parameters

> `NSString* `**`_domain`**  
> The name of the error domain.

> identifier **`_name`** *(optional)*  
> The name of the generated typedef.

## Equivalent attributes
```objc
__attribute__((ns_error_domain(_domain)))
```
where `_domain` is one of the paramaters.
## Discussion

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

## Importing into Swift


## References

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  

## Documentation copyright

> **Note**: This page uses a modified version of the documentation found in the CoreFoundation header files, which are licensed under Apache License v2.0 with Runtime Library Exception.

> **Note**: This page also uses a modified version of the documentation found in the Apple Foundation header files, which are proprietary.

> **Note**: This page also quotes from ["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants), which is proprietary.

> **Note**: This page also quotes from Swift Evolution proposals, which are licensed under Apache License v2.0 with Runtime Library Exception.