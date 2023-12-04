# Generic Types

Generic types are types (usually `struct`) which take in other types as "type arguments". For example:

```u
struct Vector<T> { ... }
```

In this case, `T` is a placeholder for a type which will be specified elsewhere:

```u
Vector<u8> buffer = ...;
```

For a full example, consider the `Vector<T>` type in the standard library:

```u
namespace Core {
    struct Vector<T> {
        private T& _pointer;
        private usize _count;
        private usize _size;

        public unsafe T& getPointer(&this) { return this._pointer; }

        // Constructors omitted for simplicity
        ...
    }
}
```

The type `T` is used in place of where a type should be. In this sense, `T` should always be considered a valid type—even if it is not yet known to the developer.

## Constraints

Generic types can be constrained to only allow for certain types. For example:

```u
struct GenericStruct<T> T : u8, u16, u32, u64, u128, usize { ... }
```

If there are multiple type parameters:

```u
struct MultiGenericStruct<T0, T1>
    T0 : u8, u16
    T1 : f32, f64
{
    ...
}
```

> Note: Multiple parameter constraints are allowed on the same line, but they must be separated by whitespace in some way.

The general syntax is as follows, where `T` is a type parameter and `typeX` is an allowed type: `T : type0, type1, type2, ..., typeN`.

It is also important to note that, like function parameters, local variables (within the generic parameter's scope) must not conflict with the name of the parameter.

## Functions

Functions can also contain type constraints. Function type constraints are treated exactly the same as type constraints used in a `struct` or `interface`, the only difference is the available scope. This means that generic parameters defined in a function cannot be used outside of that function.

Example of a generic function:

```u
void genericFunction<T>() T : u8, u16 { ... }
```

## `interface` Constraints

A constraint which only contains an `interface` type means that its functions may be used. For example:

```u
interface ExampleInterface {
    public void typeFunction();
    public void instanceFunction(&this);
}

...

void otherFunction<T>() T : ExampleInterface {
    T.typeFunction();
    ...

    T instance = ...;
    instance.instanceFunction();
}
```
