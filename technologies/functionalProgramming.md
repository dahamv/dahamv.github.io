**[Home](../index.md)**  

## Arrow functions AKA lambda functions.
https://www.vinta.com.br/blog/2015/javascript-lambda-and-arrow-functions/

Introduced in Javascript ES6
```javascript
const add = (x, y) => { return x + y;};
// You don't have to add return here since its implisit. Paranthisies are not needed.
const identity = x => x;
//three dots ... means the function takes a variable number of arguments. This is called rest operator
const array = (...elements) => { return elements; };
array(1,2,3); // this returns [1,2,3]

const log = (...args) => { console.log(...args); };
log('Hello',' ','World','!'); // Hello World!
```

## rest operator

```javascript
const langs = ['JS', 'Java', 'Python'];
const [js, ...rest] = langs;
js === 'JS';
rest[0] === 'Java';
rest[1] === 'Python';
```
```javascript
const head = ([x]) => x; // This head function takes an array and returns the first item in that array;
head([1,2,3])===1 // true
```

## default values for functions
```javascript
const greet = (name, greeting = 'Hi') => {console.log(greeting, name);};  
console.log(greet('daham', 'hello'));
```

```javascript
let obj = {}; //this is an empty object
//Object prototype
//The prototype is an object that is associated with every functions and objects by default in JavaScript, 
//where function's prototype property is accessible and modifiable and object's prototype property (aka attribute) is not visible.
function Student() {
    this.name = 'John';
    this.gender = 'M';
}
Student.prototype.age = 15; // can also asign a function.
//now you can access age.
var studObj1 = new Student();
console.log(studObj1.age); // 15
```

## Returning functions

```javascript
//Note that x get stored in the first function. This is a good way to avoid repetition.
const createAdder = (x) => {
  return (y) => x + y;
}
const add3 = createAdder(3); // 3 is stored inside the add3 function 
const add4 = createAdder(4); // 4 is stored insode the add4 function
add3(5); // returns 8
add4(5); //returns 9
```

## Bad functional programming vs Good
```javascript
//BAD since statefull (mutable - subject to change)
let name = 'My Name';
const getName = () => name;
const setName = (newName) => name = newName;
const printUpperName = () => console.log(name.toUpperCase());
printUpperName(); //MY NAME

//GOOD since stateless (immutable - the logic doesn't change. only inputs
const upperName = (name) => name.toUpperCase();
console.log(upperName('Jerremy')); // JERREMY
console.log(upperName('Jet')); //JET
```
## Self executing functions
```javascript
//In this way function name() can be called from any place. If there is another library
//calling name() function, then its a problem.
var userName = "Sean";
console.log(name());
function name() { return userName;}

//Now function name can't be called outside of main(). But main() can be called from anywhare. 
//its a low chance but still a problem.
function main() {
  // We are now in our own sound-proofed room and the external libarary's name() function 
  //can exist at the same time as this one. 
  var userName = "Sean";
  console.log(name());
  function name() { return userName;}
}
main(); // have to call main function. otherwise this code is not executed.

//Better way to do this is by using self executing functions. The syntax is ((){})();
(function main() {
    var userName = "Sean";
    console.log(name());
    function name() { return userName;}
  }
)();
```
