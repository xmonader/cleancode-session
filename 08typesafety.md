using TypeScript's type system enhances code reliability and readability, making it easier to understand what types of data are expected.

Bad Code
```typescript
function add(a, b) {
    return a + b;
}
```
Bad Code: Lacks type annotations, making it unclear what types of arguments the function expects.


Clean Code
```typescript
function add(a: number, b: number): number {
    return a + b;
}
```


Clean Code: Uses type annotations to specify that add expects two numbers and returns a number, improving clarity and enabling static type checking.


Bad Code
```typescript
function getUserInfo(userId: string): any {
    const user = database.getUser(userId);
    if (user) {
        return { name: user.name, age: user.age };
    }
    return null;
}
```
Bad Code: Returns any, which undermines type safety and makes it unclear what the function returns.


Clean Code
```typescript
interface User {
    name: string;
    age: number;
}

function getUserInfo(userId: string): User | null {
    const user: User | null = database.getUser(userId);
    if (user) {
        return { name: user.name, age: user.age };
    }
    return null;
}
```


Clean Code: Defines an interface for User and uses proper return types (User | null), enhancing type safety and clarity.


Bad Code
```typescript
function processItems(items: any[]): void {
    for (let item of items) {
        console.log(item);
    }
}
```

Bad Code: Uses any[], which provides no information about the structure of items.

Clean Code
```typescript
interface Item {
    id: number;
    name: string;
    status: 'active' | 'inactive';
}

function processItems(items: Item[]): void {
    for (const item of items) {
        console.log(item.id, item.name, item.status);
    }
}
```

Clean Code: Defines an interface for Item and uses it in the function signature, enhancing type safety and clarity.
