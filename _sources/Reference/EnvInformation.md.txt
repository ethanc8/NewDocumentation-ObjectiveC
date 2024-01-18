# Getting Environment Information

## Feature checking at runtime

### Checking for class existence

Use [`NSClassFromString`](https://developer.apple.com/documentation/foundation/1395135-nsclassfromstring?language=objc).

```objc
if ((NSClassFromString(@"SomeClass")) != nil) {
    // Class exists
} else {
    // Class doesn't exist
}
```

### Checking if a class or object implements a method

With the declarations:
```objc
@class Classname;
SEL selector = @selector(...);
id myObject = [[Classname alloc] init];
```
To determine whether a class implements a class method, use:
```objc
[Classname respondsToSelector: selector]
```
To determine whether a class implements an instance method, use:
```objc
[Classname instancesRespondToSelector: selector]
```
To determine if an object implements an instance method, use:
```objc
[myObject respondsToSelector: selector]
```
Each of these will return `YES` if the class/instance implements the method, and `NO` if the class/instance does not.

## Checking versions of system libraries at runtime

### CoreFoundation

**Header**: `<CoreFoundation/CFBase.h>` - [Apple](https://github.com/apple/swift-corelibs-foundation/blob/main/CoreFoundation/Base.subproj/CFBase.h) | [GNUstep CoreBase](https://github.com/gnustep/libs-corebase/blob/master/Headers/CoreFoundation/CFBase.h.in)  
**Official Documentation**: [Framework Version Numbers (outdated)](https://developer.apple.com/documentation/corefoundation/base_utilities/framework_version_numbers?language=objc)  
**Introduced in**: Mac OS X 10.1

The current CoreFoundation version number is in the variable:
```objc
extern double kCFCoreFoundationVersionNumber;
```
The CoreFoundation version number for each Mac OS X version is given in a `#define` macro.

For example, OS X version 10.0 had CoreFoundation version:
```objc
#define kCFCoreFoundationVersionNumber10_0	196.40
```
The version number for any version is defined in the next SDK version. For example, the OS X 10.9 SDK introduced the version number for 10.8, but did not define the version number for 10.9.

See the official documentation or the header file for a list of all the version numbers.

> **Warning**: The last defined CoreFoundation versions are the versions for Mac OS X 10.11.4 (1258.1) and iOS 9.4 (1280.38).

> **Swift note**: Accessing this variable when using the open-source CoreFoundation code from `swift-corelibs-foundation` will cause a linker error, even though the header file defines it.

> **GNUstep note**: GNUstep CoreBase defines `kCFCoreFoundationVersionNumber` to 550.13, which was the version included in OS X 10.6.2.

CoreFoundation versions past OSX 10.11 (according to opensource.apple.com)

| Mac OS X version | CoreFoundation version |
| --- | --- |
| 10.12.* | 1348.* |
| 10.13.* | 1443.* |

### Foundation

**Header**: `<Foundation/NSObjCRuntime.h>` - [Apple](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h)
**Official Documentation**: [Framework Version Numbers](https://developer.apple.com/documentation/foundation/object_runtime/foundation_framework_version_numbers?language=objc)  
**Introduced in**: Mac OS X 10.1

The current Foundation version number is in the variable:
```objc
extern double NSFoundationVersionNumber;
```
The Foundation version number for each Mac OS X version is given in a `#define` macro.

For example, OS X version 10.0 had Foundation version:
```objc
#define NSFoundationVersionNumber10_0	397.40
```
The version number for any version is defined in the next SDK version. For example, the OS X 10.9 SDK introduced the version number for 10.8, but did not define the version number for 10.9.

See the official documentation or the header file for a list of all the version numbers.

> **Warning**: The last defined Foundation versions are the versions for Mac OS X 10.11.4 (1258) and iOS 9.4 (1280.25).

> **Swift note**: Accessing this variable when using `swift-corelibs-foundation` may result in garbage, `NSFoundationVersionNumber10_11_Max`, or `NSFoundationVersionNumber_iOS_9_x_Max`

> **GNUstep note**: GNUstep Base does not define `NSFoundationVersionNumber`.

### AppKit

**Header**: `<AppKit/NSApplication.h>` - [Apple](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/AppKit.framework/Versions/C/Headers/NSApplication.h) | [GNUstep](https://github.com/gnustep/libs-gui/blob/master/Headers/AppKit/NSApplication.h)  
**Official Documentation**: [NSAppKitVersionNumber](https://developer.apple.com/documentation/appkit/nsappkitversionnumber?language=objc)
**Introduced in**: Mac OS X 10.1

Every major OSX release has included a new AppKit version. The current AppKit version number is in the `const` variable:
```objc
extern const double NSAppKitVersionNumber
```
The AppKit version number for each Mac OS X version is given in a `static const` variable or a `#define` macro.

For example, OS X version 10.0 had AppKit version:
```objc
static const double NSAppKitVersionNumber10_0 = 577;
```
or:
```objc
#define NSAppKitVersionNumber10_0 577
```
The version number for any version is defined in the next SDK version. For example, the OS X 10.9 SDK introduced the version number for 10.8, but did not define the version number for 10.9.

See the official documentation or the header file for a list of all the version numbers.

> **GNUstep note**: GNUstep GUI defines `NSAppKitVersionNumber` to 824, which was the version included in OS X 10.4.

## TargetConditionals.h

> This section quotes from the header files.

### Includes
For Clang with Apple SDKs:
[`<TargetConditionals.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/usr/include/TargetConditionals.h)

For CoreFoundation:
[`<CoreFoundation/TargetConditionals.h>`](https://github.com/apple/swift-corelibs-foundation/blob/main/CoreFoundation/Base.subproj/SwiftRuntime/TargetConditionals.h)

### Defined macros

Which macros are defined varies between SDK version. The macros documented at the top of the header file might not be defined the same way in the header itself.

### Examples

Detect building for iOS:

```objc
#if (TARGET_OS_IPHONE) || (TARGET_IPHONE_SIMULATOR) || (TARGET_OS_IOS) || (TARGET_OS_SIMULATOR)
    ...
#endif
```

## 
