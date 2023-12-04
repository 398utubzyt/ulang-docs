# Keywords

## If

```u
if (CONDITION) { ... }
```

- `CONDITION` is a boolean condition. (`true`/`false`)

If `CONDITION` is true, the code inside the brackets (`{ ... }`) is executed. Otherwise, anything inside the brackets is skipped entirely.

## Else

```u
if (CONDITION) { ... } else { ... }
```

- `CONDITION` is a boolean condition. (`true`/`false`)

If `CONDITION` is false, the code inside the first set of brackets will not be executed. If an `else` keyword is present, the code in the second set of brackets will be executed instead.

Additionally, another `if` statement can be appended after the `else` keyword in order to chain `if` statements together.

```u
if (CONDITION) { ... } else if (CONDITION2) { ... }
```

## Match

`match` statements are functionally similar to `switch` statements in languages like C:

```u
match (VALUE) {
    A { ... },
    B, C { ... }
}
```

## While

```u
while (CONDITION) { ... }
```

- `CONDITION` is a boolean condition. (`true`/`false`)

Similar to an [`if`](#if) statement, the code inside the brackets (`{ ... }`) is executed when `CONDITION` is `true`. However, the difference between `if` and `while` is that `while` will continuously run the branch `{ ... }` until `CONDITION` is false.

> Note: This can mean that a program can "freeze" if a `while` loop is not handled carefully. While this behavior may be desireable (e.g. `while (true)`), it is recommended to take extreme caution to ensure that a program does not crash by allowing some way to escape the loop.

Similar to `struct` and `union` definitions, `while` loops can also have names:

```u
while (CONDITION) NAME { ... }
```

This can prove useful when using the `break` keyword to break out of loops from within another loop:

```u
while (CONDITION) OuterWhile {
    while (CONDITION2) {
        if (CONDITION3) 
            break OuterWhile;
    }
}
```

- `CONDITION2` and `CONDITION3` are boolean conditions. (`true`/`false`)

## Break

```u
while (CONDITION) {
    ...
    break;
}
```
