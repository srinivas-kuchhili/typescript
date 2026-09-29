# 10 - TypeScript Interview Questions and Answers

Answers are concise starting points. In an interview, add a short example and connect the concept to a tradeoff you have encountered.

## Fundamentals

### 1. What is TypeScript?

TypeScript is a statically checked superset of JavaScript. It adds a type system and tooling, then its standard types are erased from emitted JavaScript. It does not provide runtime validation by itself.

### 2. Is TypeScript statically or dynamically typed?

It provides static type checking during development, while JavaScript values still follow JavaScript's runtime behavior. Types are not generally enforced after compilation.

### 3. What is type inference?

Inference is the compiler deriving types from initializers, return expressions, and context. It reduces redundant annotations while still checking how values are used.

### 4. What is the difference between `any` and `unknown`?

Both can represent values of any origin. `any` disables checking for operations on the value; `unknown` requires narrowing before using it as a more specific type.

### 5. What does `never` mean?

`never` represents a value that cannot occur. It appears for functions that never return normally and helps enforce exhaustive handling of unions.

### 6. What does `void` mean?

It describes a function whose result is not meant to be used. It is not interchangeable with `never`: a `void` function can finish normally, while a `never` function cannot.

### 7. What is structural typing?

Compatibility is primarily determined by whether a value has the required members, rather than whether it was declared with a particular type name.

### 8. What does `strict` do?

It enables a family of stronger checks, including important nullability and function checks. It helps surface assumptions but does not prove runtime data is valid.

## Functions and Object Types

### 9. What is the difference between an optional property and a union with `undefined`?

`name?: string` means the property may be absent. `name: string | undefined` means the property is required but its value may be `undefined`. `exactOptionalPropertyTypes` further distinguishes omission from explicit `undefined` assignment.

### 10. What is the difference between `type` and `interface`?

Both can describe object shapes. Interfaces support declaration merging and extension; aliases can express unions, tuples, primitives, and more complex type expressions. Choose based on the shape and project convention.

### 11. What is the difference between `readonly` and `const`?

`const` prevents reassignment of a variable binding. `readonly` prevents assigning to a property through a typed reference. Neither automatically freezes nested values at runtime.

### 12. What are function overloads?

Overloads give callers multiple public call signatures backed by one implementation. The implementation must handle every overload, but callers cannot call it using only the broader implementation signature.

## Unions and Generics

### 13. What is a discriminated union?

A union whose variants share a literal property with distinct values. Checking that property narrows the other fields and models mutually exclusive states.

### 14. How do you make a switch exhaustive?

After handling each variant, pass the remaining value to a function accepting `never`. Adding a new union member then produces a type error until it is handled.

### 15. What are generics for?

Generics let code work across multiple types while preserving relationships, such as returning the same type passed in or tying an object key to its property's value type.

### 16. What does `extends` mean in a generic constraint?

It limits which types can be supplied and lets the implementation safely use members in the constraint. The type parameter can still be a more specific subtype with additional information.

### 17. What is `keyof`?

It produces a union of the known property keys of a type. Combined with indexed access `T[K]`, it can preserve the relationship between a valid key and its value type.

### 18. What is a conditional type?

A type expression that selects one result or another based on assignability, written `T extends U ? X : Y`. With a naked type parameter, it distributes over union members.

## Runtime and Tooling

### 19. Does a type assertion validate or convert data?

No. An assertion changes the compiler's belief and emits no validation. Use runtime checks or a schema validator for external data.

### 20. How do you type data from an API?

Treat the response as `unknown` at the trust boundary, validate its shape at runtime, and only then use a domain type. A compile-time annotation alone cannot guarantee the server sent that shape.

### 21. What is the difference between `target` and `lib`?

`target` controls emitted JavaScript syntax when the compiler emits. `lib` supplies declarations for available language and platform APIs. Neither polyfills runtime features.

### 22. Does transpiling TypeScript always type-check it?

No. Some transpilers remove TypeScript syntax without running the type checker. The project should have an explicit type-check command, commonly `tsc --noEmit` or a build script that invokes it.

### 23. What does `satisfies` do?

It checks that an expression conforms to a target type while retaining the expression's more specific inferred type. It is not a runtime check and is not the same as a type assertion.

### 24. How would you reduce unsafe types in a legacy codebase?

Enable stricter checks incrementally, prioritize untrusted boundaries and public APIs, replace `any` with `unknown` plus narrowing, and add tests where runtime behavior needs coverage. Avoid a broad rewrite that obscures risk.

## Scenario Questions

### 25. A property is optional. How should a function handle it?

Model the absence in the signature and use a guard, a deliberate default, or optional chaining. Do not use a non-null assertion unless another invariant truly guarantees presence.

### 26. A new state is added and several switches break. Is that a problem?

It is useful feedback if each switch must handle every state. Exhaustive errors reveal places where the behavior needs a decision rather than silently falling through.

### 27. A build passes but CI tests fail on a missing browser API. Why?

The compiler may have declarations for that API through `lib` or package types even though the runtime does not implement it. Type declarations do not polyfill or guarantee runtime support.

### 28. When would you choose a class over an interface and functions?

Use a class when instances need identity, encapsulated state, or lifecycle behavior. Use interfaces to describe contracts, and plain functions/objects when they express the behavior more directly.

## Interview Reminder

Clarify whether a question is about compile-time compatibility or runtime behavior. Mention nullability, untrusted input, inference, and emitted JavaScript when they affect the answer.