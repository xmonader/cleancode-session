## SOLID



- Single Responsibility Principle (SRP)
- Open/Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)


###  Single Responsibility Principle (SRP)
A class should have only one reason to change, meaning it should have only one job or responsibility. AS Classes with a single responsibility are easier to understand, maintain, and test.

Bad Code
```typescript
class UserService {
    public createUser(name: string, email: string): void {
        // Validate user data
        if (!name || !email) {
            throw new Error("Invalid user data");
        }

        // Save user to database
        database.save({ name, email });

        // Send welcome email
        EmailService.sendEmail(email, "Welcome!", "Thank you for signing up.");
    }
}
```
Issues:

- Handles validation, database operations, and sending emails within a single class.

Clean Code
```typescript
// Validation Module
class Validator {
    public static validateUserData(name: string, email: string): void {
        if (!name || !email) {
            throw new Error("Invalid user data");
        }
    }
}

// Repository Module
class UserRepository {
    public saveUser(user: { name: string; email: string }): void {
        database.save(user);
    }
}

// Email Module
class EmailService {
    public static sendWelcomeEmail(email: string): void {
        this.sendEmail(email, "Welcome!", "Thank you for signing up.");
    }

    private static sendEmail(to: string, subject: string, body: string): void {
        // Implementation for sending email
    }
}

// User Service Module adhering to SRP
class UserService {
    public createUser(name: string, email: string): void {
        // Validate user data
        Validator.validateUserData(name, email);

        // Save user to database
        const user = { name, email };
        UserRepository.saveUser(user);

        // Send welcome email
        EmailService.sendWelcomeEmail(email);
    }
}
```
improvements:

- Separated responsibilities into distinct classes:
- Validator: Handles data validation.
- UserRepository: Manages database interactions.
- EmailService: Deals with email operations.
- UserService: Coordinates the user creation process by utilizing the other classes.



### Open Closed Principle (OCP)

Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

OCP allows you to add new functionality without altering existing code, reducing the risk of introducing bugs.

Example
Bad Code
```typescript
class PaymentProcessor {
    public processPayment(method: string, amount: number): void {
        if (method === "credit_card") {
            // Process credit card payment
            console.log(`Processing credit card payment of $${amount}`);
        } else if (method === "paypal") {
            // Process PayPal payment
            console.log(`Processing PayPal payment of $${amount}`);
        } else {
            throw new Error("Unsupported payment method");
        }
    }
}
```
Issues:
Every time a new payment method is introduced, the PaymentProcessor class needs to be modified.
Clean Code
```typescript
// Payment Interface
interface IPaymentMethod {
    process(amount: number): void;
}

// Credit Card Payment Implementation
class CreditCardPayment implements IPaymentMethod {
    public process(amount: number): void {
        console.log(`Processing credit card payment of $${amount}`);
        // Implementation details
    }
}

// PayPal Payment Implementation
class PayPalPayment implements IPaymentMethod {
    public process(amount: number): void {
        console.log(`Processing PayPal payment of $${amount}`);
        // Implementation details
    }
}

// Payment Processor Module adhering to OCP
class PaymentProcessor {
    private paymentMethods: { [key: string]: IPaymentMethod } = {};

    public registerPaymentMethod(name: string, method: IPaymentMethod): void {
        this.paymentMethods[name] = method;
    }

    public processPayment(name: string, amount: number): void {
        const paymentMethod = this.paymentMethods[name];
        if (!paymentMethod) {
            throw new Error("Unsupported payment method");
        }
        paymentMethod.process(amount);
    }
}

// Usage
const processor = new PaymentProcessor();
processor.registerPaymentMethod("credit_card", new CreditCardPayment());
processor.registerPaymentMethod("paypal", new PayPalPayment());

processor.processPayment("credit_card", 100);
processor.processPayment("paypal", 200);
```

Improvements:

- IPaymentMethod interface to define a contract for payment methods.
New payment methods can be added by implementing the interface and registering them without modifying the PaymentProcessor class.

### Liskov Substitution Principle 

Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

```typescript

class Vehicle {
    startEngine(): void {
        console.log("Engine started.");
    }

    move(): void {
        console.log("The vehicle is moving.");
    }
}

class Car extends Vehicle {
    // Cars have engines, so this works fine
}

class Bicycle extends Vehicle {
    // Bicycles don't have engines, but we're forced to implement this method
    startEngine(): void {
        throw new Error("Bicycles don't have engines!");
    }

    // Bicycles can still move
}

function startVehicle(vehicle: Vehicle): void {
    vehicle.startEngine(); // This assumes all vehicles have engines
    vehicle.move();
}

const car = new Car();
const bicycle = new Bicycle();

// Works as expected for cars
startVehicle(car); // Output: Engine started. The vehicle is moving.

// Violates LSP because bicycles don't have engines
startVehicle(bicycle); // Error: Bicycles don't have engines!

```

