# TypeScript - Study and Interview Guide

A topic-wise guide to TypeScript fundamentals, practical type modeling, compiler behavior, and interview preparation. Examples assume a modern TypeScript version and use strict checking unless noted otherwise.

TypeScript checks JavaScript code before it runs. Its types are erased from emitted JavaScript, so types help prevent mistakes during development but do not validate untrusted data at runtime.

## Table of Contents

| # | Topic | File |
| -- | ----- | ---- |
| 01 | TypeScript foundations, annotations, and inference | [01-typescript-foundations.md](01-typescript-foundations.md) |
| 02 | Functions, objects, arrays, and properties | [02-functions-and-objects.md](02-functions-and-objects.md) |
| 03 | Union types, narrowing, and discriminated unions | [03-unions-and-narrowing.md](03-unions-and-narrowing.md) |
| 04 | Generics and reusable typed code | [04-generics.md](04-generics.md) |
| 05 | Advanced types and utility types | [05-advanced-and-utility-types.md](05-advanced-and-utility-types.md) |
| 06 | Classes, interfaces, and modules | [06-classes-and-modules.md](06-classes-and-modules.md) |
| 07 | `tsconfig`, compiler behavior, and tooling | [07-compiler-configuration-and-tooling.md](07-compiler-configuration-and-tooling.md) |
| 08 | Async code, errors, and runtime boundaries | [08-async-errors-and-runtime-boundaries.md](08-async-errors-and-runtime-boundaries.md) |
| 09 | Practice exercises with solutions | [09-practice-exercises.md](09-practice-exercises.md) |
| 10 | Interview questions and answers | [10-interview-questions.md](10-interview-questions.md) |
| 11 | Quick revision sheet | [11-quick-revision.md](11-quick-revision.md) |
| 12 | Study roadmap | [12-study-roadmap.md](12-study-roadmap.md) |

## Suggested Study Order

```text
Foundations    -> 01 -> 02 -> 03
Reusable types -> 04 -> 05
Application    -> 06 -> 07 -> 08
Practice       -> 09
Interview      -> 10 -> 11
```

## How to Study

- Predict the inferred type before hovering over a value or running the compiler.
- Turn on `strict` and resolve errors by improving the model, not by adding `any` or assertions everywhere.
- For each example, ask whether the type is only a compile-time guarantee or whether runtime validation is also needed.
- Practice explaining the tradeoff: a useful type should make invalid states difficult to represent without making ordinary code awkward.

## Conventions

- Examples use `type` and `interface` where each is a natural fit; neither is presented as universally superior.
- Interview callouts are concise starting points. Expand them with a concrete example in an interview.
- JavaScript behavior still applies at runtime. TypeScript does not change how values execute.