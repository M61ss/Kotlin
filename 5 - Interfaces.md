# Interfaces <!-- omit from toc -->

- [Creation](#creation)
- [Implementation](#implementation)
- [Adding method to interfaces](#adding-method-to-interfaces)
- [Default implementation](#default-implementation)
- [Interfaces' variables](#interfaces-variables)
- [`is` and `as` operators: type checking and smart casting](#is-and-as-operators-type-checking-and-smart-casting)

> [Return to full index](README.md)

### Creation

A basic interface looks like this:

```kotlin
interface MyInterface
```

### Implementation

To implement an interface inside a class we have to use this syntax:

```kotlin
interface MyInterface

class MyClass : MyInterface
```

We can implement any interfaces we want:

```kotlin
interface MyInterface
interface AnotherInterface

class MyClass : MyInterface, AnotherInterface
```

### Adding method to interfaces

We can add methods to interfaces:

```kotlin
interface MyInterface {
  fun method()
}
```

Then, when we implement the interface inside a class, we have to give an implementation of interface's methods as in Java:

```kotlin
interface MyInterface {
  fun method()
}

class MyClass : MyInterface {
  override fun method() {
    println("This is my implementation")
  }
}
```

> [!NOTE] override
>
> It is mandatory to explicit that we are overriding a method.

### Default implementation

In Kotlin it is possible to provide a default implementation for methods inside interfaces:

```kotlin
interface MyInterface {
  fun method() {
    println("This is the default implementation")
  }
}

class MyClass : MyInterface
```

This will not prompt any error and works as well.

In Java it is possible to implement methods inside an interface, they have to be static, default or private. In Kotlin there isn't this limitation.

### Interfaces' variables

In Java variables inside interfaces have to be public static final. In Kotlin it is possible to put inside interfaces variables whose value can be modified or not, depending on whether they are `var` or `val`:

```kotlin
interface MyInterface {
  val value: String
  fun method() {
    println("This method is implemented by default")
    println("The value can change from one implementation to another.")
    println("For this implementation the value is: $value")
  }
}

class MyClass : MyInterface {
  override val value: String
    get() = "MyClass' value"
}
```

In this case we override only the getter because we choose a `val`, but in case of `var` we have to override setter too.

> [!WARNING]
>
> It is impossible to assign a default value to variables inside an interface. We have to provide its value for each implementation!

### `is` and `as` operators: type checking and smart casting

In Kotlin to cast objects is a bit different from other languages:

```kotlin
fun main() {
  var something = SpecificObject()
  var variable: GeneralObject = something as GeneralObject
}
```

The example is a simple upcasting, that normally is implicit in Kotlin.

In Kotlin it is possible to verify if a more general object is also an instance of another more specific using the `is` operator. This means that we can downcast safely:

```kotlin
interface General

class Specific : General
class OtherSpecific : General

fun main() {
  checkType(Specific())        // It will return true
  checkType(OtherSpecific())   // It will return false
}

fun checkType(general: General): Boolean {
  if (general is Specific) {
    println("$general is a Specific")
    return true
  } else {
    println("$general is something other")
    return false
  }
}
```

So since we are able to safely downcast, we can work with methods of the subclass, operation that in Java was unsafe.

> [!NOTE]
>
> Passing two specific classes to `checkType()` they are casted to `General` implicitly, without using `as` keyword, because it is an upcast. It works like Java.

For example, we can do something like this:

```kotlin
interface General

class Specific : General {
  fun specificMethod() {
    println("This is a specific method")
  }
}
class OtherSpecific : General

fun main() {
  checkType(Specific())        // It will return true
  checkType(OtherSpecific())   // It will return false
}

fun checkType(general: General): Boolean {
  if (general is Specific) {
    println("$general is a Specific")
    (general as Specific).specificMethod()
    return true
  } else {
    println("$general is something other")
    return false
  }
}
```

Here comes into play Kotlin's smart casting. Since we can make sure that `general` is an instance of an object of `Specific` type using `is` keyword, the compiler allow us to use `Specific`'s methods on `general` without casting explicitly:

```kotlin
interface General

class Specific : General {
  fun specificMethod() {
    println("This is a specific method")
  }
}
class OtherSpecific : General

fun main() {
  checkType(Specific())        // It will return true
  checkType(OtherSpecific())   // It will return false
}

fun checkType(general: General): Boolean {
  if (general is Specific) {
    println("$general is a Specific")
    general.specificMethod()    // This doesn't produce any error
    return true
  } else {
    println("$general is something other")
    return false
  }
}
```

As we can see, inside the `if` block we are allowed to treat `general` in the same way as a `Specific` instance.