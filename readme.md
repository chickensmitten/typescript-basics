# Understanding Typescript

## Typescript basics
- it is a compiler for javascript
- borwser can't run typescript
- gives extra error checking for logical mistakes
```
// plain javascript
function add(num1, num2) {
  return num1 + num2;
}

console.log(add("2", "3")); // causes unwanted behaviour

// converted to typescript in using-ts.ts
const button = document.querySelector("button");
const input1 = document.getElementById("num1")! as HTMLInputElement;
const input2 = document.getElementById("num2")! as HTMLInputElement;

function add(num1: number, num2: number) {
  return num1 + num2;
}

button.addEventListener("click", function() {
  console.log(add(+input1.value, +input2.value));
});

```
- `tsc using-ts.ts` invoke typescript compiler to open a ts console
### Types
- types in ts: number, string, boolean, object, array, tuple, enum, any, union, literal, alias/custom, function, unknown, never
- check types with `typeof` with `console.log(typeof number)`
- object types
```
const obj = {
  name: 'Maximilian',
  age: 30,
  hobbies: ['Sports', 'Cooking'],
  role: [2, 'author']
};
```
- array types are normally fixed type
```
let favoriteActivities: string[];
favoriteActivities = ['Sports'];
```
- tuple is a fixed lengthed and type in element array
```
role: [number, string]; // first element is a number, second element is a string
role: [2, "author"]
```
- enum `enum { NEW, OLD }`. replaces global constants
```
// const ADMIN = 0;
// const READ_ONLY = 1;
// const AUTHOR = 2;

enum Role { ADMIN = 'ADMIN', READ_ONLY = 100, AUTHOR = 'AUTHOR' };

const person = {
  name: 'Maximilian',
  age: 30,
  hobbies: ['Sports', 'Cooking'],
  role: Role.ADMIN
};

if (person.role === Role.AUTHOR) {
  console.log('is author');
}
```
- any type is `*`. Don't use this. Defeats the purpose of having typescript
- union type is `input1: number | string`
- literal type is `resultConversion: 'as-number' | 'as-text';`
- custom type is 
```
type Combinable = number | string;
type ConversionDescriptor = 'as-number' | 'as-text';

function combine(
  input1: Combinable,
  input2: Combinable,
  resultConversion: ConversionDescriptor
) { ... }
```
- function types are types that describes a function. Code below basically says, the combineValue function should accept any funtion that takes two parameters where each parameter must be a number and the function overall returns a number.
```
function add(n1: number, n2: number) {
  return n1 + n2;
}
let combineValues: (a: number, b: number) => number;
combineValues = add;
consoel.log(combineValues(8,8));
```
another example code below is that this function takes in two number parameters and also a callback function that doesn't return anything.
```
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result);
}

addAndHandle(10, 20, (result) => {
  console.log(result);
});
```
- `void` means a function doesn't have return statement
- unknown type is because we don't know that users will put inside yet.
```
let userInput: unknown;
userInput = 5;
userInput = 'Max';
if (typeof userInput === 'string') {
  userName = userInput;
}

```
- never type is such that the function never returns a value, even undefined that declared in void. In the case below, it is because the code crashes our script. Or an infinite loop function also uses never type.
```

function generateError(message: string, code: number): never {
  throw { message: message, errorCode: code };
  // while (true) {}
}

generateError('An error occurred!', 500);
```

##