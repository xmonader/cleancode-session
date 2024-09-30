Functions should be concise and perform a single, well-defined task. This makes them easier to understand, test, and maintain.

Bad Code
```typescript
function processOrder(order: any): void {
    // Validate order
    if (!order.items || order.items.length === 0) {
        console.log("No items in the order.");
        return;
    }
    // Calculate total
    let total = 0;
    for (let item of order.items) {
        total += item.price * item.quantity;
    }
    // Apply discount
    if (order.discount) {
        total -= order.discount;
    }
    // Save to database
    database.save(order);
    // Send confirmation email
    emailService.send(order.customerEmail, "Your order has been processed.");
}
```
Bad Code: The processOrder function handles multiple responsibilities: validation, calculation, saving, and sending emails.

Clean Code
```typescript
interface OrderItem {
    price: number;
    quantity: number;
}

interface Order {
    items: OrderItem[];
    discount?: number;
    customerEmail: string;
}

function validateOrder(order: Order): void {
    if (!order.items || order.items.length === 0) {
        throw new Error("No items in the order.");
    }
}

function calculateTotal(order: Order): number {
    const total = order.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
    return order.discount ? total - order.discount : total;
}

function saveOrder(order: Order): void {
    database.save(order);
}

function sendConfirmationEmail(email: string): void {
    emailService.send(email, "Your order has been processed.");
}

function processOrder(order: Order): void {
    validateOrder(order);
    const total = calculateTotal(order);
    saveOrder(order);
    sendConfirmationEmail(order.customerEmail);
}
```

Clean Code: Each responsibility is delegated to its own function, adhering to the Single Responsibility Principle. This makes the code more modular and easier to maintain.


Bad Code
```typescript
function handleUserInput(userInput: string): void {
    // Parse input
    const parts = userInput.split(',');
    const name = parts[0].trim();
    const age = parseInt(parts[1].trim(), 10);
    // Validate input
    if (age < 0) {
        console.log("Invalid age.");
        return;
    }
    // Store user
    users.push({ name, age });
    // Log action
    logger.info(`Added user ${name}, age ${age}`);
}
```


Bad Code: The handleUserInput function manages parsing, validation, storage, and logging.


Clean Code

```typescript
interface User {
    name: string;
    age: number;
}

function parseUserInput(userInput: string): { name: string; age: number } {
    const parts = userInput.split(',');
    const name = parts[0].trim();
    const age = parseInt(parts[1].trim(), 10);
    return { name, age };
}

function validateUser(age: number): void {
    if (age < 0) {
        throw new Error("Invalid age.");
    }
}

function storeUser(user: User): void {
    users.push(user);
}

function logAction(user: User): void {
    logger.info(`Added user ${user.name}, age ${user.age}`);
}

function handleUserInput(userInput: string): void {
    try {
        const { name, age } = parseUserInput(userInput);
        validateUser(age);
        const user: User = { name, age };
        storeUser(user);
        logAction(user);
    } catch (error) {
        console.error(error.message);
    }
}
```
Clean Code: Each task is separated into distinct functions, enhancing clarity and facilitating easier testing and maintenance.


Bad Code
```typescript
function updateInventory(order: any): void {
    for (let item of order.items) {
        inventory[item.id] -= item.quantity;
    }
    if (inventory[item.id] < 0) {
        console.log(`Item ${item.id} is out of stock.`);
    }
}
```

Bad Code: The updateInventory function handles both updating inventory and checking stock levels.

Clean Code
```typescript
class OutOfStockError extends Error {
    constructor(itemId: string) {
        super(`Item ${itemId} is out of stock.`);
        this.name = "OutOfStockError";
    }
}

function decrementInventory(itemId: string, quantity: number): void {
    inventory[itemId] -= quantity;
}

function checkStock(itemId: string): void {
    if (inventory[itemId] < 0) {
        throw new OutOfStockError(itemId);
    }
}

interface OrderItem {
    id: string;
    quantity: number;
}

interface Order {
    items: OrderItem[];
}

function updateInventory(order: Order): void {
    for (let item of order.items) {
        decrementInventory(item.id, item.quantity);
        checkStock(item.id);
    }
}
```


Clean Code: Separating the decrement and stock check into their own functions makes each function focused and the overall process clearer. Additionally, using a custom error class enhances error handling.
