Choosing appropriate data structures enhances the efficiency and clarity of your code.

Bad Code
```typescript
function getUserInfo(): any[] {
    return ["John Doe", 30, "Engineer"];
}

const [name, age, profession] = getUserInfo();
console.log(name, age, profession);
```

Bad Code: Returns a tuple where the order of elements is significant but not explicit, leading to potential confusion.


Clean Code
```typescript
interface UserInfo {
    name: string;
    age: number;
    profession: string;
}

function getUserInfo(): UserInfo {
    return { name: "John Doe", age: 30, profession: "Engineer" };
}

const userInfo = getUserInfo();
console.log(userInfo.name, userInfo.age, userInfo.profession);
```


Clean Code: Utilizes an interface to represent user information with named properties, improving structure and clarity.


Bad Code
```typescript
function countFruits(fruits: string[]): { [key: string]: number } {
    const counts: { [key: string]: number } = {};
    for (let fruit of fruits) {
        if (fruit in counts) {
            counts[fruit] += 1;
        } else {
            counts[fruit] = 1;
        }
    }
    return counts;
}
```

Bad Code: Manually checks and updates the dictionary, leading to verbose and repetitive code.

Clean Code
```typescript
function countFruits(fruits: string[]): Record<string, number> {
    const counts: Record<string, number> = {};
    for (const fruit of fruits) {
        counts[fruit] = (counts[fruit] || 0) + 1;
    }
    return counts;
}
```

Clean Code: Uses a more concise approach to increment counts, making the code more readable.


Bad Code
```typescript
function getEmployeeRecords(): any[] {
    const employees: any[] = [];
    // Assume fetching from database
    for (let record of database.fetchAll()) {
        employees.push([record.id, record.name, record.department]);
    }
    return employees;
}

const employees = getEmployeeRecords();
for (let employee of employees) {
    console.log(employee[0], employee[1], employee[2]);
}
```

Bad Code: Uses arrays to represent employee records, making it unclear what each element represents.


Clean Code
```typescript
interface Employee {
    id: number;
    name: string;
    department: string;
}

function getEmployeeRecords(): Employee[] {
    const employees: Employee[] = [];
    // Assume fetching from database
    for (const record of database.fetchAll()) {
        employees.push({ id: record.id, name: record.name, department: record.department });
    }
    return employees;
}

const employees = getEmployeeRecords();
for (const employee of employees) {
    console.log(employee.id, employee.name, employee.department);
}
```


Clean Code: Utilizes an interface to define an Employee structure, enhancing clarity and type safety.
