# User Types

Most programs will require their own types to make things work. There are a few ways to accomplish this, but they all follow roughly the same syntax. These syntaxes are called:

- [struct](##Struct)
- [interface](##Interface)
- [union](##Union)
- [enum](##Enum)

In general, the syntax follows this form:

```
TYPEDEF NAME { ... }
```
- `TYPEDEF` is one of the syntax options listed above.
- `NAME` is an arbitrary name that must follow conventional naming rules.

## Struct

```
struct NAME { ... }
```

`struct`s are the primary way of organizing data. They can consist of both fields and functions:

```
struct IntFloat {
    u32 integer;
    f32 float;

    IntFloat sum(IntFloat a, IntFloat b) {
        return IntFloat {
            .integer = a.integer + b.integer,
            .float a.float + b.float
        };
    }
}
```

> Note: The only data contained within an instance of a struct are its fields. Struct functions are treated as standard functions within a namespace:
> ```
> void example_sum() {
>     IntFloat a = IntFloat { .integer = 2, .float = 3.0f };
>     IntFloat b = IntFloat { .integer = 1, .float = 4.0f };
>     IntFloat sum = IntFloat.sum(a, b);
>     ...
> }
> ```

`struct` functions have an extra feature called the `self` parameter. Prefixing the first parameter with `*` instead of a type name will do a few things:

- The parameter is automatically passed when calling the function, unless there is already a method which uses the same signature.
- The parameter becomes a pointer to the instance which contains the function.

```
struct IntFloat {
    u32 integer;
    f32 float;

    IntFloat add(&self, IntFloat other) {
        return IntFloat {
            .integer = self.integer + other.integer,
            .float = self.float + other.float
        };
    }
}

void example_add() {
    IntFloat a = IntFloat { .integer = 2, .float = 3.0f };
    IntFloat b = IntFloat { .integer = 1, .float = 4.0f };
    IntFloat sum = a.add(b);
}
```

## Interface

```
interface NAME { ... }
```

`interface`s are the primary way of defining shared functions between types. In order to implement an interface for a specific type, use the `impl` keyword with the following syntax:

```
impl TYPE : INTERFACE { ... }
```

Unlike `struct`s, `interface`s can *only* contain function definitions. Function implementations go inside of the interface `impl`:

```
interface ExampleInterface {
    void function1();
    void function2(&this);
}

impl u8 : ExampleInterface {
    void function1() {
        ...
    }

    void function2(&this) {
        ...
    }
}
```

Additionally, constructors and deconstructors can be defined using the `new` and `drop` keywords:

```
interface ExampleInterface {
    public new(&this);
    public drop(&this);
}

impl u8 : ExampleInterface {
    void new(&this) {
        ...
    }

    void drop(&this) {
        ...
    }
}
```
> Note: Deconstructors are limited to having no parameters, or a single `this` parameter. This is because they are automatically called when they fall out of scope.

## Union

```
union NAME { ... }
```

`union`s are a method of using multiple data formats for a single block of data. For example:

```
union ExampleUnion {
    u32 integer;
    f32 float;
}
```

Unlike a `struct`, the size of this union is only 4 bytes. This is because the fields `integer` and `float` are actually located inside the same region of memory. Adding more fields to a union does not use more memory, but rather adds more interpretations for the same region of memory.

> Note: The size of the `union` is equal to the size of the largest data type contained within it.

## Enum

```
enum NAME { ... }
```

`enum`s describe a set of constants of the same type. For example:

```
enum ExampleEnum : u16 {
    A = 0,
    B = 1,
    C = 2
}
```

```
enum AnotherExampleEnum : f32 {
    A = 0.0f,
    B = 0.5f,
    C = 1.0f,
}
```

It's important to notice that the type which the enum extends from *must* be a [primitive type](types.md#types).
