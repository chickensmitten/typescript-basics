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

## Next Generation Javascript and Typescript
- Shouldn't use `var` anymore, it is a global scope. Instead, use `let` and `const`, it uses block scope where variables called within a scope like a function is only usable within it, unless it is called from a higher scope.
- arrow function, possible permutations
```
const add = (a: number, b: number) => {
  return a + b;
};

const add  = (a: number, b: number) => a + b;

const add  = () => "something";

const printOutput = output => console.log(output); // but not usable in ts, cauase there is no type for output
```
- default function parameters, should be the last
```
const add  = (a: number, b: number = 1) => a + b; // cannot a: number = 1

printOutput(add(5));
```
- spread operator
```
const hobbies = ["sports", "cooking"];
const activeHobbies = ["Hiking"];
activeHobbies.push(...hobbies); // not activeHobbies.push(hobbies)
```
- rest parameters with typescript
```
const add = (...numbers: number[]) => {
  return numbers.reduce((curResult, curValue) => {
    return curResult + curValue;
  }, 0);
}
```
- destructuring array and objects
```
const [hobby1, hobby2, ...remainingHobbies] = hobbies;
console.log(hobbies, hobby1, hobby2); // returns ["running", "soccer", ["basketball", "rugby"]]
const {firstName, age} = person; // the firstName and age key must be available in the object
const {firstName: userName, age} = person; // firstName key is renamed as userName
```

## Classes
- All classes have constructors. It can be shortened with the example below:
```
class Product {
  title: string;
  price: number;
  private isListed: boolean;
 
  constructor(name: string, pr: number) {
    this.title = name;
    this.price = pr;
    this.isListed = true;
  }
}

// shortened below
class Product {
  private isListed: boolean;
 
  constructor(public title: string, public price: number) {
    this.isListed = true;
  }
}
```
- `this` typically refers to the thing that calls for it. In the code example below, `this` refers to the instance of the Department class.
```
describe(this: Department) {
  console.log("Department: " + this.name)
}
```
- `readonly` is available for typescript, not javascript.
- `static` methods and properties can be accessed directly from the class and not through the instance of classes. i.e. `Math.pow()` instead of `const math1 = Math.new; math1.pow()`
- `abstract` can't be instantiated, has to be extended. It means that a method must be implemented by any class based on the abstract class. **It is useful if you want to enforce that all classes based on other class, share some common methods of property while not needing to provide the concrete value or concrete implementation of the base class, instead the inheriting class has to do that. Classes marked as abstract can't be instantiated, it can only be inherited from and for inheriting classes to instantiate**
```
abstract class Department {
  ...
  abstract describe(this: Department): void;
  ...
}

class ITDepartment extends Department {
  ...
  describe() {
    console.log("IT Department " + this.id);
  }
  ...
}

```
- Private constructors. There is a pattern in OOP called the Singleton pattern. It ensures that you only have one instance of a certain class. It can be useful if you can't use static methods or properties. When you can't use static methods or properties for whatever reason while don't want multiple objects for a class. i.e. there is only one AccountingDepartment under Department class. This is done by adding `private` before the constructor class
```
// under AccountingDepartment class
private constructor(id: string, private reports: string[]) { ... }
```
Then you can't call `new` outside of the class to instantiate a class. To call the private constructors, have to use static.
```
private static instance: AccountingDepartment; // creates one instance of an AccountingDepartment

static getInstance() {
  if (AccountingDepartment.instance) {
    return this.instance;
  }
  this.instance = new AccountingDepartment("d2", []); // new can be called within the class.
  return this.instance;
}

const accounting = AccountingDepartment.getInstance();
```

