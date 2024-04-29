% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
# Grammar

As of Clang 15.0.5

## References

* <https://github.com/llvm/llvm-project/blob/llvmorg-15.0.5/clang/lib/Parse/ParseObjc.cpp>

## A.2.4 / 6.9 External definitions
external-declaration: [C99 6.9]
: [OBJC]  objc-class-definition
: [OBJC]  objc-class-declaration
: [OBJC]  objc-alias-declaration
: [OBJC]  objc-protocol-definition
: [OBJC]  objc-method-definition
: [OBJC]  `@` `end`

objc-class-declaration:
: `@` `class` objc-class-forward-decl (`,` objc-class-forward-decl)* `;`

objc-class-forward-decl:
: identifier objc-type-parameter-list<sub>opt</sub>

objc-interface:
: objc-class-interface-attributes<sub>opt</sub> objc-class-interface
: objc-category-interface

objc-class-interface:
: `@` `interface` identifier objc-type-parameter-list<sub>opt</sub>
  : objc-superclass<sub>opt</sub> objc-protocol-refs<sub>opt</sub>
  : objc-class-instance-variables<sub>opt</sub> 
  : objc-interface-decl-list
: `@end`


objc-category-interface:
: `@` `interface` identifier objc-type-parameter-list<sub>opt</sub>
  : `(` identifier<sub>opt</sub> `)` objc-protocol-refs<sub>opt</sub>
    : objc-interface-decl-list
  : `@end`

objc-superclass:
: `:` identifier objc-type-arguments<sub>opt</sub>

objc-class-interface-attributes:
: `__attribute__((visibility("default")))`
: `__attribute__((visibility("hidden")))`
: `__attribute__((deprecated))`
: `__attribute__((unavailable))`
: `__attribute__((objc_exception))` - used by NSException on 64-bit
: `__attribute__((objc_root_class))`

objc-type-parameter-list:
: `<` objc-type-parameter (`,` objc-type-parameter)* `>`

objc-type-parameter:
: objc-type-parameter-variance<sub>opt</sub> identifier objc-type-parameter-bound<sub>opt</sub>

objc-type-parameter-bound:
: `:` type-name

objc-type-parameter-variance:
: `__covariant`
: `__contravariant`

