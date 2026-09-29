# 06 - Classes, Interfaces, and Modules

## Interfaces and Extension

Interfaces describe object contracts and can extend other interfaces:

```ts
interface Identifiable {
  id: string;
}

interface Account extends Identifiable {
  email: string;
}
```

Classes can implement interfaces to check that their public instance shape satisfies a contract. Interfaces do not exist at runtime.

## Classes and Access Modifiers

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  constructor(private readonly prefix: string) {}

  log(message: string): void {
    console.log(`${this.prefix}: ${message}`);
  }
}
```

- `public` is the default.
- `private` and `protected` are TypeScript access restrictions; JavaScript `#field` is runtime-private.
- `readonly` restricts reassignment through the typed instance after initialization.
- A class can implement an interface but does not inherit interface code.

Use classes when identity, lifecycle, or encapsulated behavior is useful. Plain objects and functions are often simpler for data and stateless behavior.

## `implements` and `extends`

`extends` inherits implementation and behavior from a class (or extends an interface contract). `implements` checks that a class conforms to a type; it does not add members or change how the class is implemented.

TypeScript is structurally typed for most compatibility checks, but classes with private or protected members have compatibility rules tied to their declarations.

## Static Members and Abstract Classes

Static members belong to the class constructor, not individual instances. An abstract class can provide shared implementation while requiring subclasses to implement abstract members. It cannot be instantiated directly.

Choose an interface for a contract without runtime behavior; choose an abstract class when shared state or implementation is actually needed.

## Modules

Files with top-level `import` or `export` are modules with their own scope. Modules are the standard way to share values and types:

```ts
export interface User {
  id: number;
}

export function getUserLabel(user: User): string {
  return `User ${user.id}`;
}
```

Import types with `import type` when an import is used only in type positions:

```ts
import type { User } from "./user.js";
```

The emitted module format and how module paths resolve depend on `tsconfig`, the runtime, and the package environment. Follow the project configuration instead of assuming every tool resolves imports identically.

## Enums and Literal Unions

Enums create a runtime construct in many configurations. A string literal union is often a lightweight alternative when only a finite set of values is needed:

```ts
type Status = "pending" | "complete" | "failed";
```

Enums are still useful when a project wants their specific runtime representation or established convention. Choose based on emitted JavaScript, interoperability, and team style.

## Key Points

- Interfaces and most types are erased; classes are runtime values.
- `implements` checks a contract but does not inject behavior.
- TypeScript `private` is not the same as JavaScript `#private`.
- Use modules for explicit file-level APIs and type-only imports where appropriate.
- Class-based and functional designs are both valid; choose the smaller fitting model.