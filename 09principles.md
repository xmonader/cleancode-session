## DRY (Don't Repeat Yourself)


Repeating code can lead to inconsistencies, making maintenance harder and increasing the risk of bugs. By adhering to DRY, you ensure that changes need to be made in only one place.

Example 1: Avoiding Repetitive Logic
Bad Code
```typescript
class OrderService {
    public calculateOrderTotal(items: { price: number; quantity: number }[]): number {
        let total = 0;
        for (const item of items) {
            total += item.price * item.quantity;
        }
        return total;
    }

    public calculateDiscountedTotal(items: { price: number; quantity: number }[], discount: number): number {
        let total = 0;
        for (const item of items) {
            total += item.price * item.quantity;
        }
        return total - discount;
    }
}
```
Issues:

The logic for calculating the total is duplicated in both methods.


Clean Code
```typescript
class OrderService {
    private calculateTotal(items: { price: number; quantity: number }[]): number {
        return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
    }

    public calculateOrderTotal(items: { price: number; quantity: number }[]): number {
        return this.calculateTotal(items);
    }

    public calculateDiscountedTotal(items: { price: number; quantity: number }[], discount: number): number {
        return this.calculateTotal(items) - discount;
    }
}
```
Improvements:

- Extracted the common logic into a private calculateTotal method.
- Both public methods now utilize the shared logic, adhering to DRY.
Example 2: Utilizing Reusable Functions

Bad Code
```typescript
function fetchUserData(userId: string): any {
    return fetch(`https://api.example.com/users/${userId}`)
        .then(response => response.json())
        .then(data => data);
}

function fetchOrderData(orderId: string): any {
    return fetch(`https://api.example.com/orders/${orderId}`)
        .then(response => response.json())
        .then(data => data);
}
```
Issues:

- Both functions perform similar fetch and parsing operations.


Clean Code
```typescript
async function fetchData(endpoint: string): Promise<any> {
    const response = await fetch(`https://api.example.com/${endpoint}`);
    if (!response.ok) {
        throw new Error(`Failed to fetch ${endpoint}`);
    }
    return response.json();
}

function fetchUserData(userId: string): Promise<any> {
    return fetchData(`users/${userId}`);
}

function fetchOrderData(orderId: string): Promise<any> {
    return fetchData(`orders/${orderId}`);
}
```
Improvements:

- Created a generic fetchData function to handle fetching and parsing.
- fetchUserData and fetchOrderData now use fetchData function.


## YAGNI (You Aren't Gonna Need It)

YAGNI advises against adding functionality until it is necessary. Avoid implementing features based on assumptions or future needs that might never materialize.



Bad Code
```typescript
class Logger {
    public log(message: string, level: string): void {
        if (level === 'info') {
            console.info(message);
        } else if (level === 'error') {
            console.error(message);
        } else if (level === 'debug') {
            console.debug(message);
        }
        // Potential for more levels in the future
    }
}
```
Issues:

Anticipates the addition of more log levels, leading to conditional complexity.

Clean Code
```typescript
class Logger {
    public log(message: string): void {
        console.log(message);
    }
}

const logger = new Logger();
logger.log("This is a log message.");
```
Improvements:

Implemented only the necessary functionality.
Avoided premature support for multiple log levels until required.


Bad Code
```typescript
class Flagsmith {
    private enabledFeatures: Set<string> = new Set();

    public enableFeature(feature: string): void {
        this.enabledFeatures.add(feature);
    }

    public disableFeature(feature: string): void {
        this.enabledFeatures.delete(feature);
    }

    public isFeatureEnabled(feature: string): boolean {
        return this.enabledFeatures.has(feature);
    }
    public enableFeatureForUser(username: string): void {
        ...
    }
    public enableFeatureForCountry(country: string): void {
        ...
    }
    public disableFeatureForUser(username: string): void {
        ...
    }
    public disableFeatureForCountry(country: string): void {
        ...
    }
    
}

```

Issues:

- Adds flexibility for future feature management that may not be needed immediately.

Good Code
```typescript
class Flagsmith {
    private features: { [key: string]: boolean } = {};
    
    public enableFeature(feature: string): void {
        this.features[feature] = true;
    }
    
    public disableFeature(feature: string): void {
        this.features[feature] = false;
    }
    
    public isFeatureEnabled(feature: string): boolean {
        return this.features[feature] || false;
    }
    
    // Future methods for advanced feature management
}
```

Improvements:
- We implement only what we needed

Avoided unnecessary complexity by focusing on current requirements.


## KISS (Keep It Simple, Stupid)
simplicity in design and implementation. The simplest solution is often the best one.



Simplify Conditional Logic

Bad Code
```typescript
function getDiscountedPrice(price: number, isMember: boolean, isHoliday: boolean): number {
    if (isMember) {
        if (isHoliday) {
            return price * 0.8;
        } else {
            return price * 0.85;
        }
    } else {
        if (isHoliday) {
            return price * 0.9;
        } else {
            return price;
        }
    }
}
```
Issues:

Nested if statements increase complexity.
Clean Code
```typescript
function getDiscountedPrice(price: number, isMember: boolean, isHoliday: boolean): number {
    let discount = 1;

    if (isMember) {
        discount *= 0.85;
    }

    if (isHoliday) {
        discount *= 0.9;
    }

    return price * discount;
}
```
Improvements:

- Flattened the conditional structure.
- Made the discount calculation more straightforward and scalable.


Avoiding Unnecessary Abstractions
Bad Code
```typescript
interface ICalculator {
    calculate(a: number, b: number): number;
}

class Addition implements ICalculator {
    calculate(a: number, b: number): number {
        return a + b;
    }
}

class Subtraction implements ICalculator {
    calculate(a: number, b: number): number {
        return a - b;
    }
}

class Calculator {
    private operations: { [key: string]: ICalculator } = {};

    public registerOperation(name: string, operation: ICalculator): void {
        this.operations[name] = operation;
    }

    public executeOperation(name: string, a: number, b: number): number {
        return this.operations[name].calculate(a, b);
    }
}

// Usage
const calculator = new Calculator();
calculator.registerOperation('add', new Addition());
calculator.registerOperation('subtract', new Subtraction());

console.log(calculator.executeOperation('add', 5, 3)); // 8
console.log(calculator.executeOperation('subtract', 5, 3)); // 2
```
Issues:

- Over-engineered for simple operations like addition and subtraction.

Clean Code
```typescript
function add(a: number, b: number): number {
    return a + b;
}

function subtract(a: number, b: number): number {
    return a - b;
}

// Usage
console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
```
Improvements:

- Removed unnecessary interfaces and classes.
- Kept the implementation simple and direct.
