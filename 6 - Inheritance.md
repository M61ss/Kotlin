# Inheritance <!-- omit from toc -->

- [`open` keyword](#open-keyword)
- [Extend a class](#extend-a-class)
- [Extention functions and extention properties](#extention-functions-and-extention-properties)

> [Return to full index](README.md)

### `open` keyword

In Kotlin classes, methods and properties are closed by default. That means that they cannot be inherited from or extended.
\
For example, to give the possibility to extend a class, we have to add the `open` keyword before `class` in the class definition:

```kotlin
open class MyClass
```

### Extend a class

If we want to override superclass' properties or methods, we have to mark also them with `open` keyword:

```kotlin
open class MySuperclass {
  open val property: String = "Superclass property"

  open fun method() {
    println("This is the superclass' method")
  }
}

class MySubclass : MySuperclass() {
  override val property: String
    get() = "Subclass property"

  override fun method() {
    println("This is the subclass' method")
  }
}
```

It works at the same way for `var`.

> [!NOTE]
>
> We have to put at the right of the colon the constructor of the superclass.

> [!IMPORTANT] super
>
> Like Java, if we want to refer to a superclass' method, we have to use `super` keyword.

> [!IMPORTANT] Visibility
>
> If we want to give to a subclass access to a property of a superclass, but we don't want to leave it accessible from any of subclass' instances, we can set its visibilty to protected:
>
> ```kotlin
> open class MySuperclass {
>   protected open val property: String = "Superclass property"
> }
>
> class MySubclass : MySuperclass() {
>   override val property: String
>     get() = "Subclass property"
> }
>
> fun main() {
>   var variable = MySubclass()
>   variable.property     // It causes a compile error!
> }
> ```
>
> Thus the only way to access `property` is from superclass' instances.

### Extention functions and extention properties

We can create functions outside a class, but give to this the possibility to use those as an their own method:

```kotlin
class MyClass(val property: String)

fun MyClass.extentionFunction() {
  println("Extention function behaviour")
}

fun main() {
  MyClass("Property").extentionFunction()
}
```

With the same logic we can create extention properties as well:

```kotlin
class MyClass(val property: String)

val MyClass.extentionProperty: String
  get() = "Extetion Property"

fun main() {
  println(MyClass("Property").extentionProperty)
}
```

As we can observe, it is mandatory to define the property with its getter and setter.

> [!NOTE] 
>
> To use extention functions and properties the class doesn't need to be `open`.