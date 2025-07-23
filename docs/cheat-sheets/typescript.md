[← Back to Index](../index.md)

# TypeScript Cheat Sheet

## Type System Overview

TypeScript adds static typing to JavaScript. It uses *structural typing* (duck typing by structure) and has advanced type inference.

```ts
let x: number = 42;
let name: string = "Rafael";
let isReady: boolean = true;
```

---

## Type Aliases & Interfaces

```ts
type UserId = string | number;

interface User {
  id: UserId;
  name: string;
  isAdmin?: boolean; // optional field
}
```

`interface` is preferred for object shapes; `type` is more flexible and supports unions/intersections.

---

## Functions

```ts
function greet(name: string = "Guest"): string {
  return `Hello, ${name}`;
}

const square = (x: number): number => x * x;
```

Function parameters can be:
- Optional: `x?: number`
- With default values
- Rest: `(...args: number[])`

---

## Union & Intersection Types

```ts
type Admin = { role: "admin"; permissions: string[] };
type Guest = { role: "guest"; expires: Date };

type User = Admin | Guest;
type Auditable = User & { createdAt: Date };
```

---

## Type Narrowing & Guards

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed());
  }
}
```

Also supported: `in`, `instanceof`, custom type guards.

---

## Generics

```ts
function identity<T>(value: T): T {
  return value;
}

function map<T, U>(list: T[], fn: (item: T) => U): U[] {
  return list.map(fn);
}
```

---

## Enums

```ts
enum Status {
  Pending,
  InProgress,
  Done,
}

const s: Status = Status.Pending;
```

Use `const enum` for performance in simple cases.

---

## Utility Types

```ts
type PartialUser = Partial<User>;
type ReadonlyUser = Readonly<User>;
type PickName = Pick<User, "name">;
type WithoutId = Omit<User, "id">;
```

---

## Type Assertions (Not Casting)

```ts
const input = document.getElementById("myInput") as HTMLInputElement;
input.value = "Hello";
```

---

## Modules & Imports

```ts
// Exporting
export function add(a: number, b: number): number {
  return a + b;
}

// Importing
import { add } from "./math";
```

Enable `"module": "ESNext"` or `"CommonJS"` in `tsconfig.json`.

---

## `tsconfig.json` Essentials

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "strict": true,
    "esModuleInterop": true,
    "baseUrl": "./src",
    "paths": {
      "@utils/*": ["utils/*"]
    }
  }
}
```

---

## Non-Null Assertion

```ts
const val: string | undefined = getValue();
console.log(val!.length); // Assume not undefined (use carefully)
```

---

## Type Inference & Control

```ts
const x = 42;               // inferred as number
let y: unknown = getData(); // explicitly unknown
```

Use `unknown` instead of `any` when possible — it forces you to narrow before using.

---

## Advanced Types

- **Mapped Types**

```ts
type Flags<T> = {
  [K in keyof T]: boolean;
};
```

- **Conditional Types**

```ts
type IsString<T> = T extends string ? true : false;
```

- **Template Literal Types**

```ts
type CSSLength = `${number}px` | `${number}%` | "auto";
```

---

## Asynchronous Code

```ts
async function fetchData(): Promise<Data> {
  const res = await fetch("/api/data");
  return await res.json();
}
```

---

## Type-Safe API Results

```ts
type ApiResult<T> = { status: "ok"; data: T } | { status: "error"; error: string };

function handleResult<T>(result: ApiResult<T>) {
  if (result.status === "ok") {
    return result.data;
  } else {
    throw new Error(result.error);
  }
}
```

---

## Best Practices

- Prefer `type` for flexibility and union composition
- Use `unknown` over `any`
- Keep `strict: true` in `tsconfig.json`
- Avoid too much clever typing — readability matters
- Use `zod` or `io-ts` for runtime validation

