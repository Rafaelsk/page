# Java Cheat Sheet

## JVM & Compilation Model

- Java code is compiled into **bytecode** by `javac` and executed on the **JVM**.
- Strongly typed, statically compiled language with runtime polymorphism.

```bash
javac Main.java
java Main
```

---

## Class Structure

```java
public class MyClass {
    private final int value;

    public MyClass(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

### Access Modifiers

- `private`: accessible only within the class
- `protected`: within the package and subclasses
- `public`: accessible from everywhere
- package-private (default): accessible within the same package

---

## Main Method

```java
public class App {
    public static void main(String[] args) {
        System.out.println("Hello, world");
    }
}
```

---

## Primitive Types

```java
int x = 42;
double d = 3.14;
char c = 'a';
boolean flag = true;
```

Objects vs primitives: use wrapper classes (`Integer`, `Double`, etc.) for collections and generics.

---

## Collections Framework

```java
import java.util.*;

List<String> list = new ArrayList<>();
list.add("hello");

Set<Integer> set = new HashSet<>();
Map<String, Integer> map = new HashMap<>();
```

### Common Iteration

```java
for (String s : list) {
    System.out.println(s);
}

list.forEach(System.out::println);
```

---

## Generics

```java
public class Box<T> {
    private T value;

    public void set(T value) { this.value = value; }
    public T get() { return value; }
}
```

Bounded types:

```java
<T extends Number> void print(T t) { ... }
```

---

## Interfaces & Abstract Classes

```java
public interface Drawable {
    void draw();
}
```

```java
public abstract class Shape {
    abstract void draw();
}
```

Java 8+ interfaces can have `default` and `static` methods.

---

## Lambdas & Functional Interfaces (Java 8+)

```java
List<Integer> list = List.of(1, 2, 3);
list.forEach(x -> System.out.println(x));
```

Functional interface example:

```java
@FunctionalInterface
interface Operation {
    int apply(int x, int y);
}
```

---

## Streams API (Java 8+)

```java
List<String> names = List.of("Rafa", "Marie", "Scala");

List<String> filtered = names.stream()
    .filter(s -> s.length() > 4)
    .map(String::toUpperCase)
    .sorted()
    .toList();
```

Terminal ops: `forEach`, `collect`, `reduce`, `count`  
Intermediate ops: `filter`, `map`, `flatMap`, `distinct`

---

## Exceptions

```java
try {
    riskyOp();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    cleanup();
}
```

Custom exception:

```java
public class MyException extends Exception {
    public MyException(String msg) { super(msg); }
}
```

---

## Records (Java 16+)

Concise immutable data classes:

```java
public record User(String name, int age) {}
```

Automatically generates constructor, getters, `equals`, `hashCode`, `toString`.

---

## Enums

```java
public enum Status {
    PENDING, IN_PROGRESS, DONE
}
```

Enums can contain fields, constructors, and methods.

---

## Threads & Concurrency

```java
new Thread(() -> System.out.println("Running")).start();
```

### Executor Service

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> doWork());
executor.shutdown();
```

---

## Annotations

```java
@Override
public String toString() {
    return "value";
}
```

Custom annotation:

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyMarker { }
```

---

## Module System (Java 9+)

```java
module my.module {
    exports com.my.package;
    requires other.module;
}
```

Useful for strong encapsulation in large systems.

---

## Best Practices

- Favor composition over inheritance.
- Use `final` where possible.
- Prefer interfaces over concrete classes for APIs.
- Use immutability and `Optional<T>` to model absence.
- Avoid `null` unless working with legacy APIs.
- Use `var` for local inference (Java 10+).
- Don’t overuse checked exceptions.

---

Want to add: JUnit, Maven/Gradle, Spring basics, or annotations like `@Autowired`, `@Bean`? Let me know.
