# Collections <!-- omit from toc -->

- [Array](#array)
- [List](#list)
- [Map](#map)

> [Return to full index](README.md)

### Array

Arrays are similar to Java's arrays, but, since `Array` is a data type, they have more methods:

```kotlin
fun main() {
  val array = arrayOf("First Element", "Second Element", "Third Element")
  val firstElement = array.get(0)     // This is not possible in Java
}
```

> [!NOTE]
>
> The `Array` type is also inferred because it is a Kotlin native data type (see [Data types](#data-types)).

### List

List are similar to Java, but for accessing elements it is possible to use indexes like arrays:

```kotlin
fun main() {
  val list = listOf("First Element", "Second Element", "Third Element")
  val firstElement = list[0]    // It works!
}
```

### Map

Create Map is a bit different from Java:

```kotlin
fun main() {
  val map = mapOf(1 to "First Element", 2 to "Second Element", 3 to "Third Element")
}
```

This way we created a map that has as keys numbers before the `to` keyword and strings as value after the `to`.

> [!IMPORTANT] to
>
> The `to` operator is very powerful because allow us to create `Pair` objects very simply:
>
> ```kotlin
> val pair = "key" to "value"   // pair is of type 'Pair'
> ```