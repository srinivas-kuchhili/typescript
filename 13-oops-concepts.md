# 13 - Object-Oriented Programming Concepts in TypeScript

This chapter covers the OOP concepts most often expected in interviews, with definitions and TypeScript examples. OOP organizes code around objects that combine state and behavior. TypeScript supports classes, interfaces, inheritance, and access modifiers, while its type annotations are still erased at runtime.

## 1. Class and Object

A **class** describes how to create objects: their state, constructor, and behavior. An **object** is a concrete instance of a class.

```ts
class User {
  constructor(
    public readonly id: number,
    public name: string,
  ) {}

  greet(): string {
    return `Hello, ${this.name}`;
  }
}

const user = new User(1, "Ada");
console.log(user.greet());
```

The constructor parameter modifiers are TypeScript parameter properties. They declare and initialize instance fields. `readonly` prevents reassignment through this typed reference after initialization; it does not freeze the object at runtime.

## 2. Encapsulation

**Encapsulation** keeps an object's data and the operations on that data together, while controlling how callers can access or change its state. The goal is to protect invariants, not merely to hide every field.

```ts
class BankAccount {
  #balanceCents = 0;

  deposit(amountCents: number): void {
    if (!Number.isInteger(amountCents) || amountCents <= 0) {
      throw new Error("Deposit must be a positive whole number of cents");
    }
    this.#balanceCents += amountCents;
  }

  get balanceCents(): number {
    return this.#balanceCents;
  }
}

const account = new BankAccount();
account.deposit(500);
console.log(account.balanceCents);
```

The private field prevents callers from changing the balance directly. The public method is the controlled operation that maintains the account's rule.

### TypeScript Access Modifiers

| Modifier | Meaning |
| -------- | ------- |
| `public` | Accessible from anywhere; the default visibility |
| `protected` | Accessible in the class and its subclasses |
| `private` | Restricted by TypeScript to the declaring class |
| `#field` | JavaScript runtime-private field |
| `readonly` | Prevents reassignment through the typed reference after initialization |

TypeScript's `private` is primarily a compile-time restriction. Use JavaScript `#private` fields when runtime privacy is required. Neither visibility option automatically prevents mutation of objects returned by public methods.

## 3. Abstraction

**Abstraction** presents the essential operations of a component while hiding implementation details. Callers depend on what an object can do rather than how it does it.

An interface describes a contract without providing implementation:

```ts
interface PaymentMethod {
  charge(amountCents: number): string;
}

class CardPayment implements PaymentMethod {
  constructor(private readonly lastFourDigits: string) {}

  charge(amountCents: number): string {
    return `Charged ${amountCents} cents to card ending in ${this.lastFourDigits}`;
  }
}
```

An abstract class can define both required operations and shared implementation:

```ts
abstract class Report {
  constructor(protected readonly title: string) {}

  abstract render(): string;

  print(): void {
    console.log(this.render());
  }
}

class TextReport extends Report {
  override render(): string {
    return `Report: ${this.title}`;
  }
}
```

An abstract class cannot be instantiated directly. Use an interface for a capability contract without shared runtime behavior; use an abstract class when related implementations genuinely share state or behavior.

## 4. Inheritance

**Inheritance** lets a derived class extend a base class, reuse its implementation, and specialize behavior. It represents an "is-a" relationship when the subtype can correctly stand in for the base type.

```ts
class Animal {
  constructor(public readonly name: string) {}

  speak(): string {
    return "Some sound";
  }
}

class Dog extends Animal {
  override speak(): string {
    return "Woof";
  }
}

const dog = new Dog("Milo");
console.log(dog.name, dog.speak());
```

`extends` inherits the base class implementation. The `override` keyword makes the intent explicit and, with `noImplicitOverride`, helps catch accidental changes to inherited members.

Inheritance is useful when the relationship is real and stable. Avoid using it only to share a few lines of code; composition often produces more flexible designs.

## 5. Polymorphism

**Polymorphism** means code can work through a shared contract while different implementations provide different behavior. The caller uses the same operation without needing to know the concrete class.

```ts
class WalletPayment implements PaymentMethod {
  charge(amountCents: number): string {
    return `Charged ${amountCents} cents from wallet`;
  }
}

class Checkout {
  constructor(private readonly paymentMethod: PaymentMethod) {}

  complete(amountCents: number): string {
    return this.paymentMethod.charge(amountCents);
  }
}

const cardCheckout = new Checkout(new CardPayment("4242"));
const walletCheckout = new Checkout(new WalletPayment());

console.log(cardCheckout.complete(1200));
console.log(walletCheckout.complete(1200));
```

`Checkout` works with any object that satisfies `PaymentMethod`. The implementation chosen at runtime determines which `charge` behavior runs. This is commonly called **runtime polymorphism** or **dynamic dispatch**.

TypeScript is structurally typed: a class does not have to explicitly say `implements PaymentMethod` to be assignable if it has the required compatible members.

