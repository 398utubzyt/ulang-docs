# Primitives

## Types

Here is a list of predefined, primitive types that can be used throughout a program:

| Name | Byte Size | Notes
|-----|-----|-----
| `i8` | 1 | Signed 8-bit integer
| `i16` | 2 | Signed 16-bit integer
| `i32` | 4 | Signed 32-bit integer
| `i64` | 8 | Signed 64-bit integer
| `i128` | 16 | Signed 128-bit integer
| `isize` | N/A | Signed pointer-sized integer
| `u8` | 1 | Unsigned 8-bit integer
| `u16` | 2 | Unsigned 16-bit integer
| `u32` | 4 | Unsigned 32-bit integer
| `u64` | 8 | Unsigned 64-bit integer
| `u128` | 16 | Unsigned 128-bit integer
| `usize` | N/A | Unsigned pointer-sized integer
| `f16` | 2 | 16-bit floating point number
| `f32` | 4 | 32-bit floating point number
| `f64` | 8 | 64-bit floating point number
| `f128` | 16 | 128-bit floating point number
| `bool` | 1 | Boolean

## Literals

| Name | Description
|-----|-----
| `true` | A boolean whose state is true (1).
| `false` | A boolean whose state is false (0).
| `null` | An unknown value. Can be used for setting pointers to null.

## Returns

| Name | Description
|-----|-----
| `void` | No value will be given when the function is returned.
| `noreturn` | The function with this return type will never return. Useful for infinite loops (`while (true) { ... }`) or program exits.