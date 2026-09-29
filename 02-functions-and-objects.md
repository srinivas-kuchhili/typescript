# 02 - Functions, Objects, and Arrays

## Function Types

Parameter and return types describe a function contract. Return types can often be inferred, but explicit returns on exported functions make API changes easier to spot.

```ts
function formatName(first: string, last: string): string {
  return `${first} ${last}`;
}

type Comparator<T> = (left: T, right: T) => number;
const compareNumbers: Comparator<number> = (left, right) => left - right;
```

Function types describe the callable shape. Parameter names in a function type are labels, not part of compatibility.

## Optional and Default Parameters

An optional parameter may be omitted and is treated as possibly `undefined` inside the function. Required parameters cannot follow optional parameters. A default initializer also makes a parameter optional to callers.

```ts
function greet(name: string, prefix = "Hello"): string {
  return `${prefix}, ${name}`;
}

function findUser(id: number, includeInactive?: boolean): void {
  // includeInactive is boolean | undefined
}
```

## Rest Parameters and Overloads

Rest parameters collect zero or more values into an array. Overloads express several public call signatures for one implementation.

```ts
function combine(value: string, separator: string): string;
function combine(value: number, separator: number): number;
function combine(value: string | number, separator: string | number) {
  if (typeof value === "string" && typeof separator === "string") {
    return value + separator;
  }
  if (typeof value === "number" && typeof separator === "number") {
    return value + separator;
  }
  throw new Error("Arguments must have matching types");
}
```

Callers see the overload signatures, not the implementation signature. Prefer a union parameter when the possible input and output relationship is simple; overloads help when call forms have meaningfully different contracts.

## Object Shapes and Optional Properties

Use an object type to document required and optional fields:

```ts
type User = {
  id: number;
  name: string;
  nickname?: string;
  readonly createdAt: Date;
};
```

`nickname?: string` means the property may be absent. With `exactOptionalPropertyTypes`, assigning explicit `undefined` is treated differently from omitting the property unless `undefined` is included in its type.

`readonly` prevents assignment through that typed reference. It does not deeply freeze the object at runtime.

## Index Signatures and `Record`

An index signature describes an object with keys of a certain kind and values of a certain type:

```ts
type Scores = Record<string, number>;
const scores: Scores = { Ada: 98, Lin: 91 };
```

Use a specific property model when keys are known. A broad index signature can make misspelled properties harder to catch and may weaken the meaning of named fields.

## Arrays, Tuples, and Readonly Data

```ts
const tags: string[] = ["ts", "types"];
const result: [number, string] = [200, "ok"];
const coordinates: readonly [number, number] = [10, 20];
```

Arrays represent variable-length collections of one element type. Tuples represent fixed positional structures. `readonly T[]` and readonly tuples prevent writes through that reference, not mutation through every alias.

## Type Aliases and Interfaces

Both can describe object shapes and are often interchangeable for ordinary object contracts.

```ts
interface Identified {
  id: number;
}

type Named = {
  name: string;
};

type UserSummary = Identified & Named;
```

Interfaces support declaration merging and `extends`. Type aliases can name unions, primitives, tuples, and mapped or conditional types. Prefer consistent local conventions over treating one as universally better.

## Key Points

- Optional parameters and properties introduce possible absence.
- A default parameter is optional for callers but has a defined value in the function body.
- Overload signatures, not the implementation signature, define the public call contract.
- `readonly` is a compile-time restriction, not runtime immutability.
- Use tuples for positional fixed-size data and arrays for collections.