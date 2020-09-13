**[Home](../index.md)**  
Introduced in Javascript ES6
Arrow functions AKA lambda functions.
```javascript
const add = (x, y) => { return x + y;};
// You don't have to add return here since its implisit. Paranthisies are not needed.
const identity = x => x;
//three dots ... means the function takes a variable number of arguments.
const array = (...elements) => { return elements; };
array(1,2,3); // this returns [1,2,3]

const log = (...args) => { console.log(...args); };
log('Hello',' ','World','!'); // Hello World!
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
