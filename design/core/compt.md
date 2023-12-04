# Compile-time

Often times, running or evaluating code at compile-time is desired—whether it's for performance gain or to help generate code. In U, running code at compile-time will require the `compt` keyword (short for `comp[ile-]t[ime]`).

## Compile-time Evaluation

### `compt` Functions

Functions marked with `compt` are considered to be available for compile-time if it meets the following criteria:

- The parameters passed to it are either:
  - Known constants
  - Constrained by the `compt` keyword
- Any functions called within it are also marked with `compt`

Consider the following examples:

```u
public compt u32 nextPow2(u32 a) {
    --x;
    x |= x >> 1;
    x |= x >> 2;
    x |= x >> 4;
    x |= x >> 8;
    x |= x >> 16;
    return ++x;
}
```

> The function above is a *valid* `compt` function and will be available for evaluation

```u
public void incompatibleFunction() { ... }

public compt void update(f32 delta) {
    incompatibleFunction();
    ...
}
```

> The function above is an *invalid* `compt` function and will ***NOT*** be available for evaluation

Many math functions within the standard library make use of `compt` to allow for an overall faster runtime.

### `compt` Parameters

Function parameters marked with `compt` are *required* to be known at runtime. This means that the value must be one of the following:

- A known constant
- Evaluatable at compile-time

For example, consider the function and its usages:

```u
public void compt someFunction(compt f32 param) { ... }
```

> Note: the `compt` constraint is not required on the function itself to constrain a parameter with `compt`.

```u
public const f32 KNOWN_CONSTANT = 3;

public void doSomething() {
    someFunction(KNOWN_CONSTANT);

    f32 local = 5.2;
    someFunction(local);
}
```

> The function above will compile successfully, because both calls to `someFunction` pass a parameter whose value is known at compile-time.

```u
public f32 getRandom() { ... }

public void doSomethingElse() {
    f32 randValue = getRandom();
    someFunction(randValue);
}
```

> The function above will cause a compiler error, because it passes a random value to `someFunction`—not something that can be known at compile-time.
