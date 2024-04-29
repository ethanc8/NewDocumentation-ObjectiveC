% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception
# `NS_ENUM` macros

This family of macros allows you to create enumerations.

```{toctree}
---
maxdepth: 2
caption: Macros
---
NS_CLOSED_ENUM.md
NS_ENUM.md
NS_ERROR_ENUM.md
```

## How to use the macros

> **Note**: The header file names are links to mirrors of the header files on GitHub.

To get `NS_ENUM` and others using an Apple SDK:

[`<Foundation/NSObjCRuntime.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/NSObjCRuntime.h) which is included in [`<Foundation/Foundation.h>`](https://github.com/phracker/MacOSX-SDKs/blob/master/MacOSX11.3.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/Foundation.h)

To get `CF_ENUM` and others using CoreFoundation:

[`<CoreFoundation/CFAvailability.h>`](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/Base.subproj/CFAvailability.h)

**You must define the macros yourself if you are using GNUstep without Apple's CoreFoundation.**