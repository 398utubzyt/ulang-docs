# Memory Management

## Allocations

In U, memory allocations can occur one of two places:
- The Stack
- The Heap

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

Unlike most other OOP languages, a constructor is not required for initialization. If calling a constructor *is* desired, however, it can be called by the follow expression:

```
new TYPE(...);
```

If a type is initialized with a constructor and also contains a deconstructor, the deconstructor is called when the constructed object falls out of scope.

Additionally, if it is desired to initialize an array of values (see first example), the following syntax can be used:

```
new TYPE[COUNT](...);
```

If an index should be passed to the constructor, a lambda expression can be used to pass an index of type `usize` to it:

```
new TYPE[COUNT] { i => (i, ...) };
```

> Note: `i` is the index variable, which can have any name as long as it is one that is not already used by a previously-defined element.

### Stack Allocations

Stack allocation syntax is very similar to heap allocation syntax. However, there is a key difference:

```
stack new TYPE[COUNT]; 
```

> Note: All the previously shown variations of the expression also apply here.

The key difference being that there is now a `stack` keyword in front of the expression. This means that the pointer will be allocated on the stack and automatically freed when exiting the scope.

### Opaque Pointers

In the case of opaque pointers (pointers that have no underlying type), there are a few more restrictions in how `new` can be used. Here is an example:

```
new [COUNT];
```

In this case, `COUNT` is now the entire size of the buffer in bytes, rather than the amount of elements in it. Also notice how there is no type mentioned in the expression.

Additionally, constructors and deconstructors cannot be used for opaque pointers, because there is no type associated with the pointer to construct.