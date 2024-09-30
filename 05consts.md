Magic numbers are hard-coded values with unexplained meanings. Replacing them with named constants improves code clarity and ease of maintenance.

Bad Code
```typescript
function getDiscountedPrice(price: number): number {
    return price - (price * 0.15);
}
```
Bad Code: Uses a magic number 0.15 directly in the calculation, which lacks context.


Clean Code
```typescript
const DISCOUNT_RATE: number = 0.15;

function getDiscountedPrice(price: number): number {
    return price - (price * DISCOUNT_RATE);
}
```
Clean Code: Defines DISCOUNT_RATE as a constant, making the code more understandable and easier to modify.

Bad Code
```typescript
function calculateCircumference(radius: number): number {
    return 2 * 3.14159 * radius;
}
```

Bad Code: Uses a hard-coded approximation of π (3.14159).


Clean Code
```typescript
const PI: number = Math.PI;

function calculateCircumference(radius: number): number {
    return 2 * PI * radius;
}
```

Clean Code: Utilizes the built-in Math.PI constant, which is more accurate and self-explanatory.


Bad Code
```typescript
function sendEmail(to: string, subject: string, body: string): void {
    const smtp = new SMTP('smtp.example.com', 587);
    smtp.startTLS();
    smtp.login('user', 'password');
    smtp.sendMail('user@example.com', to, `Subject: ${subject}\n\n${body}`);
    smtp.quit();
}
```
Bad Code: Hard-codes SMTP configuration details within the function.


Clean Code
```typescript
const SMTP_SERVER: string = 'smtp.example.com';
const SMTP_PORT: number = 587;
const SMTP_USER: string = 'user';
const SMTP_PASSWORD: string = 'password';
const SENDER_EMAIL: string = 'user@example.com';

function sendEmail(to: string, subject: string, body: string): void {
    const smtp = new SMTP(SMTP_SERVER, SMTP_PORT);
    smtp.startTLS();
    smtp.login(SMTP_USER, SMTP_PASSWORD);
    smtp.sendMail(SENDER_EMAIL, to, `Subject: ${subject}\n\n${body}`);
    smtp.quit();
}
```

Clean Code: Defines constants for SMTP configuration, making it easier to manage and update these values.

-> note that function should change to accept all as parameters (or to be retrieved e.g from a config file or environment variables)
