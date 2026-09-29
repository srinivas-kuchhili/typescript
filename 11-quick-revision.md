# 11 - Quick Revision

## Core Ideas

- TypeScript checks JavaScript before runtime; types are generally erased.
- Inference derives types from values and context.
- `unknown` requires checks; `any` opts out of them.
- With strict null checks, model possible absence as `T | null` or `T | undefined`.
- Object compatibility is primarily structural.
- Assertions do not convert or validate values.

## Narrowing

```ts
if (typeof value === "string") { /* value: string */ }
if (value instanceof Date) { /* value: Date */ }
if ("kind" in value) { /* property-presence narrowing */ }
```

Use a literal discriminant for variants and `never` to check exhaustive handling.

## Generics and Type Operators

```ts
function get<T, K extends keyof T>(value: T, key: K): T[K] {
  return value[key];
}
```

- `keyof T`: known keys of `T`.
- `T[K]`: type at key `K`.
- `typeof value`: type of a value in a type position.
- `satisfies Shape`: check compatibility while preserving inferred detail.
- `T extends U ? X : Y`: conditional type.

## Common Utilities

`Partial<T>` | `Required<T>` | `Readonly<T>` | `Pick<T, K>` | `Omit<T, K>` | `Record<K, V>` | `Exclude<T, U>` | `Extract<T, U>` | `NonNullable<T>` | `ReturnType<F>` | `Parameters<F>`

## Frequently Confused

| Pair | Difference |
| ---- | ---------- |
| `void` / `never` | No useful return value / no normal completion |
| `any` / `unknown` | Unchecked escape hatch / safe value requiring narrowing |
| `const` / `readonly` | Binding cannot be reassigned / property cannot be assigned through this reference |
| `type` / `interface` | Both model shapes; aliases cover unions and other expressions, interfaces can merge |
| `extends` / `implements` | Inheritance or interface extension / class is checked against a contract |
| `target` / `lib` | Emitted syntax level / available type declarations |
| `as` / `satisfies` | Assert compiler belief / check compatibility while preserving inferred type |
| `??` / `||` | Fallback for nullish values / fallback for all falsy values |

## Runtime Boundary Checklist

1. Treat parsed or external data as `unknown`.
2. Validate the shape at runtime.
3. Handle missing values and malformed input.
4. Use the validated domain type only after the check.

## Before an Interview

- Explain compile-time types separately from JavaScript runtime behavior.
- Be ready to narrow `unknown` without an assertion.
- Model asynchronous success and failure deliberately.
- Explain why a generic preserves a relationship.
- Describe the actual type-check command and module setup in your project.