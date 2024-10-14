# Solid-Principles-in-Angular


SOLID principles are a set of guidelines for writing clean, maintainable, and reusable code. These principles were introduced by Robert C. Martin in his book “Agile Software Development, Principles, Patterns, and Practices.”

## The acronym SOLID stands for:

### S: Single Responsibility Principle

### O: Open/Closed Principle

### L: Liskov Substitution Principle

### I: Interface Segregation Principle

### D: Dependency Inversion Principle

# SOLID Principles:

## 1. S - Single Responsibility Principle (SRP):

A class should have only one reason to change, meaning it should have only one responsibility or job. By adhering to this principle, code becomes more modular and easier to maintain.

## Example:

A class Invoice should only handle invoice-related operations. If you need to print or email an invoice, that responsibility should be handled by another class like InvoicePrinter or InvoiceEmailer.

A class should have one and only one reason to change.
```
// Violating SRP
class Invoice {
  generateInvoice() {
    // Generate invoice logic
  }

  printInvoice() {
    // Printing logic (violates SRP)
  }

  emailInvoice() {
    // Emailing logic (violates SRP)
  }
}

// Following SRP
class Invoice {
  generateInvoice() {
    // Generate invoice logic
  }
}

class InvoicePrinter {
  printInvoice(invoice: Invoice) {
    // Printing logic
  }
}

class InvoiceEmailer {
  emailInvoice(invoice: Invoice) {
    // Emailing logic
  }
}
```
In Angular, you might be tempted to write a single service or component that handles multiple concerns, such as managing state, making HTTP requests, and manipulating the DOM. This approach can make code harder to maintain. SRP encourages you to split responsibilities across multiple classes, services, or components.

## Real-World Use Case:

Imagine you're building a travel booking system. You could have a BookingService that handles fetching available trips, processing payments, sending confirmation emails, and managing user profiles. However, this would violate SRP. Instead, break down the responsibilities into services like TripService for handling trip data, PaymentService for managing payments, and EmailService for sending emails.

## 2. O - Open/Closed Principle (OCP):

Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means you can extend the behavior of a class without modifying its existing code, ensuring stability.

## Example:

You can add new functionality to a class by inheritance or composition without changing the existing code. For example, adding new discount types to an Invoice class by extending it with new classes.

Classes should be open for extension but closed for modification.
```
// Violating OCP
class Discount {
  applyDiscount(price: number, type: string): number {
    if (type === 'percent') {
      return price * 0.9; // 10% discount
    } else if (type === 'fixed') {
      return price - 10;  // $10 discount
    }
    return price;
  }
}

// Following OCP
interface Discount {
  applyDiscount(price: number): number;
}

class PercentageDiscount implements Discount {
  applyDiscount(price: number): number {
    return price * 0.9;
  }
}

class FixedDiscount implements Discount {
  applyDiscount(price: number): number {
    return price - 10;
  }
}

// Now we can add new discount types without modifying existing code
```
In Angular, this means you should be able to add new functionality without modifying existing code. By using inheritance or interfaces, you can extend the behavior of components or services while keeping the original code intact.

## Real-World Use Case:

In an e-commerce platform where you offer different discount types (percentage, fixed amount, or even seasonal discounts), OCP allows you to extend your system by simply creating new discount classes (like SeasonalDiscount) without modifying the core discount logic. This keeps your codebase clean and stable.

## 3. L - Liskov Substitution Principle (LSP):

Subtypes must be substitutable for their base types without altering the correctness of the program. This ensures that derived classes extend the functionality of a base class without changing its expected behavior.

## Example:

If a Rectangle class is a subtype of a Shape class, you should be able to replace Shape with Rectangle without affecting the program. If the subclass violates this, the principle is broken.

Subtypes must be substitutable for their base types.
```
// Violating LSP
class Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error('Penguins cannot fly');
  }
}

// Following LSP
class Bird {
  move() {
    console.log('Moving');
  }
}

class Penguin extends Bird {
  move() {
    console.log('Swimming');  // Override move, not fly
  }
}

let bird: Bird = new Penguin();
bird.move();  // Outputs "Swimming" without violating LSP
```
In Angular, this means that any derived class should be able to replace its parent class without breaking functionality. This principle ensures that inherited components or services can be used interchangeably with their base classes.

## Real-World Use Case:

