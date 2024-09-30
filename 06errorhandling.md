Effective error handling ensures that your code can gracefully handle unexpected situations without crashing.

Bad Code
```typescript
function readFile(filePath: string): string {
    const fs = require('fs');
    const data = fs.readFileSync(filePath, 'utf8');
    return data;
}
```


Bad Code: Does not handle potential exceptions, which can lead to unhandled errors.


Clean Code
```typescript
import * as fs from 'fs';

function readFile(filePath: string): string | null {
    try {
        const data = fs.readFileSync(filePath, 'utf8');
        return data;
    } catch (error) {
        if (error.code === 'ENOENT') {
            console.error(`Error: The file ${filePath} does not exist.`);
        } else {
            console.error(`Error: An error occurred while reading ${filePath}.`);
        }
        return null;
    }
}
```

Clean Code: Uses a try-catch block to handle specific exceptions gracefully, providing informative error messages.

Bad Code
```typescript
function divide(a: number, b: number): number {
    return a / b;
}
```
Bad Code: Division by zero will raise an unhandled exception.


Clean Code
```typescript
function divide(a: number, b: number): number | null {
    if (b === 0) {
        console.error("Error: Cannot divide by zero.");
        return null;
    }
    return a / b;
}
```

Clean Code: Checks for division by zero and handles it gracefully by informing the user and returning null.

Bad Code
```typescript
function getUserAge(userId: string): number {
    const user = database.getUser(userId);
    return user.age;
}
```

Bad Code: Assumes that the user exists and has an age property, which may not always be true.

Clean Code
```typescript
interface User {
    id: string;
    age: number;
}

function getUserAge(userId: string): number | null {
    try {
        const user: User = database.getUser(userId);
        return user.age;
    } catch (error) {
        if (error instanceof UserNotFoundError) {
            console.error(`Error: User with ID ${userId} not found.`);
        } else {
            console.error(`Error: Unable to retrieve user age for ID ${userId}.`);
        }
        return null;
    }
}
```


Clean Code: Handles cases where the user might not exist or the age property is missing, preventing potential crashes and providing informative error messages.
