# Activity-25-SOLID-PRINCIPLES-in-Angular''

## Applying SOLID Principles in Angular

The SOLID principles are a set of guidelines that help developers write clean, flexible, and maintainable code. These principles are not limited to object-oriented programming and can be effectively implemented in Angular, which is built on TypeScript.

Let’s explore how each SOLID principle can be applied in Angular and how it improves code quality, along with examples and real-life use cases.

## 1. Single Responsibility Principle (SRP)

Explanation: Each class or module should focus on one task or responsibility. This helps avoid overcomplicating components and promotes cleaner, modular code.

In Angular, this means separating concerns between components and services. Components should focus on rendering the view, while services handle business logic or data retrieval.

Example:
``` typescript
// INCORRECT: Component doing too much, violating SRP
@Component({
  selector: 'app-user-profile',
  template: `<div>{{ userData.name }}</div>`,
})
export class UserProfileComponent {
  userData: any;

  constructor() {
    this.userData = this.getUserData();
  }

  getUserData() {
    // Directly fetching data inside the component (bad practice)
    return { name: 'John Doe', age: 30 };
  }
}

// CORRECT: Component delegates data handling to a service
@Component({
  selector: 'app-user-profile',
  template: `<div>{{ userData.name }}</div>`,
})
export class UserProfileComponent {
  userData: any;

  constructor(private userService: UserService) {
    this.userData = this.userService.getUserData();
  }
}

@Injectable({ providedIn: 'root' })
export class UserService {
  getUserData() {
    // Business logic in the service
    return { name: 'John Doe', age: 30 };
  }
}
```
Real-World Use Case:

In an online store application, the ProductComponent should only display product data, while a ProductService handles the backend API calls to fetch product information.

## 


## 2. Open/Closed Principle (OCP)

Explanation: A class or function should be open to extensions (adding new features) but closed to modifications. This prevents changes to existing, stable code when adding new functionality.

Example:
``` typescript
// INCORRECT: Altering the class directly to add a new condition
export class DiscountService {
  calculateDiscount(price: number, isLoyalCustomer: boolean) {
    if (isLoyalCustomer) {
      return price * 0.9; // Loyalty discount
    } else {
      return price;
    }
  }
}

// CORRECT: Introducing new behavior through an interface or abstraction
export interface DiscountStrategy {
  calculate(price: number): number;
}

export class LoyaltyDiscount implements DiscountStrategy {
  calculate(price: number) {
    return price * 0.9;
  }
}

export class NoDiscount implements DiscountStrategy {
  calculate(price: number) {
    return price;
  }
}

export class DiscountService {
  constructor(private strategy: DiscountStrategy) {}

  applyDiscount(price: number) {
    return this.strategy.calculate(price);
  }
}
```
Real-World Use Case:

When designing a shopping cart system, different types of discounts (e.g., promotional discounts, member discounts) may be added in the future. Using OCP, you can create new discount types without altering existing functionality.

##

## 3. Liskov Substitution Principle (LSP)

Explanation: Any instance of a subclass should be able to replace its superclass without altering the correctness of the program. In other words, derived classes should maintain the behavior of the parent class.

Example:
```typescript
// INCORRECT: Violating LSP by changing behavior
export class Bird {
  fly() {
    return 'Flying';
  }
}

export class Penguin extends Bird {
  fly() {
    throw new Error('Penguins can’t fly');
  }
}

// CORRECT: Following LSP by using separate behaviors
export interface Flyable {
  fly(): string;
}

export class Sparrow implements Flyable {
  fly() {
    return 'Flying';
  }
}

export class Penguin {
  swim() {
    return 'Swimming';
  }
}
```

Real-World Use Case:

In a warehouse management system, you might have a base class for Item. Subclasses like PerishableItem or NonPerishableItem should all behave consistently as Item objects and be substitutable in the system without unexpected errors.

##

## 4. Interface Segregation Principle (ISP)

Explanation: It’s better to have multiple, smaller, specific interfaces than one large, generic interface. This ensures that classes don’t implement methods they don’t need.

Example:
```typescript
// INCORRECT: A large interface forcing implementation of unused methods
export interface Machine {
  print(doc: Document): void;
  scan(doc: Document): void;
  fax(doc: Document): void;
}

export class Printer implements Machine {
  print(doc: Document) {
    console.log('Printing...');
  }

  scan(doc: Document) {
    throw new Error('Scan feature not available');
  }

  fax(doc: Document) {
    throw new Error('Fax feature not available');
  }
}

// CORRECT: Use smaller, focused interfaces
export interface Printer {
  print(doc: Document): void;
}

export interface Scanner {
  scan(doc: Document): void;
}

export class BasicPrinter implements Printer {
  print(doc: Document) {
    console.log('Printing...');
  }
}
```

Real-World Use Case:

In an e-commerce application, you can separate interfaces for payment methods like OnlinePayment and OfflinePayment. This way, online payment systems don’t need to implement offline methods like cash handling.

##

## 5. Dependency Inversion Principle (DIP)

Explanation: High-level modules should not depend on low-level modules; both should rely on abstractions. This principle is easily implemented in Angular thanks to dependency injection (DI).

Example:
```typescript
// INCORRECT: Hard dependency on a low-level class
export class UserComponent {
  constructor() {
    const userService = new UserService(); // Tight coupling
    userService.getUserData();
  }
}

// CORRECT: Dependency injected via constructor
export class UserComponent {
  constructor(private userService: UserService) {}

  getUserData() {
    this.userService.getUserData();
  }
}

@Injectable({ providedIn: 'root' })
export class UserService {
  getUserData() {
    return { name: 'John Doe', age: 30 };
  }
}

```

Real-World Use Case:

For a weather app, the WeatherComponent should depend on an abstract weather service, not directly on a low-level API implementation. This allows you to easily swap out the API service in the future without changing the component.

##


## Conclusion

Implementing SOLID principles in Angular promotes clean, modular, and maintainable code. It encourages separation of concerns, reduces tight coupling, and allows for flexibility in future feature development. By following these principles, you can improve the quality of your applications and make them easier to maintain and scale.

