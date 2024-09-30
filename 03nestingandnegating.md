Deeply nested code can be hard to read and maintain. Reducing nesting levels improves code clarity.

Bad Code
```typescript
function processData(data: any): void {
    if (data) {
        if (Array.isArray(data)) {
            if (data.length > 0) {
                for (let item of data) {
                    if (item !== null) {
                        console.log(item);
                    }
                }
            }
        }
    }
}
```

Bad Code: Multiple nested if statements make the function hard to follow.


Clean Code
```typescript
function processData(data: any[]): void {
    if (!data || data.length === 0) {
        return;
    }

    for (const item of data) {
        if (item === null) {
            continue;
        }
        console.log(item);
    }
}
```

Clean Code: Uses guard clauses to handle edge cases early, reducing the need for deep nesting and improving readability.


Bad Code
```typescript
function findUser(users: any[], userId: string): any | null {
    for (let user of users) {
        if (user.id === userId) {
            if (user.active) {
                return user;
            }
        }
    }
    return null;
}
```

Bad Code: Nested if statements increase complexity.

Clean Code

```typescript
interface User {
    id: string;
    active: boolean;
    [key: string]: any;
}

function findActiveUser(users: User[], userId: string): User | null {
    for (const user of users) {
        if (user.id === userId && user.active) {
            return user;
        }
    }
    return null;
}
```
Clean Code: Combines conditions into a single if statement, simplifying the logic.

Guard clauses help handle edge cases and errors early in a function, reducing the need for deep nesting.

Bad Code
```typescript
function processOrder(order: any): void {
    if (order.isValid()) {
        // Process order
        console.log("Processing order...");
    } else {
        console.log("Invalid order.");
    }
}
```

Bad Code: Uses an if-else structure, which can lead to deeper nesting as more conditions are added.

Clean Code
```typescript
function processOrder(order: any): void {
    if (!order.isValid()) {
        console.log("Invalid order.");
        return;
    }
    // Process order
    console.log("Processing order...");
}
```


Clean Code: Uses a guard clause to handle invalid orders early, allowing the main logic to proceed without additional nesting.

Bad Code
```typescript
function sendEmail(user: any): void {
    if (user) {
        if (user.emailVerified) {
            send(user.email, "Welcome!");
        } else {
            console.log("Email not verified.");
        }
    } else {
        console.log("No user provided.");
    }
}
```

Bad Code: Uses nested if statements to handle different scenarios.

Clean Code
```typescript
function sendEmail(user: any): void {
    if (!user) {
        console.log("No user provided.");
        return;
    }
    if (!user.emailVerified) {
        console.log("Email not verified.");
        return;
    }
    send(user.email, "Welcome!");
}
```

Clean Code: Uses guard clauses to handle each case separately, reducing nesting and improving readability.


Bad Code
```typescript
function calculateDiscount(order: any): number {
    if (order) {
        if (order.total > 100) {
            return order.total * 0.2;
        } else {
            return order.total * 0.1;
        }
    } else {
        return 0;
    }
}
```
Bad Code: Uses nested if statements to handle different discount scenarios.


Clean Code
```typescript
function calculateDiscount(order: any): number {
    if (!order) {
        return 0;
    }
    if (order.total > 100) {
        return order.total * 0.2;
    }
    return order.total * 0.1;
}
```

Clean Code: Uses guard clauses to handle the absence of an order and simplifies the discount logic.



Bad Code
```typescript
function calculateTax(order: any): number {
    if (order) {
        if (order.total > 100) {
            if (order.customer.isLoyal) {
                return order.total * 0.2;
            }
        }
    }
    return 0;
}
```
Bad Code: Deeply nested conditions make the function difficult to read.



Clean Code
```typescript
interface Order {
    total: number;
    customer: {
        isLoyal: boolean;
    };
}

function calculateTax(order: Order | null): number {
    if (!order || order.total <= 100 || !order.customer.isLoyal) {
        return 0;
    }
    return order.total * 0.2;
}
```

Clean Code: Combines conditions using logical operators, reducing nesting and enhancing clarity.


Bad Code
```typescript
function processOrder(order: any): void {
    if (order) {
        if (order.isValid()) {
            if (order.total > 100) {
                applyDiscount(order);
                sendConfirmation(order);
            }
        }
    }
}
```
Bad Code: Multiple nested if statements make the function hard to follow.


Clean Code
```typescript
interface Order {
    isValid(): boolean;
    total: number;
}

function processOrder(order: Order | null): void {
    if (!order || !order.isValid() || order.total <= 100) {
        return;
    }
    applyDiscount(order);
    sendConfirmation(order);

}
```

Clean Code: Combines conditions using logical operators, reducing nesting and enhancing clarity.



