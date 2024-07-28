# Classes <!-- omit from toc -->

- [Creation](#creation)
- [Instantiation](#instantiation)
- [Constructors](#constructors)
- [Getters \& Setters](#getters--setters)
- [Methods](#methods)
- [`equals`](#equals)
- [Visibility modifier](#visibility-modifier)
- [Abstract classes](#abstract-classes)

> [Return to full index](README.md)

### Creation

A basic class declaration looks like this:

```kotlin
class MyClass {

}
```

> [!NOTE]
>
> It works even if we omit curly braces:
>
> ```kotlin
> class MyClass
> ```
>
> Works as well as:
>
> ```kotlin
> class MyClass {
>
> }
> ```

### Instantiation

We can create an instance of a class like this:

```kotlin
fun main() {
  val myClass = MyClass()   // Constructor call
}
```

### Constructors

Constructors in Kotlin are very different from Java. The primary constructor's arguments are defined in round brackets after the class' name and it is implemented inside the `init` block:

```kotlin
class MyClass(_firstParameter: String, _secondParameter: String) {
  val firstName: String
  val lastName: String

  // Primary constructor
  init {
    firstName = _firstName
    lastName = _lastName
  }
}
```

The `init` block isn't mandatory. If the unique purpose of the constructor is to assign values to properties, the following syntax is equivalent and more clear:

```kotlin
class MyClass(firstParameter: String, secondParameter: String)
```

It works as well as the previous.

> [!NOTE]
>
> Underscores are used simply to distinguish parameters of the constructor from class' properties

> [!NOTE] Multiple init
>
> It is also possible to define multiple `init` blocks inside the same class. It will be executed one after the other.

If the unique purpose is to assign values to class' properties, we can omit the `init` block and assign them directly to properties:

```kotlin
class MyClass(_firstParameter: String, _secondParameter: String) {
  val firstName: String = _firstName
  val lastName: String = _lastName
}
```

It exists also a more compact way to create the primary constructor:

```kotlin
firstParameter: String, val firstParameter
class MotherVClass(val firstParameter: String, val secondParametr: String)
```

It works as well as previous ones.

According to our needs, we have the possibility to define other constructors:

```kotlin
class MyClass(val firstParameter: String, val secondParameter: String) {
  constructor(): this("First default value", "Second default value") {

  }
}
```

The secondary constructor that we defined takes no parameters, but is followed to a call to the primary constructor, which must be called in every case, using `this()`. This means that `init` blocks will be executed anyway before the secondary constructor.

> [!NOTE]
>
> For the primary constructor it is correct also the following syntax:
>
> ```kotlin
> class MyClass constructor(firstParameter: String, secondParameter: String)
> ```
>
> But `constructor` keyword is always omitted as it is redundant.

### Getters & Setters

When we try to access class' properties using getters and setters, we have simply to use the same syntax that in Java is used to directly access fields. In Kotlin there is no possibility to access properties without pass throught getters and setters because they are called by the compiler every time we try to access a class' field:

```kotlin
class MyClass(var firstParameter: String, val secondParameter: String) {

}

fun main() {
  var instance = MyClass("First Value", "Second Value")
  instance.firstParameter = "Other Value"       // Setter call
  var variable = instance.firstParameter        // Getter call
  var otherVariable = instance.secondParameter  // Getter call
}
```

Constant values (`val`) haven't setters.

As we can see, elementary getters and setters are implicit. If we want to define a specific behaviour, we have to implement them like this:

```kotlin
class MyClass(var firstParameter: String, val secondParameter: String) {
  var property: String? = null
    set(value) {
      field = value
      println("$field updated")   // Customization that we wanted to add setting
    }
    get() {
      println("$field has been accessed")   // Customization that we wanted to add getting
      return field
    }
}

fun main() {
  var instance = MyClass("First Value", "Second Value")
  instance.property = "Other Value"       // Setter call
  var variable = instance.property        // Getter call
}
```

When we access the field, the compiler invokes automatically the getter and the setter that we implemented.

> [!NOTE] field
>
> `field` is the default name of the variable that inside `set` and `get` lambdas represents the property variable.

### Methods

To create methods inside a class we have simply to create a normal function inside it:

```kotlin
class MyClass(var firstParameter: String, val secondParameter: String) {
  fun printInfo() {
    println("The first parameter is: $firstParameter")
    println("The second parameter is: $secondParameter")
  }
}

fun main() {
  var instance = MyClass("First Value", "Second Value")
  instance.printInfo()
}
```

As we can see, methods are really similar to Java.

### `equals`

In Kotlin it is not necessary to use explicitly `equals()` method to check the equality of two object because we can use simply the `==` operator, that in Java it is used to check the referential equality:

```kotlin
class MyClass(val property: String)

fun main() {
  val firstInstance = MyClass("Property")
  val secondInstance = MyClass("Property")

  if (firstInstance == secondInstance) {    // It is true
    println("The two instance are equal")
  }
}
```

In Kotlin it is still possible to check referential equality using the `===` operator: 

```kotlin
class MyClass(val property: String)

fun main() {
  val firstInstance = MyClass("Property")
  val secondInstance = MyClass("Property")

  if (firstInstance === secondInstance) {    // It is false
    println("The two instance are equal")
  }
}
```

### Visibility modifier

Visibility modifier in Kotlin are:

- `public`: the visibility is global (default visibility).
- `internal`: the visibility is limited to element's module.
- `protected`: the visibility is limited to the class that contains the `protected` element and its subclass.

  > [!WARNING]
  > Class can't be `protected`.

- `private`: the visibility is limited to the file that contains the element.

### Abstract classes

An abstract class is created adding the `abstract` keyword before `class`:

```kotlin
abstract class MyClass
```

Abstract classes are very similar to those of Java. Remember that they are not instantiable.