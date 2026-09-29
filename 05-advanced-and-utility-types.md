# 05 - Advanced and Utility Types

## Type Operators

Type operators build new types from existing ones:

```ts
type User = { id: number; name: string; active: boolean };
type UserKeys = keyof User;         // "id" | "name" | "active"
type UserName = User["name"];       // string
type UserValue = User[keyof User];  // number | string | boolean
```

`typeof` in a type position captures the type of a value:

```ts
const defaults = { retries: 3, mode: "safe" } as const;
type Defaults = typeof defaults;
```

## `typeof` and `satisfies`

The `satisfies` operator checks that an expression conforms to a target type while retaining the expression's more specific inferred type.

```ts
type RouteName = "home" | "profile";
const routes = {
  home: "/",
  profile: "/me",
} satisfies Record<RouteName, string>;
```

This checks missing or misspelled keys without replacing the inferred property types with the broader target type. It is a compile-time check, not runtime validation.

## Mapped Types

Mapped types transform the properties of another type. Built-in utilities such as `Partial<T>` use this mechanism.

```ts
type ReadonlyFields<T> = {
  readonly [K in keyof T]: T[K];
};

type User = { id: number; name: string };
type ReadonlyUser = ReadonlyFields<User>;
```

Mapped types can add/remove modifiers with `readonly` and `?`, including `-readonly` and `-?`.

## Conditional Types and `infer`

A conditional type chooses a type based on assignability. `infer` introduces a type variable to extract part of a matched type.

```ts
type ElementOf<T> = T extends readonly (infer Item)[] ? Item : T;

type Name = ElementOf<readonly string[]>; // string
type Value = ElementOf<number>;           // number
```

When a conditional type has a naked type parameter on the left of `extends`, it distributes over unions:

```ts
type ToArray<T> = T extends unknown ? T[] : never;
type Items = ToArray<string | number>; // string[] | number[]
```

Wrapping both sides in tuples prevents distribution: `[T] extends [U] ? X : Y`.

## Template Literal Types

Template literal types create string types from literal unions:

```ts
type EventName = "open" | "close";
type HandlerName = `on${Capitalize<EventName>}`; // "onOpen" | "onClose"
```

Useful for deriving event names, property names, and small string-based APIs. They do not replace runtime parsing or validation.

## Common Utility Types

| Utility | Purpose |
| ------- | ------- |
| `Partial<T>` | Make all properties optional |
| `Required<T>` | Make all properties required |
| `Readonly<T>` | Make properties readonly |
| `Pick<T, K>` | Keep selected properties |
| `Omit<T, K>` | Remove selected properties |
| `Record<K, V>` | Map keys `K` to values `V` |
| `Exclude<T, U>` | Remove union members assignable to `U` |
| `Extract<T, U>` | Keep union members assignable to `U` |
| `NonNullable<T>` | Remove `null` and `undefined` |
| `ReturnType<F>` | Get a function type's return type |
| `Parameters<F>` | Get a function type's parameter tuple |

Utility types can make intent concise, but be cautious when chaining many transformations: the resulting contract should remain understandable at the call site.

## Branded Types

Structural typing considers two values compatible if their shapes match. A brand can distinguish otherwise identical values at compile time:

```ts
type UserId = number & { readonly __brand: "UserId" };
type OrderId = number & { readonly __brand: "OrderId" };

function loadUser(id: UserId): void {}
```

Creating a branded value still requires a trusted constructor or assertion. A brand is erased at runtime and is not validation.

## Key Points

- `keyof`, indexed access, and `typeof` derive types from existing shapes and values.
- `satisfies` validates compatibility while retaining a narrower inferred type.
- Conditional types select types; distributivity can transform each union member independently.
- Utility types are built-in transformations, not runtime functions.
- Prefer simple public types; advanced type machinery should earn its complexity.