Negating conditions in if statements can make the code harder to understand. It's often clearer to use positive conditions.

Example 1
Bad Code
```typescript
function processOrder(order: any): void {
    if (!order.isValid()) {
        return;
    }
    // Continue processing
    console.log("Processing order...");
}
```


Bad Code: Negates the condition, introducing an early return that might be less intuitive. (however it is still usefull to reduce the nested ifs by inverting the conditions)

Clean Code
```typescript
function processOrder(order: any): void {
    if (order.isValid()) {
        // Continue processing
        console.log("Processing order...");
    }
}
```

Clean Code: Uses a positive condition, making the logic straightforward.


Bad Code
```typescript
function sendEmail(user: any): void {
    if (!user.emailVerified) {
        return;
    }
    send(user.email, "Welcome!");
}
```
Bad Code: Negates the condition, which can obscure the intent.

Clean Code
```typescript
function sendEmail(user: any): void {
    if (user.emailVerified) {
        send(user.email, "Welcome!");
    }
}
```
Clean Code: Uses a positive condition, clearly indicating when the email should be sent.



Bad Code
```typescript
function getDiscountedPrice(price: number, discount: number | null): number {
    if (!discount) {
        return price;
    }
    return price - (price * discount);
}
```
Bad Code: Negates the discount condition, making the primary action less prominent.


Clean Code
```typescript
function getDiscountedPrice(price: number, discount: number | null): number {
    if (discount) {
        return price - (price * discount);
    }
    return price;
}
```

Clean Code: Uses a positive condition, emphasizing the discount application.



Complex conditional logic can be hard to follow. Encapsulating conditionals into well-named functions clarifies intent.

Bad Code
```typescript
function getDiscount(user: any): number {
    if (user.age > 65 && user.isMember) {
        return 0.2;
    } else if (user.age > 65) {
        return 0.1;
    } else if (user.isMember) {
        return 0.05;
    } else {
        return 0.0;
    }
}
```


Bad Code: Combines multiple conditions within if statements, making it harder to understand the discount logic.


Clean Code
```typescript
interface User {
    age: number;
    isMember: boolean;
}

function isSenior(user: User): boolean {
    return user.age > 65;
}

function isMember(user: User): boolean {
    return user.isMember;
}

function getDiscount(user: User): number {
    if (isSenior(user) && isMember(user)) {
        return 0.2;
    }
    if (isSenior(user)) {
        return 0.1;
    }
    if (isMember(user)) {
        return 0.05;
    }
    return 0.0;
}
```

Clean Code: Encapsulates conditions into isSenior and isMember functions, making the getDiscount function's intent clearer.


Bad Code
```typescript
function processPayment(payment: any): void {
    if (payment.method === 'credit_card' && payment.amount > 1000) {
        applySurcharge(payment);
    } else if (payment.method === 'credit_card') {
        processCreditCard(payment);
    } else if (payment.method === 'paypal') {
        processPaypal(payment);
    } else {
        throw new Error("Unsupported payment method");
    }
}
```
Bad Code: Contains complex conditional logic within if statements.


Clean Code
```typescript
interface Payment {
    method: 'credit_card' | 'paypal';
    amount: number;
}

function isCreditCard(payment: Payment): boolean {
    return payment.method === 'credit_card';
}

function isLargeAmount(payment: Payment): boolean {
    return payment.amount > 1000;
}

function processPayment(payment: Payment): void {
    if (isCreditCard(payment) && isLargeAmount(payment)) {
        applySurcharge(payment);
    } else if (isCreditCard(payment)) {
        processCreditCard(payment);
    } else if (payment.method === 'paypal') {
        processPaypal(payment);
    } else {
        throw new Error("Unsupported payment method");
    }
}
```

Clean Code: Encapsulates conditions into isCreditCard and isLargeAmount functions, enhancing readability.


Bad Code
```typescript
function canAccess(user: any, resource: any): boolean {
    if (user.isAdmin || (resource.isPublic && !user.isBanned)) {
        return true;
    }
    return false;
}
```

Bad Code: Combines multiple conditions in a single if statement, making the access logic less transparent.

Clean Code
```typescript
interface User {
    isAdmin: boolean;
    isBanned: boolean;
}

interface Resource {
    isPublic: boolean;
}

function isAdmin(user: User): boolean {
    return user.isAdmin;
}

function isPublicResource(resource: Resource): boolean {
    return resource.isPublic;
}

function isNotBanned(user: User): boolean {
    return !user.isBanned;
}

function canAccess(user: User, resource: Resource): boolean {
    if (isAdmin(user) || (isPublicResource(resource) && isNotBanned(user))) {
        return true;
    }
    return false;
}

```


Clean Code: Encapsulates each condition into its own function, clarifying the access rules.
