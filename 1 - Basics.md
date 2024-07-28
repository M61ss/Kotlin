# Basics <!-- omit from toc -->

- [Data types](#data-types)
- [Variables](#variables)
  - [Type inference](#type-inference)
  - [Null values](#null-values)
  - [Constant variables](#constant-variables)
  - [Top level variables](#top-level-variables)
- [Flow control statements](#flow-control-statements)

> [Return to full index](README.md)

## Data types

Kotlin's data types are:

- Signed:
  - `Byte`
  - `Short`
  - `Int`
  - `Long`
- Unsigned:
  - `UByte`
  - `UShort`
  - `UInt`
  - `ULong`
- Floating point:
  - `Float`
  - `Double`
- `Boolean`
- `Char`
- `String`
- `Array`
- `Unit` (== `void` in Java)

> [!IMPORTANT]
>
> These are not primitive types, but wrapper class of these one. So, each of those has its own methods which simplify a lot working with data.

## Variables

### Type inference

Kotlin supports the type inference, so there are two ways to assign values to variables:

```kotlin
var inferred = "The compiler infers that this is a string"
var explicated: String = "This is explicitly a string"
```

### Null values

In Kotlin all variables are not nullable by default. If we want to get the possibility to assign them the `null` value, we have to make explicit the type followed by a question mark:

```kotlin
var nullable: String? = null    // Without the '?' the program doesn't compile
```

### Constant variables

In Kotlin it is possible to create constant variables. We have to declare it using the `val` keyword:

```kotlin
val immutable = "This string is immutable"
```

If we try to change its value, the program doesn't compile.

> [!IMPORTANT] const val
> - `val` is a read-only variable, so we for each initialization we can assign to it a different value
> - `const val` is a constant whose value has to be known at the compile time, so it can be initialized only one time.

### Top level variables

It is possible to define top level variables (both `var` and `val`) that works like C global variables:

```kotlin
val globalVal = "This is a global value"
var globalVar = "This is a global variable"

fun main() {

}
```

## Flow control statements

All flow control statements are more or less equal to those of Java, except for `when` statement. It works like Java's `switch`:

```kotlin
var variable: String? = "Some value"
when(variable) {
    null -> println("I'm null!")
    else -> println(variable)
}
```

If the purpose of the switch is to decide what value assign to a variable, it is possible to build it like this:

```kotlin
var variable: String? = "Some value"
var otherVariable = when(variable) {
    null -> "Replacement value"
    else -> variable
}
```

> [!NOTE] One-line-if
>
> The one-line-if statement is a bit different from Java's one:
>
> ```kotlin
> var variable: String? = "Some value"
> var otherVariable = if(variable != null) variable else "Replacement value"
> ```
>
> If the only purpose is to check null values, we can use this compact syntax:
>
> ```kotlin
> var variable: String? = "Some value"
> var otherVariable = variable ?: "Replacement value"
> ```
>
> If `variable` is null, it will assume `"Replacement value"`, otherwise, it will keep unchanged.

> [!NOTE] For-each
>
> The for-each statement is a bit different from Java's one:
>
> ```kotlin
> for (item in collection) {
>
> }
> ```
