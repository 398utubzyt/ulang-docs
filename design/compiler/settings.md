# Compiler Settings

## Overview

The following is a table of valid compiler flags which can be passed through the command line:

<!-- This is such a dumb hack -->
|Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Flag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Arguments|Notes|
|----|----|----|----|
|[Default Allocator](#default-allocator)|`--alloc` <p> `-a`|`allocFunc`, `deallocFunc`|Sets the default allocator to be used when using the `new` and `drop` keywords.|
|[Entry Point](#entry-point)|`--entry` <p> `-e`|`entryFunc`|Sets the entry point to be used when execution starts.|
|[No Deprecated](#no-deprecated)|`--nodeprecated` <p> `-d`|N/A|Treats all deprecation warnings as errors.|
|[No Warnings](#no-warnings)|`--nowarn` <p> `-w`|N/A|Treats all warnings as errors.|

## Details

### Default Allocator

`--alloc [allocFunc] [deallocFunc]` \
`--alloc null`

This argument tells the compiler to use a different allocator as the default when using the `new` and `drop` keywords.

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
--alloc Name.Space.customAllocate Name.Space.customDeallocate
```

### Entry Point

`--entry [entryFunc]`

This argument specifies the first function to be executed at program startup. This argument is required in order for a valid executable to be generated, as there is no default value (e.g. `main`).

To prevent a compiler error, the function given by `entryFunc` must adhere to the following requirements:

- The function must return `i32`.
- The function must either:
    - Have no parameters.
    - Contain a single parameter of type `char*`.

The `i32` returned by the function will act as the exit code of the program. Additionally, when the optional `char*` parameter is used, the command which executed the program on the command line will be passed.

Example usage:
```
--entry Name.Space.customEntry
```

### No Deprecated

`--nodeprecated`

This argument tells the compiler to treat any use of deprecated API as an error. This means that any deprecation warning that is thrown will cause compilation to fail—essentially preventing deprecated code from appearing in the build.

### No Warnings

`--nowarn`

This argument tells the compiler to treat all compiler warnings as errors. Consequently, this means that any warning that is thrown will cause compilation to fail. This is good for production builds which aim to be as stable and error-free as possible.