Imagine you're developing a transportation system in Angular that handles different types of vehicles. You might have a Vehicle class with a move() method. A Car moves on roads, while a Boat moves on water. By ensuring the method move() is appropriately handled in derived classes like Car or Boat, you allow for LSP compliance. The system should be able to handle both without changes to the core logic.

## 4. I - Interface Segregation Principle (ISP):

Clients should not be forced to depend on interfaces they do not use. It is better to have multiple, smaller, specific interfaces rather than a large, general-purpose interface.

## Example:

Instead of having one large interface for a printer (e.g., IPrinter with Print, Scan, Fax methods), it is better to have separate interfaces like IPrinter, IScanner, and IFax, so a class can implement only what it needs.

Clients should not be forced to depend on interfaces they don't use.
```
// Violating ISP
interface IPrinter {
  print(): void;
  scan(): void;
  fax(): void;
}

class SimplePrinter implements IPrinter {
  print() {
    console.log('Printing...');
  }

  scan() {
    throw new Error('Scan not supported');
  }

  fax() {
    throw new Error('Fax not supported');
  }
}

// Following ISP
interface IPrint {
  print(): void;
}

interface IScan {
  scan(): void;
}

interface IFax {
  fax(): void;
}

class SimplePrinter implements IPrint {
  print() {
    console.log('Printing...');
  }
}

class AllInOnePrinter implements IPrint, IScan, IFax {
  print() {
    console.log('Printing...');
  }

  scan() {
    console.log('Scanning...');
  }

  fax() {
    console.log('Faxing...');
  }
}
```
In Angular, large interfaces can make your components or services dependent on methods they don’t need. ISP suggests breaking down large interfaces into smaller, more specific ones, allowing you to implement only what is required.

## Real-World Use Case:

Consider a dashboard in Angular where different user roles have access to different functionalities. By segregating interfaces, an admin might need an interface with methods like addUser(), removeUser(), generateReport(), while a regular user might only need an interface with viewReport(). By splitting the interfaces, you ensure each role only depends on what it actually uses.

## 5. D - Dependency Inversion Principle (DIP):

High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions. This promotes loose coupling and makes the system easier to maintain and test.

## Example:

Instead of a Report class depending directly on a Database class, it should depend on an interface like IDataStorage. This way, you can switch from a database to a file system without changing the Report class.

High-level modules should not depend on low-level modules. Both should depend on abstractions.
```
// Violating DIP
class MySQLDatabase {
  connect() {
    console.log('Connecting to MySQL database...');
  }
}

class UserService {
  private database: MySQLDatabase;

  constructor() {
    this.database = new MySQLDatabase();  // Tightly coupled
  }

  getUser() {
    this.database.connect();
    // Get user logic
  }
}

// Following DIP
interface IDatabase {
  connect(): void;
}

class MySQLDatabase implements IDatabase {
  connect() {
    console.log('Connecting to MySQL database...');
  }
}

class MongoDBDatabase implements IDatabase {
  connect() {
    console.log('Connecting to MongoDB database...');
  }
}

class UserService {
  private database: IDatabase;

  constructor(database: IDatabase) {
    this.database = database;  // Loosely coupled
  }

  getUser() {
    this.database.connect();
    // Get user logic
  }
}

const mySQLDatabase = new MySQLDatabase();
const userService = new UserService(mySQLDatabase);
userService.getUser();  // Now we can easily switch databases if needed
```
In Angular, services often depend on external resources like APIs or databases. DIP suggests that high-level components (e.g., UI components) should not be tightly coupled with low-level services (e.g., database service). Instead, both should depend on abstractions such as interfaces.

## Real-World Use Case:

In an inventory management system, the system might need to connect to different types of databases (SQL, NoSQL). By applying DIP, your system would depend on an abstraction (an interface like IDatabase), allowing the system to switch between databases without changing the core logic of the application.

# Conclusion:

Applying the SOLID principles in your Angular development ensures that your codebase remains flexible, maintainable, and scalable. By embracing these principles, you avoid common pitfalls like tight coupling, large interfaces, and classes that do too much. Whether you're building a travel booking system, e-commerce platform, or real-time inventory management solution, following SOLID will lead to better software architecture and cleaner code.

Start refactoring your Angular code today with these principles in mind to improve quality and efficiency in the long run.

References: [Applying SOLID Principles to Angular with examples](https://sheldonrcohen.medium.com/applying-solid-principles-to-angular-with-examples-fec460ffa541)




