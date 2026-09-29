# 04 - Generics

## Why Generics?

Generics preserve relationships between types. They let a function or data structure work with many types without giving up information to `any`.

```ts
function identity<T>(value: T): T {
  return value;
}

const id = identity("item"); // T inferred as "item"
```

The type parameter connects the argument and return types. A version returning `unknown` would accept any input but would not preserve which type was supplied.

## Generic Functions and Inference

Usually let the compiler infer type arguments from values. Specify one explicitly when inference is impossible or the desired type is wider than the inferred literal.

```ts
function first<T>(items: readonly T[]): T | undefined {
  return items[0];
}

const firstName = first(["Ada", "Lin"]); // string | undefined
```

## Constraints

`extends` constrains a type parameter to the members an implementation needs.

```ts
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}

getLength("TypeScript");
getLength([1, 2, 3]);
```

The constraint does not mean the generic is exactly that shape; `T` may have additional members, which the function can preserve in its return type.

## `keyof` and Indexed Access Types

`keyof T` is a union of the known property keys of `T`. `T[K]` looks up the type at key `K`.

```ts
function getProperty<T, K extends keyof T>(value: T, key: K): T[K] {
  return value[key];
}

const user = { id: 12, name: "Ada" };
const userName = getProperty(user, "name"); // string
```

This pattern keeps the key and returned property type connected.

## Generic Interfaces and Defaults

```ts
interface ApiResponse<TData, TError = Error> {
  data: TData;
  error?: TError;
}

const response: ApiResponse<{ id: number }> = {
  data: { id: 1 },
};
```

Type parameter defaults reduce repetition when a common choice exists. A required type parameter cannot follow an optional/defaulted one.

## Generic Constraints vs Union Parameters

Use a union when the function accepts a fixed set of alternatives and does not need to preserve a relationship between input and output. Use a generic when the caller's specific type should flow through the contract.

```ts
function wrap<T>(value: T): { value: T } {
  return { value };
}
```

## Common Generic Mistakes

- Adding `<T>` when the type parameter is not used to connect any types.
- Using `any` inside a generic and erasing the relationship the generic was meant to preserve.
- Over-constraining a type parameter to one concrete type.
- Assuming `T extends Base` means every `T` is exactly `Base`.
- Creating very elaborate generic APIs when a straightforward concrete type is easier to use.

## Interview One-Liner

> Generics let an API work across types while preserving relationships, such as returning the same type it receives or tying a key to the corresponding property value type.