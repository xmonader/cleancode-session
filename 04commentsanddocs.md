Effective use of comments and documentation enhances code comprehension without cluttering the codebase.

Bad Code
```typescript
function add(a: number, b: number): number {
    return a + b; // returns the sum of a and b
}
```

Bad Code: The comment // returns the sum of a and b is redundant because the code is self-explanatory.

Clean Code
```typescript
/**
 * Adds two numbers and returns the result.
 *
 * @param a - The first number.
 * @param b - The second number.
 * @returns The sum of a and b.
 */
function add(a: number, b: number): number {
    return a + b;
}
```


Clean Code: A proper JSDoc comment provides detailed documentation, including parameters and return types, which is useful for users and tools like IDEs and documentation generators.

Bad Code
```typescript
function processData(data: number[]): number[] {
    // Initialize result
    let result: number[] = [];
    // Loop through data
    for (let item of data) {
        // Check if item is positive
        if (item > 0) {
            result.push(item);
        }
    }
    // Return result
    return result;
}
```
Bad Code: Multiple comments explain what each part of the function does, which can be inferred from the code itself.


Clean Code
```typescript
/**
 * Filters out non-positive numbers from the data.
 *
 * @param data - An array of numbers to process.
 * @returns An array containing only positive numbers from the input data.
 */
function processData(data: number[]): number[] {
    return data.filter(item => item > 0);
}
```


Clean Code: A concise JSDoc comment describes the purpose and behavior of the function without redundant inline comments. Additionally, using the filter method enhances readability.



Bad Code
```typescript
// Function to calculate factorial
function fact(n: number): number {
    if (n === 0) {
        return 1;
    } else {
        return n * fact(n - 1);
    }
}
```

Bad Code: The comment is minimal and doesn't provide detailed information. The function name fact is also less descriptive.
Clean Code
```typescript
/**
 * Recursively calculates the factorial of a non-negative integer.
 *
 * @param n - A non-negative integer whose factorial is to be computed.
 * @returns The factorial of the input number.
 * @throws {Error} If n is negative.
 */
function calculateFactorial(n: number): number {
    if (n < 0) {
        throw new Error("Factorial is not defined for negative numbers.");
    }
    if (n === 0) {
        return 1;
    }
    return n * calculateFactorial(n - 1);
}
```


Clean Code: A descriptive function name and a comprehensive JSDoc comment explain the function's purpose, parameters, return values, and potential exceptions.



Adhering to a consistent coding style improves readability and reduces cognitive load for developers. TypeScript’s styling is often guided by tools like ESLint and Prettier.

Bad Code
```typescript
function calculateSum(a:number,b:number):number{
    return a+b
}

let x= calculateSum( 5,2 )
console.log(x)
```


Bad Code: Inconsistent spacing (a:number,b:number), missing semicolons, and variable naming (x) reduce readability.


Clean Code
```typescript
function calculateSum(a: number, b: number): number {
    return a + b;
}

const total: number = calculateSum(5, 2);
console.log(total);
```


Clean Code: Follows TypeScript and general coding standards with proper spacing, semicolons, and descriptive variable names (total), enhancing consistency and readability.


Bad Code
```typescript
class person {
    name: string;
    Age: number;

    constructor(name: string, Age: number){
        this.name = name;
        this.Age = Age;
    }

    getInfo(){
        return `${this.name} is ${this.Age} years old.`;
    }
}
```


Bad Code: Class names should follow PascalCase (Person), and attribute names should use camelCase (age). Inconsistent capitalization reduces readability.
Clean Code
```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    getInfo(): string {
        return `${this.name} is ${this.age} years old.`;
    }
}
```

Clean Code: Adheres to naming conventions with Person, name, age, and getInfo, ensuring consistency.

Bad Code
```typescript
function process(data:any){
    if(data){
        console.log("Data exists")
    }
    else{
        console.log("No data")
    }
}
```

Bad Code: Unnecessary parentheses around the if condition and a generic function name process.

Clean Code
```typescript
function processData(data: any): void {
    if (data) {
        console.log("Data exists");
    } else {
        console.log("No data");
    }
}
```

Clean Code: Removes redundant parentheses, uses a more descriptive function name processData, and includes proper spacing and semicolons.



Readable code is easier to understand, reducing the likelihood of errors and simplifying maintenance.

Bad Code
```typescript
function f(a: number, b: number): number {
    if (a > b) {
        return a - b;
    } else if (a === b) {
        return 0;
    } else {
        return b - a;
    }
}
```
Bad Code: Uses single-letter variable names (a, b) and a non-descriptive function name (f), making it hard to understand.

Clean Code
```typescript
function compareValues(value1: number, value2: number): number {
    if (value1 > value2) {
        return value1 - value2;
    } else if (value1 === value2) {
        return 0;
    } else {
        return value2 - value1;
    }
}
```


Clean Code: Uses descriptive names (compareValues, value1, value2), enhancing clarity and readability.


Bad Code
```typescript
function compute(x: number, y: number, z: number): number {
    return Math.sqrt(x * x + y * y) + z;
}
```
Bad Code: The function name compute and parameters x, y, z are vague, and the operation performed is not immediately clear.


Clean Code
```typescript
function calculateHypotenuseWithOffset(sideA: number, sideB: number, offset: number): number {
    const hypotenuse = Math.sqrt(sideA ** 2 + sideB ** 2);
    return hypotenuse + offset;
}
```


Clean Code: The function name and parameter names clearly describe the purpose, and breaking down the calculation into intermediate steps enhances readability.


Bad Code
```typescript
function process(items: any[]): void {
    for (let i of items) {
        if (i.status === 'active') {
            doSomething(i);
        } else {
            doSomethingElse(i);
        }
    }
}
```
Bad Code: Uses generic types (any[]), single-letter variable i, and generic function names doSomething and doSomethingElse.

Clean Code
```typescript
interface Item {
    status: 'active' | 'inactive';
    // Other properties...
}

function processItems(items: Item[]): void {
    for (const item of items) {
        if (item.status === 'active') {
            activateItem(item);
        } else {
            deactivateItem(item);
        }
    }
}

function activateItem(item: Item): void {
    // Implementation for activating the item
}

function deactivateItem(item: Item): void {
    // Implementation for deactivating the item
}
```


Clean Code: Uses descriptive types (Item), descriptive variable names (item), and function names that clearly indicate the actions being performed (activateItem, deactivateItem).
