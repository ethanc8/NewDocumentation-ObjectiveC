# String literals
## Description
`NSConstantString` or `NXConstantString` are the classes used by string literals.

> Includes part of Cppreference's [C -> Language -> String Literals](https://en.cppreference.com/w/c/language/string_literal)

## Syntax
| | | |
| --- | --- | --- |
| `@"` *s-char-sequence* `"` | (1) | (since )
where
> ***`s-char-sequence`***  
> 	zero or more characters, each of which is either a multibyte character from the source character set (excluding (`"`), `\`, and newline), or character escape, hex escape, octal escape, or universal character name (since C99) as defined in [escape sequences](https://en.cppreference.com/w/c/language/escape).

1\) *NSConstantString literal*: The type of the literal is `NSConstantString*` (if Foundation is being used).

## Layout
### GCC

```objc
struct {
    Class isa;
    char* c_string;
    unsigned int len;
}
```
### GNUstep v2 ABI

```objc
struct {
    Class isa;
    /**
     * Flags value.  The low 16 bits are reserved for the compiler, the top 16
     * for the class implementation.  Currently, only the low 2 bits are
     * defined by the compiler.
     */
    uint32_t flags;
    /**
     * The number of UTF-16 code units in the string.
     */
    uint32_t length;
    /**
     * The number of bytes occupied by the string.
     */
    uint32_t size;
    /**
     * Storage preallocated for the hash (not initialized by the compiler).
     */
    uint32_t hash;
    /**
     * Data pointer; format described by the bottom bits of `flags`.
     */
    const char * const str;
}
```
