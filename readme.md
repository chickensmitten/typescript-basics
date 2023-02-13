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
- types in ts: number, string, boolean

## 