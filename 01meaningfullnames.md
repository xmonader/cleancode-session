## Naming

Choosing descriptive and meaningful names for variables, functions, classes, interfaces, and other identifiers enhances code readability and maintainability.




### Example
Bad Code
```typescript
function doIt(a: number, b: number): number {
    return a + b;
}

let x = doIt(5, 2);
console.log(x);
```


Bad Code: The function calc and variables a, b, x have ambiguous names that don't convey their purpose.

Clean Code
```typescript
function add(num1: number, num2: number): number {
    return num1+num2
}

const sum = add(5, 2);
console.log(sum);
```

Clean Code: Descriptive names like add, num1, num2, sum




### Example
Bad Code
```typescript
function calc(a: number, b: number): number {
    return a * b + 10 / b;
}

let x = calc(5, 2);
console.log(x);
```


Bad Code: The function calc and variables a, b, x have ambiguous names that don't convey their purpose.

Clean Code
```typescript
function calculateTotalPrice(unitPrice: number, quantity: number): number {
    const taxRate = 10;
    return unitPrice * quantity + taxRate / quantity;
}

const totalPrice = calculateTotalPrice(5, 2);
console.log(totalPrice);
```

Clean Code: Descriptive names like calculateTotalPrice, unitPrice, quantity, and totalPrice make the code self-explanatory.

### Example
Bad Code

```typescript
function getData(): any {
    return { n: "Alice", a: 28 };
}

const user = getData();
console.log(user.n, user.a);
```

Bad Code: The function getData returns an object with unclear property names n and a, making the code harder to understand.

Clean Code

```typescript
interface UserInfo {
    name: string;
    age: number;
}

function getUserInfo(): UserInfo {
    return { name: "Alice", age: 28 };
}

const userInfo = getUserInfo();
console.log(userInfo.name, userInfo.age);
```


Clean Code: Using an interface UserInfo with clear property names name and age enhances clarity and type safety.

### Example 3

Bad Code

```typescript

class C {
    private d: any;

    constructor(d: any) {
        this.d = d;
    }

    public m(): void {
        for (let i of this.d) {
            console.log(i);
        }
    }
}
```
Bad Code: The class name C, method m, and variable d lack descriptive meaning.

```typescript
class Customer {
    private data: string[];

    constructor(data: string[]) {
        this.data = data;
    }

    public displayData(): void {
        for (const item of this.data) {
            console.log(item);
        }
    }
}
```

Clean Code: The class Customer, method displayData, and variable data clearly indicate their roles and contents.



### Example

Misleading name

Bad Code

```typescript
function hasData(): void {
    if this.socket.ready() {
        console.log("no data in the socket yet")
    }
}
```

Bad Code: name doesn't really 

Clean Code

```typescript
function hasData(): boolean {
    if ... {
        return true
    }
    return false
}
```




Bad Code

```typescript
function isValidUsername(username: string): boolean {
    if ... {
        return true
    }
    throw ValidationError(...)
}
```

Bad Code: the function raises in case of invalid username instead of returning false


Clean Code

```typescript
function isValidUsername(username: string): boolean {
    if ... {
        return true
    }
    throw false
}
```

