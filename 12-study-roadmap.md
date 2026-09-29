# 12 - Study Roadmap

## Phase 1: Core Language

Read [01 - Foundations](01-typescript-foundations.md), [02 - Functions and Objects](02-functions-and-objects.md), and [03 - Unions and Narrowing](03-unions-and-narrowing.md). Practice annotating API inputs and making nullable cases explicit.

**Checkpoint:** Explain `any`, `unknown`, `never`, structural typing, and a discriminated union without relying on memorized definitions.

## Phase 2: Reusable Type Models

Read [04 - Generics](04-generics.md) and [05 - Advanced and Utility Types](05-advanced-and-utility-types.md). Recreate a typed property getter, a `Result<T>` type, and a small mapped type.

**Checkpoint:** Explain which type relationship a generic preserves and why `satisfies` differs from `as`.

## Phase 3: Application Context

Read [06 - Classes and Modules](06-classes-and-modules.md), [07 - Compiler Configuration and Tooling](07-compiler-configuration-and-tooling.md), and [08 - Async and Runtime Boundaries](08-async-errors-and-runtime-boundaries.md). Inspect a real project's `package.json` scripts and `tsconfig.json`.

**Checkpoint:** Identify whether the project's build runs the type checker and where external values are validated.

## Phase 4: Practice and Interview

Complete [09 - Practice Exercises](09-practice-exercises.md) before reading each answer. Then review [10 - Interview Questions](10-interview-questions.md) and [11 - Quick Revision](11-quick-revision.md).

**Checkpoint:** Solve a small modeling problem aloud, state assumptions, and explain both compile-time guarantees and runtime gaps.

## One-Week Schedule

| Day | Focus |
| --- | ----- |
| 1 | Foundations and strict null checking |
| 2 | Functions, objects, and arrays |
| 3 | Unions, narrowing, and exhaustive checks |
| 4 | Generics and type operators |
| 5 | Utility types, classes, and modules |
| 6 | Compiler options, async code, and validation boundaries |
| 7 | Exercises, interview answers, and revision |

## Ongoing Practice

- Prefer strict settings in new code and resolve errors at their source.
- Review every assertion: what invariant makes it safe, and is that invariant checked at runtime?
- Keep domain types close to the concepts they represent; avoid building an elaborate type framework without a real use case.
- Use tests for runtime behavior and the compiler for static contracts; neither replaces the other.