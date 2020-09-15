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
# Functional Programming

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
## Returning functions ( Higer order pattern in functional programming )

```javascript
//Note that x get stored in the first function. This is a good way to avoid repetition.
const createAdder = (x) => {
  return (y) => x + y;
}
//can write the above function with one line using => functions. This is called CURRYING in functional programming
const createAdder = x => y => x + y;

//same thing in old ES5 javascript
function createAdder(x) {
    return function (y) { return x + y ;}; //returns an annonymous function which takesin y.
}

const add3 = createAdder(3); // 3 is stored inside the add3 function 
const add4 = createAdder(4); // 4 is stored insode the add4 function
add3(5); // returns 8
add4(5); //returns 9

//Same thing with the function.bind() - 
var sum = function(a, b) {
  return a + b;
};
var add5 = sum.bind(null, 5); // with bind() null --> a, 5 --> b and the new function add5(a) will return a + 5
add5(7); //returns 12
```

## Using Partial functions

```javascript
const add = (x,y) => x + y;
const partial = (fn,...args) => fn.bind(null, ...args); //null --> 1st arg, ...args --> 2nd arg of fn
const add4 = partial(add, 4);
console.log(add4(2)) //returns 6
//Another way
const partial = (fn,...args) => {
    return (...otherArgs) => { return fn(...args, ...otherArgs)}
}
```

## Complex usage of Curring in functional programming

```javascript
//These are called curried functions in functional programming.
const map = fn => array => array.map(fn);
const multiply = x => y => x * y;
//you give pluck(key) a key and it gives you back a fn the expects an object
const pluck = key => object => object[key];

const discount = multiply(0.98); //2% discount.
const tax = multiply(1.0925); // 9.25% tax

const request = defaults => options => {
    options = Object.assign({}, defaults, options);
    console.log(options);
    return fetch(options.url)
            .then(resp => resp.json())
            .catch(error => console.log(error));
}
const customRequest = request({
    headers: {'X-Custom':'myKey'}
});

customRequest({ url: 'https://dahamv.free.beeceptor.com/price'}) //returns the json [{"price": 5}, {"price": 10}, {"price": 3}] from endpoint.                       
                        //Attahing then(fn) callbacks for the promise.
                        //map expects a fn. So give it the fn to pluck out the price.
                        .then(map(pluck('price'))) // gives [5,10,3]
                        .then(map(discount)) // gives [4.9, 9.8, 2.94]
                        .then(map(tax)) // gives [5.35, 10.71, 3.21]
                        .then(data => console.log(data))
                        .catch(function(error) {console.log(error)}); 
````
## Composing Closures. i.e. The output of f(x) is input of the g(x) -> g(f(x))
In the above example map() is calld 3 times. It iterates the array 3 times. Its expensive. So to do it at once..
```javascript
//create a compose function
compose(tax, discount, pluck('price'));
```

## Optimizing recursive call functions.

```javascript
//not optimised
const factorial = (n) => {
        if(n < 2) return 1;
        //To complete the multiplication step the recursive function should return.
        // So the called function is put in call stack. It will give Stack overflow error for large numbers.
        return n * factorial(n-1); 
}

//optimized. This is called Tail call optimization.
const factorial = (n, accum = 1) => {
    if(n < 2) return accum;
    return factorial(n-1, n * accum);
}
```
