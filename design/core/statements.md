# Statements

## If

```
if (CONDITION) { ... }
```

- `CONDITION` is the boolean condition to check.

If `CONDITION` is true, the code inside the brackets (`{ ... }`) is executed. Otherwise, anything inside the brackets is skipped entirely.

## Else

```
if (CONDITION) { ... } else { ... }
```

If `CONDITION` is false, the code inside the first set of brackets will not be executed. If an `else` keyword is present, the code in the second set of brackets will be executed instead.

Additionally, another `if` statement can be appended after the `else` keyword in order to chain `if` statements together.

```
if (CONDITION) { ... } else if (CONDITION2) { ... }
```

## Match

`match` statements are functionally similar to `switch` statements in languages like C:

```
match (VALUE) {
    A { ... },
    B, C { ... }
}
```

## While

```
while (CONDITION) { ... }
```

Similar to `struct` and `union` definitions, `while` loops can also have names:

```
while (CONDITION) NAME { ... }
```

This can prove useful when attempting to `break` out of loops from within another loop:

```
while (CONDITION) OuterWhile {
    while (CONDITION2) {
        if (CONDITION3) 
            break OuterWhile;
    }
}
```