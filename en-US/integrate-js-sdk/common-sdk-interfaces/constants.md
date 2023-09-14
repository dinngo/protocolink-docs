# Constants

Constants are values that remain fixed throughout the execution of a program, and they are usually declared at the beginning of the code. In this SDK, we have defined several constants that can be used in different parts of the code.

* A basis point is a unit of measure used in finance to describe the percentage change in a financial instrument. It is equal to 0.01%. We use a basis points base of `10,000`, which means that 1 basis point is equivalent to 0.01%. This constant can be used, for example, to calculate the interest rate or the fee percentage of a financial transaction.

```typescript
const BPS_BASE = 10000;
```
