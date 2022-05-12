# Exceptions

> How to multi catch?

use or in try-catch.

```java
try {

} catch (E1 e1 || E2 e2) {

}
```

> How to get the information of the stack trace?

use `printStackTrace()` or `getStackTrace()`.

`getStackTrace()` returns an array of `StackTraceElement`. The first element is the top of the stack and is the last method invocation in the sequence.
