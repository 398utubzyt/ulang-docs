# Compiler Settings

## Overview

The following is a table of valid compiler flags which can be passed through the command line:

<!-- This is such a dumb hack -->
|Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Flag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Arguments|Notes|
|----|----|----|----|
|[Custom Allocator](#custom-allocator)|`--alloc` <p> `-a`|`allocFunc`, `deallocFunc`|Defines a custom default allocator to be used when using the `new` and `drop` keywords.|

## Details

### Custom Allocator

`--alloc [allocFunc] [deallocFunc]` \
`--alloc null`

In order for this argument to not throw a compiler error when passing function names, the following requirements must be met:

- The allocator function (`allocFunc`) must be a generic function (type `T`) which:
    - Contains a single parameter of type `usize`.
    - Has a return type of `T*`.
- The deallocator function (`deallocFunc`) must be a generic function (type `T`) which:
    - Contains a single parameter of type `T*`.
    - Has a return type of `void`.

Otherwise, if `null` is passed, no default allocator is used. In this case, the compiler will throw an error when `new` or `drop` is used anywhere in the source code.

Example usage:
```
--alloc Name.Space.allocate Name.Space.deallocate
```