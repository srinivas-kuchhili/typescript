# 03 - Union Types and Narrowing

## Union and Intersection Types

A union `A | B` means a value may be either type. Code can use only what is safe for every member until the value is narrowed. An intersection `A & B` combines requirements from both types.

```ts
type Identifier = string | number;
type Timestamped = { createdAt: Date };
type Entity = { id: string } & Timestamped;
```

## Narrowing With Runtime Checks

TypeScript follows JavaScript checks such as `typeof`, `instanceof`, equality, and property presence to narrow a union.

```ts
function describe(value: string | number): string {
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  return value.toFixed(2);
}
```

For `null`, compare explicitly. `typeof null` is `"object"` in JavaScript.

## Discriminated Unions

Give each variant a shared literal property with a different value. Checking the discriminant narrows the rest of the shape.

```ts
type Result<T> =
  | { status: "success"; data: T }
  | { status: "error"; message: string };

function render(result: Result<string>): string {
  if (result.status === "success") {
    return result.data;
  }
  return result.message;
}
```

This pattern models mutually exclusive states clearly, such as loading/success/error, payment methods, or commands with different payloads.

## Exhaustiveness and `never`

When every union member has been handled, the remaining value should be `never`. A helper makes omissions fail at compile time when a new variant is added.

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number };

function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "rectangle":
      return shape.width * shape.height;
    default:
      return assertNever(shape);
  }
}
```

## User-Defined Type Guards

A type predicate lets a function communicate a runtime check to the compiler. The implementation must genuinely uphold the claim.

```ts
type Cat = { kind: "cat"; meows: boolean };
type Dog = { kind: "dog"; barks: boolean };

function isCat(pet: Cat | Dog): pet is Cat {
  return pet.kind === "cat";
}
```

An incorrect predicate is effectively an unsafe assertion and can make later code unsound.

## `in`, `instanceof`, and Equality

- `"field" in value` checks whether a property exists on an object or its prototype chain.
- `value instanceof SomeClass` checks a runtime prototype relationship.
- Comparing a literal discriminant often gives the clearest narrowing.
- For plain JSON data, avoid relying on class identity because parsed objects are not class instances.

## Optional Chaining and Nullish Coalescing

```ts
const city = user.address?.city;
const label = user.nickname ?? "Anonymous";
```

`?.` stops property access or a call when the left side is `null` or `undefined`. `??` uses the fallback only for `null` or `undefined`; unlike `||`, it preserves valid falsy values such as `0` and `""`.

## Key Points

- A union exposes only operations safe for all members until narrowed.
- Discriminated unions make state-dependent fields explicit.
- `never` is useful for checking exhaustive handling.
- Type guards are runtime code and must be correct.
- Optional chaining handles access; it does not validate external data.