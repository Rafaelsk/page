[← Back to Index](../index.md)

# Kotlin Cheat Sheet

## Language Overview

- Kotlin is a modern statically-typed language for the JVM (also compiles to JS & Native).
- Designed to be concise, safe, and interoperable with Java.
- Full type inference, null-safety, coroutines, and extension functions.

---

## Basics

```kotlin
val a: Int = 10    // immutable
var b: Int = 20    // mutable
val c = "Rafael"   // type inferred
```

---

## Functions

```kotlin
fun add(x: Int, y: Int): Int = x + y

fun greet(name: String = "Guest") {
    println("Hello, $name")
}
```

Named arguments & default values are built-in:

```kotlin
greet()
greet(name = "Valerie")
```

---

## Null Safety

```kotlin
val s: String? = null
println(s?.length)         // safe call
val len = s?.length ?: 0   // Elvis operator
```

Use `!!` to force unwrap (throws NPE if null):

```kotlin
val definitelyNotNull = s!!
```

---

## Control Flow

```kotlin
if (x > 0) {
  println("Positive")
} else {
  println("Non-positive")
}

val max = if (a > b) a else b

when (x) {
  0 -> "Zero"
  in 1..10 -> "Small"
  else -> "Large"
}
```

---

## Collections

```kotlin
val list = listOf(1, 2, 3)       // immutable
val mutable = mutableListOf(4)  // mutable

val set = setOf("a", "b")
val map = mapOf("a" to 1, "b" to 2)

for (item in list) println(item)

list.map { it * 2 }
    .filter { it > 3 }
    .forEach(::println)
```

---

## Classes

```kotlin
class User(val name: String, var age: Int)

val u = User("Rafael", 42)
println(u.name)
```

### Data Classes

```kotlin
data class Point(val x: Int, val y: Int)

val p1 = Point(1, 2)
val p2 = p1.copy(y = 3)
```

Auto-generates: `equals()`, `hashCode()`, `toString()`, `copy()`, and destructuring.

---

## Object-Oriented Features

```kotlin
open class Animal {
  open fun speak() = println("...")
}

class Dog : Animal() {
  override fun speak() = println("Woof!")
}
```

Singletons:

```kotlin
object Logger {
  fun log(msg: String) = println(msg)
}
```

---

## Interfaces & Extensions

```kotlin
interface Clickable {
  fun click()
}

fun String.shout(): String = this.uppercase() + "!"
println("hello".shout()) // HELLO!
```

---

## Lambdas & Higher-Order Functions

```kotlin
val double = { x: Int -> x * 2 }
fun apply(x: Int, f: (Int) -> Int) = f(x)
apply(10, double)
```

Use trailing lambda syntax:

```kotlin
list.filter { it > 2 }.map { it * 2 }
```

---

## Coroutines (Kotlinx)

```kotlin
suspend fun fetchData(): String {
    delay(1000)
    return "Done"
}

GlobalScope.launch {
    val result = fetchData()
    println(result)
}
```

Use `runBlocking {}` in entry point for testing or CLI tools.

---

## Destructuring & Ranges

```kotlin
val (x, y) = Point(1, 2)

for (i in 1..5) println(i)        // inclusive
for (i in 5 downTo 1 step 2) println(i)
```

---

## Smart Casts & Type Checks

```kotlin
fun handle(x: Any) {
  if (x is String) {
    println(x.uppercase()) // smart-cast to String
  }
}
```

---

## Sealed Classes

```kotlin
sealed class Result
data class Success(val data: String) : Result()
data class Failure(val error: Throwable) : Result()

fun handle(result: Result) = when (result) {
  is Success -> println("Yay: ${result.data}")
  is Failure -> println("Oops: ${result.error}")
}
```

---

## Companion Objects

```kotlin
class Utils {
  companion object {
    fun doSomething() = println("Static-like method")
  }
}

Utils.doSomething()
```

---

## Type Aliases & Inline Classes

```kotlin
typealias UserId = String

@JvmInline
value class Email(val value: String)
```

---

## Best Practices

- Prefer `val` over `var`.
- Avoid nullable types unless needed.
- Favor immutability, use `data` and `sealed` classes.
- Prefer pure functions and expression style.
- Use `apply`, `let`, `run`, `also`, `with` wisely.

---

Add-ons: want to cover testing (`kotest`, `mockk`), Android-specific stuff, or DSL building later?
