# Memory Management

## Allocations

In U, dynamic memory allocations can occur one of two places: the heap and the stack. This page will cover how to allocate and free memory on both.

### Heap Allocations

This is the most common type of dynamic memory allocation. Similar to most other OOP languages, this is done with a `new` keyword. In its most simplist form, a `new` expression can be generalized into the following expression:

```
new TYPE[COUNT];
```

- `TYPE`: The type of the data being allocated.
- `COUNT`: The amount of `TYPE`s to allocate.

This allocates a chunk of memory on the heap of size `COUNT * sizeof(TYPE)` bytes.

In most cases, `COUNT` is not necessary if only one element is desired. For example:

```
new TYPE;
```

> Note: It should be clarified that these operations return a pointer (e.g. `TYPE*`/`TYPE[COUNT]`), NOT a built-in array/vector. See [Arrays](array.md) if creating a type-safe array is desired instead.

Unlike most other OOP languages, a constructor is not required for initialization. If calling a constructor *is* desired, however, it can be called by the follow expression:

```
new TYPE(...);
```

Normally, if a type is initialized with a constructor and also contains a deconstructor, the deconstructor is called when the constructed object is freed. However, since this is a pointer allocation, a manual free call is required in order to call it. See [Deconstruction](#Deconstruction) for more info.

Additionally, if it is desired to initialize an array of values (see first example), the following syntax can be used:

```
new TYPE[COUNT](...);
```

If an index should be passed to the constructor, a lambda expression can be used to pass an index of type `usize` to it:

```
new TYPE[COUNT] { i => (i, ...) };
```

> Note: `i` is the index variable, which can have any name as long as it is one that is not already used by a previously-defined element.

### Deconstruction

In all cases, a pointer must be freed once it is not needed. To acomplish this, the `drop` keyword can be used:

```
TYPE* x = new TYPE;
...
drop x;
```

It can also be used on multiple pointer variables at once:

```
TYPE* x = new TYPE;
TYPE* y = new TYPE;
TYPE* z = new TYPE;
...
drop x, y, z;
```

### Stack Allocations

Stack allocation syntax is very similar to heap allocation syntax. However, there is a key difference:

```
stack new TYPE[COUNT]; 
```

> Note: All the previously shown variations of the expression also apply here.

The `stack` keyword in front of the expression is used to specify that the allocation should occur on the stack. This also means that the pointer will be automatically freed when exiting the scope.

<!-- Probably don't want to introduce something unsafe here...
### Opaque Pointers

In the case of opaque pointers (pointers that have no underlying type), there are a few more restrictions in how `new` can be used. Here is an example:

```
new [COUNT];
```

In this case, `COUNT` is now the entire size of the buffer in bytes, rather than the amount of elements in it. Also notice how there is no type mentioned in the expression.

Additionally, constructors and deconstructors cannot be used for opaque pointers, because there is no type associated with the pointer to construct.
-->

### Miscellaneous

Different projects require different needs. If a custom default allocator is needed, see the [Custom Allocator](../../compiler/settings.md#custom-allocator) compiler setting.

Most modern platforms support allocating memory on the heap. If a platform's method of memory allocation is unavailable, then no default allocator will be used. In these cases, the compiler will throw an error if a `new` or `drop` statement is used anywhere in the source. To fix the errors, either define a custom allocator, or remove the heap allocations from the source.

<!-- I want to wait until this is more fleshed-out before adding examples.
### Examples

Here are some examples of how allocations are used in context:

```u
public i32 main() {
    u8* a = new u8[16];
    u16[32] b = new u16[32];

    for (usize i = 0; i < sizeof(a); i++)
        a[i] = (u8)i;

    drop a, b;
}
```
-->