after

```typescript
// Base class for all vehicles
class Vehicle {
    move(): void {
        console.log("The vehicle is moving.");
    }
}

// Subclass for vehicles that have engines
class EngineVehicle extends Vehicle {
    startEngine(): void {
        console.log("Engine started.");
    }
}

// Car is an engine-based vehicle
class Car extends EngineVehicle {
    // Inherits startEngine() and move()
}

// Bicycle is a non-engine vehicle
class Bicycle extends Vehicle {
    // Inherits move() but does not need startEngine()
}

function startEngineVehicle(vehicle: EngineVehicle): void {
    vehicle.startEngine();
    vehicle.move();
}

function startNonEngineVehicle(vehicle: Vehicle): void {
    vehicle.move(); // Non-engine vehicles don't need to start engines
}

const car = new Car();
const bicycle = new Bicycle();

// Works as expected for cars (engine vehicles)
startEngineVehicle(car); // Output: Engine started. The vehicle is moving.

// Works as expected for bicycles (non-engine vehicles)
startNonEngineVehicle(bicycle); // Output: The vehicle is moving.


```





### Interface Segregation Principle (ISP)

No client should be forced to depend on methods it does not use. Split large interfaces into smaller, more specific ones.

Promotes more modular and flexible code by ensuring that classes only implement the methods they need.

Bad Code
```typescript
interface IWorker {
    work(): void;
    eat(): void;
}

class HumanWorker implements IWorker {
    public work(): void {
        console.log("Human working");
    }

    public eat(): void {
        console.log("Human eating");
    }
}

class RobotWorker implements IWorker {
    public work(): void {
        console.log("Robot working");
    }

    public eat(): void {
        // Robots don't eat
        throw new Error("Robots don't eat");
    }
}
```
Issues:

- RobotWorker is forced to implement the eat method, which it doesn't require, violating ISP.

Clean Code
```typescript
interface IWorkable {
    work(): void;
}

interface IFeedable {
    eat(): void;
}

class HumanWorker implements IWorkable, IFeedable {
    public work(): void {
        console.log("Human working");
    }

    public eat(): void {
        console.log("Human eating");
    }
}

class RobotWorker implements IWorkable {
    public work(): void {
        console.log("Robot working");
    }
}
```
Improvements:

- Split the IWorker interface into IWorkable and IFeedable.
- RobotWorker now only implements IWorkable, adhering to ISP.


### Dependency Injection

Depend on abstractions (interfaces), not on concrete types. High-level modules should not depend on low-level modules; both should depend on abstractions.

Promotes flexibility and decoupling, making the system easier to maintain and extend.


Bad Code
```typescript
class MySQLDatabase {
    public connect(): void {
        console.log("Connecting to MySQL");
    }
}

class UserService {
    private db: MySQLDatabase;

    constructor() {
        this.db = new MySQLDatabase();
    }

    public getUser(id: number): void {
        this.db.connect();
        console.log(`Fetching user with id ${id}`);
    }
}
```
Issues:

- UserService depends directly on MySQLDatabase, making it hard to switch databases or mock during testing.

Clean Code
```typescript
// Abstraction
interface IDatabase {
    connect(): void;
}

// MySQL Implementation
class MySQLDatabase implements IDatabase {
    public connect(): void {
        console.log("Connecting to MySQL");
    }
}

// PostgreSQL Implementation
class PostgreSQLDatabase implements IDatabase {
    public connect(): void {
        console.log("Connecting to PostgreSQL");
    }
}

// User Service Module adhering to DIP
class UserService {
    private db: IDatabase;

    constructor(db: IDatabase) {
        this.db = db;
    }

    public getUser(id: number): void {
        this.db.connect();
        console.log(`Fetching user with id ${id}`);
    }
}

// Usage
const mySQLDb = new MySQLDatabase();
const userService = new UserService(mySQLDb);
userService.getUser(1);

// Switching to PostgreSQL
const postgreSQLDb = new PostgreSQLDatabase();
const userServicePostgreSQL = new UserService(postgreSQLDb);
userServicePostgreSQL.getUser(2);
```
Improvements:

- Defined an IDatabase interface as an abstraction.
- UserService depends on IDatabase, not on any concrete database class.
- Allows easy switching of database implementations and facilitates testing with mocks.

