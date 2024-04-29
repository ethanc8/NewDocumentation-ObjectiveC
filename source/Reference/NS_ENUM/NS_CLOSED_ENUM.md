> **Note**: The definitions shown here are not always the actual definitons and therefore might not compile.

# `NS_CLOSED_ENUM` / `CF_CLOSED_ENUM`
Declares a frozen enum.

## Declaration
```objc
#define NS_CLOSED_ENUM(_type, _name) CF_CLOSED_ENUM(_type, _name
#define CF_CLOSED_ENUM(_type, _name) /* ... */
```
## Parameters

> type name **`_type`**  
> The integer type used for the values of the enum.

> identifier **`_name`**  
> The name of the generated typedef.

## Equivalent attributes
[`__attribute__((enum_extensibility(closed)))`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)
## Discussion

Use the `NS_CLOSED_ENUM` or `CF_CLOSED_ENUM` macro for a simple group of constants that you can never add new cases to. Closed enumerations are useful for representing a finite set of states that you expect people to switch over using a switch statement. Once an enum is marked as closed, it is a binary and source incompatible change to add a new value.

The three cases of `NSComparisonResult`—`NSOrderedAscending`, `NSOrderedSame`, and `NSOrderedDescending`—are an example of a finite set. They're the only logical cases for performing an ordered comparison during tasks like sorting.

Don't use the `NS_CLOSED_ENUM` or `CF_CLOSED_ENUM` macro if:

* You've ever added cases to an enumeration after its initial declaration

* You can think of additional cases you might add later

* The enumeration has any private cases

In these scenarios, use the `NS_ENUM` or `CF_ENUM` macro instead.

## Importing into Swift

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

## References

[Swift Evolution Proposal SE-0192 "Handling Future Enum Cases"](https://github.com/apple/swift-evolution/blob/master/proposals/0192-non-exhaustive-enums.md)

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)  
[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)  
[Clang documentation for `__attribute__(enum_extensibility)`](https://clang.llvm.org/docs/AttributeReference.html#enum-extensibility)

["Grouping Related Objective-C Constants"](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants)

[Article on SwiftjectiveC.com](https://www.swiftjectivec.com/ns_closed_enum/)

## Documentation copyright

> **Note**: This page uses a modified version of the documentation found in the CoreFoundation header files, which are licensed under Apache License v2.0 with Runtime Library Exception.

> **Note**: This page also quotes from Swift Evolution proposals, which are licensed under Apache License v2.0 with Runtime Library Exception.