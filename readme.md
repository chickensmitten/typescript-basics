# Understanding Typescript

## Typescript basics
- it is a compiler for javascript
- borwser can't run typescript
- gives extra error checking for logical mistakes

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

## Typescript compiler and its configuration
- to use `tsc` command either `npm install typescript@latest -g` or use `npx`
- `tsc using-ts.ts --watch` invoke typescript compiler to open a ts console and let it auto compile
- run `tsc --init` in the project root directory to create `tsconfig.json` file
- `tsconfig.json` is crucial file for managing the project. `include` and `exclude` in it will exclude or include files to compile.
- `compilerOptions` helps us control how our typescript code is compiled
- if `sourceMap` set to true, it will help us with debugging ts files as we can access them in browser inspect
- `.d.ts` is a declaration of types. these files are needed for third party ts libraries.
- `"outDir": "./dist"` tells where the create js files will be stored
- `"rootDir": "./src"` to make sure typescript compiler doesn't look at other folders. It also maintains the folder structure.
- `"removeComments": true` removes comment in ts file.
- `noEmit: true` if you don't want to create output js file. because you just want to check. It is helpful for big projects to save time.
- `"noEmitOnError": true,` means don't emit if there is an error in typescript code.
- `"noImplicitAny": false` at false, it ensures that we have to be clear on the parameters and value types that we are working with our code.
- `"strictNullChecks": true` asks typescript to be strict on working with values that might contain null value.
- for debugging, install chrome debugger from vscode marketplace. then "Run" > "Start Debugging". You should see a `launch.json` file. Then start debugging, the code should stop running at the breakpoint.