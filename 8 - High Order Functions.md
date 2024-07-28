# Higher order functions <!-- omit from toc -->

- [Creation](#creation)
- [Nullable predicates](#nullable-predicates)
- [Variables that contain predicates](#variables-that-contain-predicates)
- [Functions that return predicates](#functions-that-return-predicates)
- [Streams](#streams)

> [Return to full index](README.md)

### Creation

Higher order functions are something similar to Java's functional programming:

```kotlin
fun printFilteredStrings(list: List<String>, predicate: (String) -> Boolean) {
  list.forEach {
    if (predicate(it)) {
      println("$it satisfies the predicate")
    }
  }
}

fun main() {
  val list = listof("First", "Second", "Third")
  printFilteredStrings(list, { it.startsWith("S") })
}
```

As shown above, we pass to `printFilteredStrings()` `list` and a lambda that take as input a `String` and return as output a `Boolean`, as the function require to work.

It is possible to simplify the syntax:

```kotlin
fun printFilteredStrings(list: List<String>, predicate: (String) -> Boolean) {
  list.forEach {
    if (predicate(it)) {
      println("$it satisfies the predicate")
    }
  }
}

fun main() {
  val list = listof("First", "Second", "Third")
  printFilteredStrings(list) { 
    it.startsWith("S") 
  }
}
```

It is easy to intuite that `forEach()` method is also a high order function with only one lambda as required parameter and that returns `Unit`.

### Nullable predicates

Predicates can also be nullable:

```kotlin
fun printFilteredStrings(list: List<String>, predicate: ((String) -> Boolean)?) {
  list.forEach {
    if (predicate?.invoke(it) == true) {
      println("$it satisfies the predicate")
    }
  }
}

fun main() {
  val list = listof("First", "Second", "Third")
  printFilteredStrings(list) { 
    it.startsWith("S") 
  }
}
```

To prevent access to something that could be null, in Kotlin exists a check like that shown above: `predicate?.invoke(it)`. That code allow the compiler to check if `predicate` is null, then, in case it isn't null, invoke it with its own required parameters.

### Variables that contain predicates

It is also possible to store lambdas inside variables and use those further:

```kotlin
fun printFilteredStrings(list: List<String>, predicate: ((String) -> Boolean)?) {
  list.forEach {
    if (predicate?.invoke(it) == true) {
      println("$it satisfies the predicate")
    }
  }
}

fun main() {
  val list = listof("First", "Second", "Third")
  val predicate: (String) -> Boolean = {
    it.startsWith("S")
  }
  printFilteredStrings(list, predicate)
}
```

### Functions that return predicates

Another possibility is to create functions that return lambdas:

```kotlin
fun printFilteredStrings(list: List<String>, predicate: ((String) -> Boolean)?) {
  list.forEach {
    if (predicate?.invoke(it) == true) {
      println("$it satisfies the predicate")
    }
  }
}

fun getPredicate(): (String) -> Boolean {
  return {
    it.startsWith("S") 
  }
}

fun main() {
  val list = listof("First", "Second", "Third")
  printFilteredStrings(list, getPredicate())
}
```

### Streams

It is possible to build streams similarly to Java creating a chain of high order functions one after the other. Every lambda takes as input the output of the previous exactly like Java:

```kotlin
fun main() {
  val list = listof("First", "Second", "Third")
  list
      .filter {
        it.startsWith("T")
      }
      .forEach {
        println(it)
      }
}
```

The difference between Kotlin and Java is that in Kotlin we don't need to convert the list to a stream using `.stream()` method.

> [!NOTE]
>
> If `list` contains null values first of all we need to remove null items. We can do that using `filterNonNull()` function:
>
> ```kotlin
> fun main() {
>   val list = listof("First", "Second", "Third", null, null)
>   list
>       .filterNonNull()
>       .filter {
>         it.startsWith("T")
>       }
>       .forEach {
>         println(it)
>       }
> }
> ```
>
> So this way all null values will be removed.

> [!IMPORTANT] inline
>
> All this high order functions are defined in the standard library as `inline`. It is a common practice to define high order functions as `inline` to improve performance avoiding to call functions since these haven't a body.