## Interfaces
- An interface describes the structure of an object. It only exits in typescript, not javascript.
- Interfaces can't be instantiated and are not compiled, classes can be instantiated and are compiled.
- Can use interface to type check an object
```
interface Greetable {
  name: string;
  greet(phrase: string): void;
}

let user1: Greetable;

user1 = {
  name: "Simon",
  greet(phrase: string) {
    console.log(phrase + " " + this.name);
  }
};

user1.greet("Hi there I am"); // returns "hi there I am Simon"
```
- Why use `interface` when can use custom `type`? `type` is more flexible, but `interface` is more strict therefore clear i.e. can't use union unlike `type`. `interface` can be used as a contract that a class will have to adhere to. 
```
class Person implements Greetable, AnotherInterface {
  name: string;
  age = 30;

  constructor(n: string) {
    this.name = n;
  }

  greet() {
    console.log(phrase + " " + this.name);
  }

}

let user1: Greetable;
user1 = new Person("Max");
```
- Can also add `readonly` into interfaces
```
interface Greetable {
  readonly name: string;
}
```
- extending interfaces with `extends`
```
interface Named {
  readonly name: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name: string;
  age = 30;

  ...
}
```
- interfaces as function types
```
// type AddFn = (a: number, b: number) => number;
interface AddFn {
  (a: number, b: number): number;
}

let add: AddFn;

add = (n1: number, n2: number) => {
  return n1 + n2;
}
```
- Optional methods, parameters and properties with `?`
```
interface Named {
  readonly name: string;
  outputName?: string;
  myMethod?() { ... }
}
```

## Advanced Types
- intersection types combines two types like interface inheritance
```
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  startDate: Date;
};

// interface ElevatedEmployee extends Employee, Admin {}

type ElevatedEmployee = Admin & Employee;
```
- type guard is the idea of checking a certain property or method exist before you use it. Code below uses `typeof`
```
type Combinable = string | number;
type Numeric = number | boolean;

function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string" ) {
    return a.toString() + b.toString();
  }
  return a + b;
}
```
- use type guard to check if there is a property in an instance with `in`
```
const e1: ElevatedEmployee = {
  name: 'Max',
  privileges: ['create-server'],
  startDate: new Date()
};

type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnkonwnEmployee) {
  if ('privileges' in emp) {
    console.log("Privileges: " + emp.privileges);
  }
}
```
- Discriminated Unions. there is one common property in every object. note the `type` is the only common property in the interfaces below. Without it, it is difficult to have type guards.
```
interface Bird {
  type: 'bird';
  flyingSpeed: number;
}

interface Horse {
  type: 'horse';
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  let speed;
  switch (animal.type) {
    case 'bird':
      speed = animal.flyingSpeed;
      break;
    case 'horse':
      speed = animal.runningSpeed;
  }
  console.log('Moving at speed: ' + speed);
}

moveAnimal({type: 'bird', flyingSpeed: 10});
```
-typecasting `<HTMLInputElement>` or `as HTMLInputElement`
- the exclamation `!` means the result will never yield null.
```
// const userInputElement = <HTMLInputElement>document.getElementById('user-input')!;
const userInputElement = document.getElementById('user-input');

if (userInputElement) {
  (userInputElement as HTMLInputElement).value = 'Hi there!';
}

interface ErrorContainer { // { email: 'Not a valid email', username: 'Must start with a character!' }
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: 'Not a valid email!',
  username: 'Must start with a capital character!'
};
```
- Index properties or index types solves the problem where I don't know in advance which property or how many is inside. Need an object where I am pretty clear on the key type and value type. `[prop: string]: string;`
```
interface ErrorContainer { // { email: 'Not a valid email', username: 'Must start with a character!' }
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: 'Not a valid email!',
  username: 'Must start with a capital character!'
};
```
- function overloads helps typescript infer the return type in your function
```
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}

const result = add('Max', ' Schwarz');
```
- optional chaining, checking if object or property exists with `?`
```
const fetchedUserData = {
  id: 'u1',
  name: 'Max',
  // job: { title: 'CEO', description: 'My own company' }
};

console.log(fetchedUserData?.job?.title);
```
- Nullish coalescing with `??`
```
const userInput = undefined;

const storedData = userInput ?? 'DEFAULT';

console.log(storedData);
```