### Overriding vs Overloading

- **Overriding** replaces or specializes an inherited method in a subclass. It is used for runtime polymorphism.
- **Overloading** gives one function or method several type-checked call signatures. It is resolved by the compiler; it does not create multiple runtime implementations.

```ts
class Formatter {
  format(value: number): string;
  format(value: Date): string;
  format(value: number | Date): string {
    return value instanceof Date ? value.toISOString() : value.toFixed(2);
  }
}
```

The overload signatures are visible to callers. One implementation must handle every listed input form.

## 6. Composition and Object Relationships

**Composition** builds a larger object by giving it other objects to use. It is often preferable to inheritance when behavior can be supplied as a replaceable dependency.

The `Checkout` example uses composition: it has a `PaymentMethod`, rather than inheriting from a card or wallet payment class.

| Relationship | Meaning | Example |
| ------------ | ------- | ------- |
| Association | Objects know about or use one another | A teacher works with students |
| Aggregation | A whole refers to parts that can exist independently | A team has players who can move to another team |
| Composition | A whole owns parts whose lifecycle is tied to it by the design | An order owns its line items |

These labels describe design and lifecycle intent. A TypeScript reference alone does not enforce aggregation or composition ownership.

## 7. Static Members

A **static member** belongs to the class itself rather than to each instance. It is useful for behavior or data associated with the type as a whole.

```ts
class IdGenerator {
  private static nextId = 1;

  static create(): number {
    return IdGenerator.nextId++;
  }
}

const firstId = IdGenerator.create();
```

Static methods do not have an instance `this`. Avoid using mutable static state for data that should be isolated per instance or per request.

## 8. Interfaces vs Abstract Classes

| Interface | Abstract class |
| --------- | -------------- |
| Describes a shape or capability | Provides a shared class implementation and contract |
| Has no runtime implementation | Exists as a JavaScript class at runtime |
| A class can implement multiple interfaces | A class can extend only one class |
| Cannot contain instance state or constructor logic | Can contain fields, constructors, and implemented methods |

Both can be useful together: an abstract class may implement an interface, while callers depend on the interface type.

## 9. SOLID Principles

SOLID is a set of design guidelines for making object-oriented systems easier to change. They are principles, not rules that require one class per method.

| Principle | Definition | Practical example |
| --------- | ---------- | ----------------- |
| **S - Single Responsibility** | A module should have one focused responsibility or reason to change. | Keep invoice calculation separate from invoice printing. |
| **O - Open/Closed** | Prefer extending behavior without repeatedly modifying stable code. | Add a new `PaymentMethod` implementation instead of adding another `if` branch to `Checkout`. |
| **L - Liskov Substitution** | A subtype should be usable wherever its base contract is expected without breaking that contract. | Every `PaymentMethod` must honor the meaning and guarantees of `charge`. |
| **I - Interface Segregation** | Prefer small, focused contracts over forcing clients to implement unused operations. | Split `PrinterScanner` into `Printable` and `Scannable` when clients need only one capability. |
| **D - Dependency Inversion** | High-level policy should depend on abstractions, not concrete low-level details. | `Checkout` accepts `PaymentMethod`, not a concrete `CardPayment`. |

Example of focused interfaces:

```ts
interface Printable {
  print(document: string): void;
}

interface Scannable {
  scan(): string;
}

class PrintOnlyDevice implements Printable {
  print(document: string): void {
    console.log(document);
  }
}
```

## 10. Common Interview Comparisons

### Encapsulation vs Abstraction

Encapsulation controls access to internal state and protects invariants. Abstraction exposes a simpler contract while hiding implementation details. A class can use both at once.

### Inheritance vs Composition

Inheritance models a subtype relationship and reuses a base implementation. Composition assembles behavior from collaborators. Prefer composition when the relationship is "uses" or when behaviors should be replaceable independently.

### Overloading vs Overriding

Overloading provides multiple compile-time call signatures for one implementation. Overriding specializes an inherited method and selects behavior at runtime.

### Interface vs Implementation

An interface defines the operations a consumer can rely on. An implementation supplies the concrete behavior. Depending on an interface lets an implementation be replaced without changing its consumer.

## Interview Summary

> The four common OOP pillars are encapsulation, abstraction, inheritance, and polymorphism. In TypeScript, classes provide runtime objects, interfaces and most types provide compile-time contracts, and structural typing means compatible behavior matters more than declared names. I use inheritance for genuine subtype relationships and composition for replaceable collaborators.

## Key Points

- Classes are blueprints; objects are instances with state and behavior.
- Encapsulation protects invariants through controlled operations.
- Abstraction hides implementation behind a focused contract.
- Inheritance should preserve the base type's promises.
- Polymorphism lets one consumer work with multiple implementations.
- Prefer composition when behavior should be swappable.
- TypeScript types do not provide runtime validation or ownership enforcement.