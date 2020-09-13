**[Home](../index.md)**  
- TypeScript is a supperset of JavaScript.
- Browsers don't understand Typescript. Only Javascript. So have to be compile (transpiled) to Javascript.
- Java: Stong static typing. Typescript: has Strong-static typing is optional. But its useful to catch a lot of bugs in compile time. Its useful to debug.
- Typesript supports Object oriented features: **Interfaces, Classes, Constructores, Access Modifires (pulic/private), Fields, Properties, Generics, Enums etc.**
- Good support by IDEs like Vscode.

```npm install -g typescript```. You can create a **main.ts** file and type any javascript code and compile it with typescript compiler ```tsc main.ts``` and it will create a **main.js** file and it will have the exact same code since any **valid javascript code is valid typescript code.** Run the javascript file with ```node main.js```   
When an Angular application is run using ```ng serve``` under the hood it calls ```tns``` and compiles to Javascript.  

## Variable declarations
Can use **var** keyword like in Javascript ES5. And use **let** keyword from Javascript version ES6.  
ES5 doesn't have **let**. **tsc** copiler compiles all typescript code to javascript ES5.       
when **var** is used within a ```for(var i,..)``` block. Its available outside the for(..) block *within the function*. But ```for(let i,..)``` is only available within the for() block. TypeScript supporting IDEs detects this when a **let** variable is used outside the scope.  
```typescript
//You can do this.
var a = 1;
a = 'Hello';
//Compile Error
let a = 1;
a = "Hello";
```
Other Javascript versions.
- ES5 (ECMAScript 5)
- ES6 (2015)
- ES2016
- ES 2017

## TypeScript Types and Enums
```typescript
let a : number;  let b : boolean ..... , string , any
let c : number[] = [1,2,3];

//Enums
//in Vanila Javascript
const colorRed = 0; const colorGreen = 1; const colorBlue = 2;
//with Typescript Enums
Enum Color = { Red, Green, Blue } // Red, Green, Blue automatically gets incremented values 0, 1, 2
let backgroundColor = Color.Red;
```
## Type assersions
```typescript
let message = 'abc'; // message is of type string. So can call String specific functions like message.endsWith(..) etc.
//and
let message; // this is of type any. So typescript gets confused with message.endsWith(..) is called
message = 'abc';
// For this use type assersions.
let endsWithC = (<string>message).endsWith('c'); // returns boolean.
// can also use
let endsWithC2 = (message as string).endsWith('c') // does the same thing.
```
## Typescript Arrow functions
In the above example, **sum** is an arrow function, **"a: number, b: number"** is a parameter type, **"number"** is the return type, the arrow notation **=>** separates the function parameter and the function body.
```typescript
let sum = (a: number, b: number): number => {  
            return a + b;  
}  
console.log(sum(20, 30)); //returns 50  
//JavaScript way
let log = function(message) { console.log(message); }
//Can do with arrow functions
let doLog = message => console.log(message);
// if there are no parameters
let doLog2 = () => console.log('hello');
```

## Custom types
```typescript
let drawPoint = (x,y) => { ... }
// can also pass a point object instead of x and y
let drawPoint = (point) => { ... }  // But problem of this is you can call drawPoint('Some String');
//you can call that function
drowPoint({ x : 1, y : 2 });
// Its a good idea to annotate the input 'point' parameter (inline annotation) with a custom type.
let drawPoint = (point : { x : number, y : number }) => { ... } 
```
