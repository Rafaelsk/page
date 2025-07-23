# C++ Cheat Sheet

## Evaluation & Execution Model

- **Pass by Value**: Arguments are copied. Modifying the parameter doesn't affect the original.
- **Pass by Reference**: Arguments are passed as aliases using `&`, enabling in-place mutation.
- **Const correctness**: Use `const` aggressively to enforce immutability and clarify intent.

```cpp
int square_by_value(int x);        // x is copied
int square_by_ref(const int& x);   // x is read-only reference
```

---

## RAII (Resource Acquisition Is Initialization)

- C++ idiom for managing resources (memory, files, locks).
- Constructor acquires, destructor releases.

```cpp
class File {
  std::ifstream f;
public:
  File(const std::string& path) : f(path) {}
  ~File() { f.close(); }
};
```

---

## Memory Management

```cpp
int* p = new int(42);
delete p;

int* arr = new int[10];
delete[] arr;
```

Avoid manual memory management by using smart pointers:

```cpp
#include <memory>

auto ptr = std::make_unique<MyClass>();  // Unique ownership
auto sp  = std::make_shared<MyClass>();  // Shared ownership
```

---

## Templates

Generic programming with compile-time type substitution.

```cpp
template <typename T>
T max(T a, T b) {
  return (a > b) ? a : b;
}
```

Type constraints via `std::enable_if` (C++11+) or `concepts` (C++20).

---

## Lambdas

Inline anonymous functions. Capture by value `[=]` or reference `[&]`.

```cpp
auto square = [](int x) { return x * x; };
```

Capture local variables:

```cpp
int base = 10;
auto add = [base](int x) { return x + base; };
```

---

## STL Containers (Standard Template Library)

### Sequence Containers
- `std::vector<T>` – dynamic array
- `std::list<T>` – doubly-linked list
- `std::deque<T>` – double-ended queue

### Associative Containers
- `std::set<T>`, `std::map<K,V>` – ordered
- `std::unordered_set<T>`, `std::unordered_map<K,V>` – hash-based

### Common Operations

```cpp
std::vector<int> v = {1, 2, 3};
v.push_back(4);
v[1] = 42;
v.size();       // 4
v.empty();      // false
```

---

## Iterators

STL algorithms use iterators, which act like generalized pointers.

```cpp
std::vector<int> v = {1, 2, 3};
std::for_each(v.begin(), v.end(), [](int x) {
  std::cout << x << " ";
});
```

Use range-based for-loop for readability:

```cpp
for (int x : v) {
  std::cout << x;
}
```

---

## Algorithms

Header: `<algorithm>`

```cpp
std::sort(v.begin(), v.end());
auto it = std::find(v.begin(), v.end(), 42);
std::transform(v.begin(), v.end(), v.begin(), [](int x) { return x * 2; });
```

---

## Classes and Inheritance

```cpp
class Animal {
protected:
  std::string name;
public:
  Animal(const std::string& n) : name(n) {}
  virtual void speak() const = 0;
};

class Dog : public Animal {
public:
  Dog(const std::string& n) : Animal(n) {}
  void speak() const override {
    std::cout << name << " says woof\n";
  }
};
```

---

## Constexpr & Inline (Modern C++)

- `constexpr`: evaluates expressions at compile time.
- `inline`: avoids multiple definition errors in headers.

```cpp
constexpr int square(int x) { return x * x; }
```

---

## Namespaces

Organize code and avoid collisions.

```cpp
namespace math {
  int square(int x) { return x * x; }
}

int x = math::square(5);
```

---

## Error Handling

Use exceptions for recoverable errors. Avoid overusing.

```cpp
try {
  // risky operation
} catch (const std::exception& e) {
  std::cerr << "Error: " << e.what();
}
```

---

## Compilation Model

```cpp
// header file (.h / .hpp)
#ifndef MYCLASS_H
#define MYCLASS_H
class MyClass { ... };
#endif

// source file (.cpp)
#include "MyClass.h"
```

Compile:
```bash
g++ main.cpp MyClass.cpp -o app -std=c++20 -O2
```

---

## Concurrency (C++11+)

```cpp
#include <thread>

void run() { /* work */ }
std::thread t(run);
t.join();
```

Mutexes:

```cpp
std::mutex m;
m.lock();
shared_state++;
m.unlock();

// or
std::lock_guard<std::mutex> guard(m);
```

---

## Smart Usage Patterns

- Prefer `std::vector` over `new[]`
- Prefer `unique_ptr` over raw pointers
- Prefer `const&` for heavy parameter types
- Avoid `#define`, use `constexpr` or `inline`
- Use `enum class` instead of raw enums

---

