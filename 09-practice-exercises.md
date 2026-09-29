# 09 - Practice Exercises

Try each exercise before reading its solution. Compile with `strict` enabled and explain why the type is safe, not just how to silence an error.

## Exercise 1: Safe Lookup

Write a function that returns an item from a readonly array by index and accurately represents the possibility that the index is missing.

<details>
<summary>Solution</summary>

```ts
function at<T>(items: readonly T[], index: number): T | undefined {
  return items[index];
}
```

The result may be absent. `noUncheckedIndexedAccess` also makes this possibility visible for indexed reads.
</details>

## Exercise 2: Discriminated Result

Define a generic success/error result and write a function that returns the success value or error message without assertions.

<details>
<summary>Solution</summary>

```ts
type Result<T> =
  | { ok: true; value: T }
  | { ok: false; error: string };

function unwrapMessage<T>(result: Result<T>): string {
  return result.ok ? String(result.value) : result.error;
}
```

The literal `ok` property narrows the union. In real code, avoid converting an arbitrary value to a message unless that is the intended behavior.
</details>

## Exercise 3: Typed Property Getter

Write `getProperty` so a valid key for an object returns the type of that specific property.

<details>
<summary>Solution</summary>

```ts
function getProperty<T, K extends keyof T>(value: T, key: K): T[K] {
  return value[key];
}
```
</details>

## Exercise 4: Exhaustive State Handling

Given a union of `"queued" | "running" | "done"`, return a user-facing label and ensure a new state forces the function to be updated.

<details>
<summary>Solution</summary>

```ts
type JobState = "queued" | "running" | "done";

function unreachable(value: never): never {
  throw new Error(`Unhandled state: ${value}`);
}

function label(state: JobState): string {
  switch (state) {
    case "queued": return "Waiting";
    case "running": return "In progress";
    case "done": return "Complete";
    default: return unreachable(state);
  }
}
```
</details>

## Exercise 5: Runtime Validation

Write a type guard for an object with `id: number` and `name: string`. It should reject `null`, arrays, and objects with incorrectly typed fields.

<details>
<summary>Solution</summary>

```ts
type Person = { id: number; name: string };

function isPerson(value: unknown): value is Person {
  if (typeof value !== "object" || value === null || Array.isArray(value)) {
    return false;
  }

  const candidate = value as Record<string, unknown>;
  return typeof candidate.id === "number" && typeof candidate.name === "string";
}
```

The assertion is limited to viewing an already-checked non-array object as a string-keyed record of unknown values. Each field is then checked at runtime.
</details>

## Exercise 6: Partial Update

Given a `User` type, define a patch type allowing any subset of its fields, and a function signature that accepts the user and patch.

<details>
<summary>Solution</summary>

```ts
type User = { id: number; name: string; active: boolean };
type UserPatch = Partial<Omit<User, "id">>;

function updateUser(user: User, patch: UserPatch): User {
  return { ...user, ...patch };
}
```

This type allows an empty patch. Add a runtime rule if at least one field must be supplied.
</details>

## Exercise 7: Explain the Type

What is the type of `value` below, and how can it be used safely?

```ts
const value: unknown = JSON.parse(text);
```

<details>
<summary>Solution</summary>

It is `unknown`. No specific operation is safe until a runtime check narrows it or a trusted validator establishes a type. `JSON.parse` itself returns `any` in the standard library, so assigning to `unknown` contains that unsafety at the boundary.
</details>

## Discussion Prompts

- When would you choose an overload rather than a union parameter?
- Why does `readonly` not guarantee that an object can never change?
- What does `satisfies` check that a type assertion does not?
- When is a generic unnecessary complexity?
- Which project command actually performs type checking?