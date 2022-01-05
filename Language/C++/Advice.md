# Advice

## Decide which type to use

- Use an unsigned type when you know that the values cannot be negative.
- Use `int` for integer arithmetic. If `int` is small, use `long long`.
- Do not use plain `char` or `bool` in arithmetic expressions.
- Use `double` for floating-point computations.

如果函数无须改变引用形参的值，最好将其声明为常量引用