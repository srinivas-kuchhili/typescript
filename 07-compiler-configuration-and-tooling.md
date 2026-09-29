# 07 - Compiler Configuration and Tooling

## The TypeScript Compiler

`tsc` can type-check a project and emit JavaScript. Build tools and test runners may also transpile TypeScript, sometimes without performing full type checking. Know which command in a project actually runs the checker.

```sh
npx tsc --noEmit
npx tsc --project tsconfig.json
```

Do not assume a successful bundler build proves that all TypeScript errors were checked unless its configuration says so.

## `tsconfig.json`

A project configuration defines compiler options and which files are included. A useful baseline for a new application often includes strict checking and explicit module settings appropriate to its runtime.

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noEmit": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts"]
}
```

This is an example, not a universal configuration. Browser applications, Node.js packages, bundlers, and libraries can need different module and emit settings.

## Important Options

| Option | Effect |
| ------ | ------ |
| `strict` | Enables a family of stronger checks, including strict null checks and strict function checks |
| `noEmit` | Type-check without writing output files |
| `target` | Controls the JavaScript language level emitted when emitting |
| `module` | Chooses the module syntax/semantics for emitted JavaScript |
| `moduleResolution` | Defines how import specifiers are resolved |
| `lib` | Selects available platform API declarations, such as DOM types |
| `types` | Limits automatically included `@types` packages |
| `noUncheckedIndexedAccess` | Adds `undefined` to unchecked indexed reads |
| `exactOptionalPropertyTypes` | Distinguishes omitted optional properties from explicit `undefined` assignments |
| `declaration` | Emits `.d.ts` declarations for library consumers |

## `target`, `lib`, and Runtime Support

`target` affects emitted syntax, not which APIs exist at runtime. `lib` supplies type declarations for APIs such as `Promise` or DOM types; it does not polyfill them. A program can type-check while calling an API unavailable in its actual runtime.

Use runtime support policy and polyfills separately from the compiler's declarations.

## Declaration Files

`.d.ts` files describe the types of JavaScript code without containing implementations. They are commonly provided by libraries or `@types` packages. Declaration files are trusted by the compiler, so an inaccurate declaration can make type checking misleading.

## Type Checking and Linting

The compiler checks type relationships. A linter can enforce conventions or catch patterns the type checker does not target. Formatting, linting, testing, and type checking are related but distinct checks; a project may run them as separate scripts.

## Key Points

- Check the project's actual scripts to learn whether the build performs type checking.
- `target` does not add runtime APIs; `lib` does not polyfill APIs.
- Module settings must match the runtime or bundler.
- Strict options expose assumptions early and make nullability explicit.
- `.d.ts` files are contracts trusted by the checker.