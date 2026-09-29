# 08 - Async Code, Errors, and Runtime Boundaries

## Promises and `async` Functions

An `async` function always returns a `Promise`. Its declared return type describes the value inside that promise.

```ts
async function loadCount(): Promise<number> {
  return 3;
}
```

Use `await` to observe a promise's fulfillment value. Rejections still need appropriate error handling; `Promise<T>` does not encode a checked rejection type.

## Error Handling

JavaScript allows throwing any value. With `useUnknownInCatchVariables` (included by `strict` in modern TypeScript), a caught error is `unknown` and should be narrowed.

```ts
try {
  await loadCount();
} catch (error: unknown) {
  if (error instanceof Error) {
    console.error(error.message);
  } else {
    console.error("A non-Error value was thrown");
  }
}
```

Do not assume every rejection is an `Error` unless the code boundary guarantees it.

## Runtime Data Validation

Type annotations do not validate network responses, environment variables, local storage, or parsed JSON. Parse external values as `unknown`, then validate before treating them as a domain type.

```ts
type User = { id: number; name: string };

function isUser(value: unknown): value is User {
  if (typeof value !== "object" || value === null) return false;
  if (!("id" in value) || !("name" in value)) return false;

  return typeof value.id === "number" && typeof value.name === "string";
}
```

For larger schemas, a runtime validation library can avoid duplicating complex checks. The key is that validation must execute at runtime; a cast is not enough.

## DOM Types and Nullability

Browser APIs frequently return nullable values:

```ts
const element = document.querySelector("#app");
if (element) {
  element.textContent = "Ready";
}
```

`querySelector` returns an element or `null`. A non-null assertion may hide a missing element that would otherwise fail at runtime. Use a guard or handle the absence deliberately.

## Promise Combinators

- `Promise.all` fulfills when all inputs fulfill and rejects when an input rejects; result positions preserve input order.
- `Promise.allSettled` reports each input's fulfilled or rejected outcome.
- `Promise.race` settles with the first settled input.
- `Promise.any` fulfills with the first fulfilled input and rejects with `AggregateError` if all reject.

These are runtime behaviors; TypeScript infers useful result types but does not change the semantics.

## Key Points

- `async` functions return promises even when returning a plain value.
- Promise types describe fulfillment, not a typed rejection channel.
- Catch values and external data should be treated as `unknown` until checked.
- DOM and other platform APIs retain their runtime nullability.
- Compile-time types cannot substitute for runtime schema validation.