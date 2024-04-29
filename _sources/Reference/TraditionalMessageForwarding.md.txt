% SPDX-License-Identifier: GPL-3.0-or-later
# Traditional Message Forwarding
## GCC runtime

Taken from GCC's [`<objc/message.h>`](https://github.com/gcc-mirror/gcc/blob/16e2427f50c208dfe07d07f18009969502c25dc8/libobjc/objc/message.h).

```objc
IMP objc_msg_lookup (id receiver, SEL op);
```
If `receiver` does not respond to the selector `op`, `objc_msg_lookup()`
will try to call `+resolveClassMethod:` or `resolveInstanceMethod:` as
appropriate, and if they return `YES`, it will try the lookup again
(`+resolveClassMethod:` and `+resolveInstanceMethod:` can thus install
dynamically methods as they are requested).  If
`+resolveClassMethod:` or `+resolveInstanceMethod:` are either not
available, or return NO, or return YES but 'receiver' still doesn't
implement the 'selector' after calling them, the runtime returns a
generic "forwarding" function that can be called with the required
method signature and which can process the method invocation
according to the forwarding API.  There are two runtime hooks that
allow Foundation libraries (such as GNUstep-Base) to return their
own forwarding function in preference to the runtime ones.  When
that happens, the Foundation library effectively takes complete
control of the forwarding process; any method invocation where the
selector is not implemented by the receiver will end up calling a
forwarding function chosen by the Foundation library.

```objc
objc_EXPORT IMP (*__objc_msg_forward)(SEL);
objc_EXPORT IMP (*__objc_msg_forward2)(id, SEL);
```
Hooks for method forwarding.  They make it easy to substitute the
built-in forwarding with one based on a library, such as `ffi`, that
implement closures, thereby avoiding gcc's `__builtin_apply`
problems.  `__objc_msg_forward2`'s result will be preferred over that
of `__objc_msg_forward` if both are set and return non-NULL.

## Apple/NeXT runtime

The forward handler is set by the function `objc_setForwardHandler`. The default forward handler in Objective-C 1.x is `nil`. The runtime, if it detects a forward handler of `nil`, will call `forward::` on the object. In Objective-C 2.x, the default forward handler prints a message detailing the error and exits the program.

## `objc_setForwardHandler`
Sets the function to be called by `objc_msgForward`.

Defined in `<objc/runtime.h>`.

### Declaration

```objc
void objc_setForwardHandler(void * _Nonnull fwd, void * _Nonnull fwd_stret);
```

### Paramaters



### Code snippets

`<objc/message.h>`
```objc
/* Message Forwarding Primitives
 * Use these functions to forward a message as if the receiver did not 
 * respond to it. 
 *
 * The receiver must not be nil.
 * 
 * class_getMethodImplementation() may return (IMP)_objc_msgForward.
 * class_getMethodImplementation_stret() may return (IMP)_objc_msgForward_stret
 * 
 * These functions must be cast to an appropriate function pointer type 
 * before being called. 
 *
 * Before Mac OS X 10.6, _objc_msgForward must not be called directly 
 * but may be compared to other IMP values.
 */
#if !OBJC_OLD_DISPATCH_PROTOTYPES
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wincompatible-library-redeclaration"
OBJC_EXPORT void
_objc_msgForward(void /* id receiver, SEL sel, ... */ ) 
    OBJC_AVAILABLE(10.0, 2.0, 9.0, 1.0, 2.0);

OBJC_EXPORT void
_objc_msgForward_stret(void /* id receiver, SEL sel, ... */ ) 
    OBJC_AVAILABLE(10.6, 3.0, 9.0, 1.0, 2.0)
    OBJC_ARM64_UNAVAILABLE;
#pragma clang diagnostic pop
#else
OBJC_EXPORT id _Nullable
_objc_msgForward(id _Nonnull receiver, SEL _Nonnull sel, ...) 
    OBJC_AVAILABLE(10.0, 2.0, 9.0, 1.0, 2.0);

OBJC_EXPORT void
_objc_msgForward_stret(id _Nonnull receiver, SEL _Nonnull sel, ...) 
    OBJC_AVAILABLE(10.6, 3.0, 9.0, 1.0, 2.0)
    OBJC_ARM64_UNAVAILABLE;
#endif
```
`objc-runtime.mm`
```objc
#if !__OBJC2__

// Default forward handler (nil) goes to forward:: dispatch.
void *_objc_forward_handler = nil;
void *_objc_forward_stret_handler = nil;

#else

// Default forward handler halts the process.
__attribute__((noreturn, cold)) void
objc_defaultForwardHandler(id self, SEL sel)
{
    _objc_fatal("%c[%s %s]: unrecognized selector sent to instance %p "
                "(no message forward handler is installed)", 
                class_isMetaClass(object_getClass(self)) ? '+' : '-', 
                object_getClassName(self), sel_getName(sel), self);
}
void *_objc_forward_handler = (void*)objc_defaultForwardHandler;

#if SUPPORT_STRET
struct stret { int i[100]; };
__attribute__((noreturn, cold)) struct stret
objc_defaultForwardStretHandler(id self, SEL sel)
{
    objc_defaultForwardHandler(self, sel);
}
void *_objc_forward_stret_handler = (void*)objc_defaultForwardStretHandler;
#endif

#endif

void objc_setForwardHandler(void *fwd, void *fwd_stret)
{
    _objc_forward_handler = fwd;
#if SUPPORT_STRET
    _objc_forward_stret_handler = fwd_stret;
#endif
}
```

## References

[NeXT Release 3.3 documentation for `Object` class](http://www.cilinder.be/docs/next/NeXTStep/3.3/nd/GeneralRef/01_RootClass/Classes/Object.htmld/index.html)

[NeXT Release 3.3 documentation for runtime functions](http://www.cilinder.be/docs/next/NeXTStep/3.3/nd/GeneralRef/15_RunTime/Functions/RunTimeFunctions.htmld/index.html)

[Re: `objc_setForwardHandler`](http://web.archive.org/web/20140115122206if_/http://lists.apple.com/archives/objc-language/2007/Oct/msg00036.html)

### Apple Objective-C runtime source code (version 818.2)

[`<objc/runtime.h>`](https://opensource.apple.com/source/objc4/objc4-818.2/runtime/runtime.h.auto.html)

[`<objc/message.h>`](https://opensource.apple.com/source/objc4/objc4-818.2/runtime/message.h.auto.html)

[`objc-runtime.mm`](https://opensource.apple.com/source/objc4/objc4-818.2/runtime/objc-runtime.mm)