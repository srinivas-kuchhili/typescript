# 01 - TypeScript Foundations

## What TypeScript Adds

TypeScript is a statically checked superset of JavaScript. The compiler analyzes types and can emit JavaScript, but ordinary type annotations are erased. A TypeScript type cannot inspect or validate a value after the program is running.

```ts
const greeting: string = "Hello";
console.log(greeting);
```

The emitted JavaScript contains the value and `console.log`, not `: string`.

> Interview one-liner: TypeScript adds compile-time checking and editor tooling to JavaScript; it does not add runtime type enforcement by itself.

## Type Annotations and Inference

An annotation states the intended type. Inference lets the compiler derive a type from an initializer or context.

```ts
let count = 3;             // number
const title = "Guide";     // "Guide"
const names: string[] = ["Ada", "Lin"];

function double(value: number) {
  return value * 2;        // return type inferred as number
}
```

Use annotations at meaningful boundaries, such as public function parameters, exported APIs, and values initialized later. Avoid repeating obvious types when inference is clear.

`let` variables usually widen to a general type such as `string`; `const` bindings can retain a narrower literal type because the binding cannot be reassigned. `as const` preserves literal values recursively and makes object properties readonly:

```ts
const settings = { mode: "dark", retries: 3 } as const;
// { readonly mode: "dark'; readonly retries: 3 }
```

## Common Types

| Type | Meaning | Example |
| ---- | ------- | ------- |
| `string`, `number`, `boolean` | Primitive values | `let enabled: boolean = true` |
| `T[]` or `Array<T>` | An array of `T` | `const ids: number[] = [1, 2]` |
| `[A, B]` | Tuple with positions | `const point: [number, number] = [4, 7]` |
| `object` | Any non-primitive value; often too broad | Prefer a specific object shape |
| `unknown` | A value whose type must be checked before use | Safer boundary for uncertain data |
| `any` | Disables checking for the value | Avoid unless intentionally opting out |
| `void` | A function's result is not used / no useful result | `function log(): void {}` |
| `never` | A value that cannot occur | Exhaustive branches and throwing functions |

JavaScript has one `number` type for integers and floating-point values. TypeScript has no separate built-in integer type.

## `any` vs `unknown`

Both can hold values of any origin, but `unknown` requires narrowing before use.

```ts
function printLength(value: unknown): void {
  if (typeof value === "string") {
    console.log(value.length);
  }
}
```

With `any`, operations are allowed without checks and errors can spread through the program. Use `unknown` for values from JSON, external APIs, or other untrusted sources until they have been checked.

## `null`, `undefined`, and Strict Null Checks

With `strictNullChecks: true`, `null` and `undefined` are distinct types and cannot be used where a definite value is required. Model optionality explicitly:

```ts
function findName(id: number): string | undefined {
  return id === 1 ? "Ada" : undefined;
}

const name = findName(2);
if (name !== undefined) {
  console.log(name.toUpperCase());
}
```

The non-null assertion `value!` tells the compiler to trust that a value is present. It performs no runtime check, so use it only when the invariant is guaranteed elsewhere.

## Structural Typing

TypeScript primarily compares object types by their members (structural compatibility), rather than by declared names.

```ts
type HasId = { id: number };

function showId(value: HasId): number {
  return value.id;
}

const user = { id: 7, name: "Ada" };
showId(user); // compatible: it has the required member
```

Fresh object literals receive additional excess-property checking, which catches likely spelling mistakes. That check does not turn TypeScript into a nominal type system.

## Type Errors and Assertions

Assertions such as `value as User` do not convert or validate a value. They only change what the compiler believes. Prefer a real check or a type guard when data needs validation.

```ts
const raw: unknown = JSON.parse('{"id":7}');
// `raw as User` would not prove that an id exists or is numeric.
```

> Interview one-liner: Inference is usually preferable for obvious local values; explicit types are valuable at boundaries where they document and check a contract.

## Key Points

- Types are erased from standard JavaScript output.
- `unknown` is the safe top type; `any` opts out of checking.
- `strictNullChecks` makes absence visible in the type system.
- Structural compatibility depends primarily on members.
- Assertions are not runtime conversions or validation.