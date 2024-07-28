# Functions <!-- omit from toc -->

- [Main function](#main-function)
- [Free function](#free-function)
- [Single expression functions](#single-expression-functions)
- [Input \& Output](#input--output)
- [Lambdas](#lambdas)
- [`vararg`](#vararg)
- [Named parameters](#named-parameters)
- [Default values](#default-values)
- [`inline` keyword](#inline-keyword)

> [Return to full index](README.md)

### Main function

Unlike Java, Kotlin doesn't need to include the main function inside a class:

```kotlin
fun main() {

}
```

If we want to pass arguments, we need to compose the function this way:

```kotlin
fun main(args: Array<String>) {

}
```

### Free function

Free function means that the function is not associated to any class.
A basic free function without arguments in Kotlin is like that:

```kotlin
fun myFunction() {

}
```

Arguments are defined inside round brackets and we can assign to each argument a default value, even if it is not mandatory:

```kotlin
fun myFunction(argument: String = "Default value") {

}
```

It is possible to define a return type:

```kotlin
fun myFunction(): String {
  return "Return value"
}
```

> [!TIP] String templates
>
> In case we have arguments and we want to build a string composed of this arguments consider to use string templates instead of boilerplate concatenations:
>
> ```kotlin
> fun myFunction(argument: String = "value") {
>   return "Return $argument"
> }
> ```
>
> It is equivalent to:
>
> ```kotlin
> fun myFunction(argument: String = "value") {
>   return "Return " + argument
> }
> ```

It is also possible to return nothing, so the return type is `Unit`:

```kotlin
fun myFunction() {

}
```

Return type and return statement are implicit.

### Single expression functions

Single expression functions in Kotlin supports type inference, so it is possible to compose those like this:

```kotlin
fun myFunction() = "Return Value"
```

As we can see, the return type is inferred. It is absolutely equivalent to:

```kotlin
fun myFunction(): String {
  return "Return value"
}
```

### Input & Output

Function are called like C.
\
To print something on `stdout` we use `print()` function:

```kotlin
fun main() {
  print("Message")    // The \n character has to be explicated
  println("Line")     // The \n character is implicit
}
```

To read something from `stdin` we use `readln()` function:

```kotlin
fun main() {
  var input = readln()
}
```

### Lambdas

Lambdas works at the same way of Java, but have a different syntax. It is called "inline functions":

```kotlin
inline fun myLambda() {

}
```

To use an inline function refer to this example that iterates elements of a collection:

```kotlin
fun main() {
  collection.forEach {
    println(it)
  }
}
```

`it` is the default name of lambdas' input element. 
\
We can even specify a custom one:

```kotlin
fun main() {
  collection.forEach { item ->
    println(item)
  }
}
```

### `vararg`

In Kotlin it is possible to define functions that accept a variable number of a specific argument:

```kotlin
fun myFunction(firstArgument: String, vararg secondArgument: String) {

}
```

In that function it is possible to pass a variable number of the type of `secondArgument`. Kotlin will treat the second argument of that function as an `Array`. Let's see an example:

```kotlin
fun greeter(greeting: String, vararg toBeGreeted: String) {
  toBeGreeted.forEach { subject ->
    println("$greeting $subject")
  }
}

fun main() {
  greeter("Hi")   // It works (toBeGreeted will be treated as an empty Array)
  greeter("Hi", "Lisa", "Luca", "Katia")    // It works (toBeGreeted will be treated as Array of size 3)
}
```

As we can see in the example `toBeGreeted` is treated as an Array inside `greeter()`, but when we call this function we can pass an arbitrary number of parameters, that could be also 0!

> [!TIP]
>
> If we want to pass an array to `greeter()` instead of a sequence of single parameters, we should operate like this:
>
> ```kotlin
> fun main() {
>   val array = arrayOf("Lisa", "Luca", "Katia")
>   greeter("Hi", *array)
> }
> ```

### Named parameters

To get the code more readable and to simplify coding, Kotlin provides the possibility to pass parameters to function without respecting them order, assigning to each parameter its value using its name:

```kotlin
fun function(parameter: String, otherParameter: String) {

}

fun main() {
  function("Value", "Other value")
  function(parameter = "Value", otherParameter = "Other value")
  function(otherParameter = "Other value", parameter = "Value")
}
```

The first function call works as well as the second one as well as the third one.

### Default values

It is possible to define functions that have a default value for one or more parameters:

```kotlin
fun function(parameter: String = "Default value", otherParameter: String) {

}

fun main() {
  function(otherParameter = "Other value")
  function(parameter = "Custom value", otherParameter = "Other value")
}
```

If we doesn't pass any value as `parameter`, the function uses its default value assigned in its definition.

### `inline` keyword

In Kotlin we can declare functions as `inline`, so that the compiler will substituite all function calls with the function body. This is a practice that allows to improve performance removing the overhead caused by calling functions:

```kotlin
inline fun myFunction()
```

See [Streams](#streams) to understand its powerful.