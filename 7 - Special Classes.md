# Special classes <!-- omit from toc -->

- [Object expressions](#object-expressions)
- [Companion objects](#companion-objects)
- [Object declaration](#object-declaration)
- [Enum classes](#enum-classes)
- [Sealed classes](#sealed-classes)
- [Data classes](#data-classes)

> [Return to full index](README.md)

### Object expressions

In Kotlin object expressions are similar to Java's anonymous classes. The syntax to create them is the following:

```kotlin
open class MyClass {
  open val property: String = "Class property"

  open fun method() {
    println("This is the class' method")
  }
}

fun main() {
  val objectExpression = object : MyClass {
    override val property: String
      get() = "Object expression property"

    override fun method() {
      println("This is the object expression's method")
    }
  }
}
```

The keyword `object` identifies the creation of an object expression.

> [!NOTE]
>
> The class must be open and everything that we override must be open

### Companion objects

It is possible to define the primary constructor as private, but thanks companion we still be able to create instances of the object:

```kotlin
class MyClass private constructor(val property: String) {
  companion object {
    fun create() = Entity("Value of the property")
  }
}

fun main() {
  val instance = MyClass("Value")             // It causes a compile error
  val instance = MyClass.Companion.create()   // It works
}
```

> [!NOTE]
> Since we use Kotlin, we can omit `.Companion`, but if we try to run this code in Java, we have to explicate it as shown above.

We have also the possibility to rename the companion:

```kotlin
class MyClass private constructor(val property: String) {
  companion object CustomCompanionName {
    fun create() = Entity("Value of the property")
  }
}

fun main() {
  val instance = MyClass.CustomCompanionName.create()
}
```

A companion can contain variables as well and they can be accessed outside the class: 

```kotlin
class MyClass private constructor(val property: String) {
  companion object Companion {
    const val companionProperty = "Companion property"

    fun create() = Entity("Value of the property")
  }
}

fun main() {
  val instance = MyClass.Companion.create()
  instance.companionProperty
}
```

A companion can also implement an interface like a normal class:

```kotlin
interface MyInterface {
  fun interfaceMethod(): String
}

class MyClass private constructor(val property: String) {
  companion object Companion :  {
    override fun interfaceMethod(): String {
      return "Some value"
    }

    fun create() = Entity(interfaceMethod())
  }
}

fun main() {
  var variable = instance.interfaceMethod()
}
```

Then we can access outside the class the method implemented inside the `companion object` block. It is very important to notice that we didn't create any instance of the class, anyway we are able to use the method. Companion objects corresponde to Java's static methods.

### Object declaration

In Kotlin it is possible to create objects that have only one instance. They are also called singleton objects. It is impossible to instantiate them, but we can call their methods:

```kotlin
object SingletonClass {
  val property: String = "Property"
  var otherProperty: String = "Other property"

  fun method() {
    println("Some behaviour")
  }
}

fun main() {
  SingletonClass.method()
}
```

These objects are similar to Java static concept, but they are more flexible thanks some additional feature. For example, they can extend classes or implement interfaces and they are thread-safe.

> [!IMPORTANT] Thread safety
> These objects are thread-safe by definition.

### Enum classes

Enums are very similar to those of Java:

```kotlin
enum class MyEnum {
  FIRST_TYPE,
  SECOND_TYPE,
  THIRD_TYPE
}

class MyClass(type: MyEnum) {
  val property: String

  init {
    property = when(type) {
      MyEnum.FIRST_TYPE -> "First Type"
      MyEnum.SECOND_TYPE -> "Second Type"
      MyEnum.THIRD_TYPE -> "Third Type"
    }
  }
}

fun main() {
  var instance = MyClass(MyEnum.SECOND_TYPE)
}
```

Enum classes have all the `name` default property. We can use that to create functions inside them:

```kotlin
enum class MyEnum {
  FIRST_TYPE,
  SECOND_TYPE,
  THIRD_TYPE;

  fun method() = name.toLowerCase.capitalize()
}

class MyClass(type: MyEnum) {
  val property: String

  init {
    property = when(type) {
      MyEnum.FIRST_TYPE -> type.name          // It corresponds to "FIRST_TYPE"
      MyEnum.SECOND_TYPE -> type.method()     // It corresponds to "Second_Type"
      MyEnum.THIRD_TYPE -> "Third Type"
    }
  }
}

fun main() {
  var instance = MyClass(MyEnum.SECOND_TYPE)
}
```

### Sealed classes

A basic sealed class has the following definition:

```kotlin
sealed class SealedClass()
```

Sealed classes are not directly instantiable. Inside a sealed class we can create data classes or object classes that all can extend the sealed one, but they are instantiable,:

```kotlin
sealed class SealedClass() {
  object SomeObject {
    val property = "Static property"
  }
  data class First(val property: String) : SealedClass()
  data class Second(val otherProperty: String) : SealedClass()
}

fun main() {
  val firstInstance = SealedClass.First()
  var secondInstance = SealedClass.Second()
  val staticProperty = StaticClass.SomeObject.property
}
```

> [!NOTE] data
> `data` keyword can be removed and all works as well (see [Data classes](#data-classes)).

> [!NOTE] Smart casting
> The compiler supports the smart casting also working with sealed classes as well.

> [!NOTE] 
> Since `SomeObject` doesn't extend `SealedClass`, it is impossible to access it from outside the sealed class.

Sealed classes are powerful also if combined with enum classes. Here a complete example:

```kotlin
enum class MyEnum {
  FIRST_TYPE,
  SECOND_TYPE,
  THIRD_TYPE
}

object Generator {
  fun create(type: MyEnum) : SealedClass {
    return when(type) {
      MyEnum.FIRST_TYPE -> SealedClass.Third("Property")
      MyEnum.SECOND_TYPE -> SealedClass.Second("Other property")
      MyEnum.THIRD_TYPE -> SealedClass.First("Some property", "Another property")
    }
  }
}

sealed class SealedClass() {
  data class First(val property: String) : SealedClass()
  data class Second(val otherProperty: String) : SealedClass()
  data class Third(val someProperty: String, var anotherProperty) : SealedClass()
}

fun main() {
  var selectedInstance = Generator.create(THIRD_TYPE)
}
```

Then we can obtained a decisional structure for generating different objects of the same type similarly we can do using interfaces, but this way is very well organized and fast as well.

### Data classes

Data classes are immutable objects that generate automatically `equals()`, `hashCode()`, `toString()` and `copy()` methods:

```kotlin
data class MyDataClass(val property: String)
```

> [!NOTE] copy()
>
> The `copy()` method allow us to generate a copy of an existent instance, but also to create a copy with one or more properties changed:
>
> ```kotlin
> data class MyDataClass(val property: String, val otherProperty: String)
> 
> fun main() {
>   val firstInstance = MyDataClass("Property", "Other Property")
>   val secondInstance = firstInstance.copy()
>   val thirdInstance = firstInstance.copy(property = "Different Property") 
> }
> ```

Data classes don't have any implementation, but we have the possibility to override their methods.
