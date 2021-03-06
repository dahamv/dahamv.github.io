**[Home](../../index.md)**  

Javascript is not a classbased language, its a prototype based language. 
## Javascript prototypes
Every Javascript obj will have a prototype object attached. Its gonna have all the default things that parent obj has. The prototype object can have a prototype object.    
obj -> prototype obj -> its prototype obj -> ... -> parent Object prototype (its prototype will be null).    
eg: methods like **toString()**, **valueOf()**, **isPropertyOf()**, **hasOwnProperty()** etc.
```js
let obj1 = {
 prop1 = () => console.log('prop1');
}
obj1.prop(); // will print 'prop1'
//Eventhogh toString() is not a function defined, we can still call it
obj1.toString(); //This wont throw an error since its attached to the obj prototype.
obj1.prop2(); // will throw an error after going throug the prototype chain.
let obj2 = {
 prop2 = () => console.log('prop2');
}
//You can set the prototype of an obj.
Object.setPrototypeof(obj2, obj1);
//Now you can call
obj2.prop1();
//To list all numarable properties of an object upto its parent prototype Object.
for (let prop in obj2) {
    console.log(prop); // output: prop2 prop1
}
//To list all properties of the parent prototype object
let propList = Object.getOwnPropertyNames(Object.getPrototypeOf(obj1));
console.log(propList);
```

## this in Javascript
In a function definition, **this** refers to the "owner" of the function.   
A functin inside an object is a method.  **this** in that method refers to the obj.
in a regular function (not a part of an obj) -> **this** referes to the global object -> **window** object in browsers and **global** in node.
```js
const video = {
    title: 'a',
    play(){ console.log(this);}
};
vidio.play(); //prints the video object

video.stop = () => console.log(this);
video.stop(); //will still print the video object. since stop is a method of video obj.

const playVideo = () => console.log(this); 
playVideo();//prints the window object.

//A constructor function
const Video = (title) => {
    this.title = title;
    console.log(this);
}
const v = new Video('b'); // new keyword creates a new empty object {} and make 'this' of the consctructor fuction to point to the empty object.

//in callback functions since they are regular functions this referes to the window object.
const video = {
    title : 'a',
    tags : ['a','b','c'],
    showTags() {
        //this -> video obj
        this.tags.forEach(tag => console.log(this.title, tag));  //Error: last this referes to window obj since its an anonymous function
        //solution
        this.tags.forEach(tag => {console.log(this.title, tag)} , this); // in the forEach function we can pass the 'this' of video obj to the this of the anonymous fn.
    }
}
```
## Inheritance in Javascript

```js
//Constructor function. Conventianally first letter Uppercase.
Book = (title, author, year) => {
 this.title = title;
 this.author = author;
 this.year = year;
}
//A prototype function
Book.prototype.getSummery = () => {
//Using Template literal with back ticks. Part of ES6. No need concatnation with + sign.
 return `${this.title} was written by ${this.author} in year ${this.year}`;
}
//Inheritance on Magazine constructor fucntion.
//So here Magazine inherits Book.
Magazine = (title, author, year, month) => {
 Book.call(this, title, author, year); //With call() an object can use a method belonging to another object. Call Book fn on Magazine obj.
 this.month = month; //Now Magazin obj will have all 4 variables.
}
//Instantiate - but this won't have the prototype function.
const mag1 = new Magazine('Mag1','John','2020','Jan')
//Inherite prototype
Magazine.prototype = Object.create(Book.prototype); // Now a new Magazine obj can be created and can call getSummery on it.
//To change prototype constructor from Book to Magazine use
Magazine.prototype.constructor = Magazine;
```

## Object.create() ES5

```js
//Create the prototypes first
const bookProtos = {
 getSummery: () => {...logic...},
 getAge: () => {...logic...}
}
//Create object
const book1 = Object.create(bookProtos);
book1.titile = 'Book1';
book1.author = 'John'
book1.year = '2020'
```

## Classes ES6
Same stuff above can be done with classes. No need to deal with prototypes.

```js
class Book {
 constructor(title, author, year) {
  this.title ......
 }
 getSummery() {....}
 //Can also have static methods
 static topBookStore() {...} //call Book.topBookStore();
}

//Can have subclasses
Magazine extends Book {
 constructor(title, author, year, month) {
  super(titile, author, year);
  this.month = month;
 }
}
```

## Promises ES6
First of all, a Promise is an object. There are 3 states of the Promise object:

- **pending**: initial state, neither fulfilled nor rejected.
- **fulfilled**: meaning that the operation was completed successfully.
- **rejected**: meaning that the operation failed.

```Promise.resolve(value);``` and ```resolve``` patermeter - annonymous function in a Promise  ```new Promise((resolve, reject) => {...});``` are different.
- ```Promise.resolve()``` takes in any value or a Promise and **returns a/the Promise**.
- ```Promise.prototype.then(fn1,fn2)``` method takes in 2 callback functions for the **fulfilled** and **rejected** and **returns a Promise**. fn1 is mapped to ```resolve```

```js
let promise1 = new Promise((resolve, reject) => {
  //Using setTimeout(...) to simulate async code. Note: setTimeOut() DOES NOT WAIT !!!
  setTimeout( function() {
    resolve("Success!")  // call the resolve (fn1) annonymous function
  }, 250) 
}) 
//debug: At this point (before calling then) promise1 prototype values are [[PromiseState]]: "pending" [[PromiseResult]]: undefined
promise1.then((successMessage) => {
  // this annonymous fn is what's passed in as resolve above. So "Success!" is passed in as successMessage.
  console.log("Yay! " + successMessage) 
});

//promise2 is an already resolved Promise. beause of Promise.resolve();
var promise2 = Promise.resolve(17468);  
//debug: promise2 objects protype values have: [[PromiseState]]: "fulfilled" [[PromiseResult]]: 17468
promise2.then(function(val) { //debug: the resolved value 17468 is passed as val
    console.log(val); 
});

//debug: until 3 secs from calling .then() promise3 is in pending state. Since it takes 3 secs to call resolve annonymous fn1.
const promise3 = new Promise((resolve, reject) => { 
      setTimeout(() => { 
          //debug: when resoleved is called after 3 secs, promise 2 is in fulfilled state.
          resolve([89, 45, 323]); 
      }, 3000); 
  }); 
//debug:The resolved array is passed into fn1 in then(fn1,..)
promise3.then(values => { 
    console.log(values[1]); 
});  
 ```
 
### Return value of Promise.prototype.then(fn1, fn2)

**Returns a Promise** according to the following rules.        
If a handler function fn1 or fn2 (on fulfilled or on rejected):      

- returns a **value**, the **returned promise** gets resolved with the returned value as its value. ```[[PromiseState]]: "fulfilled" [[PromiseResult]]: value```
- doesn't return anything, the **returned promise** gets resolved with an undefined value. ```[[PromiseState]]: "fulfilled" [[PromiseResult]]: undefined```
- throws an error, **returned promise** gets rejected with the thrown error as its value. ```[[PromiseState]]: "rejected" [[PromiseResult]]: Error```
- returns an already fulfilled promise, **returned promise** gets fulfilled with that promise's value as its value. ```[[PromiseState]]: "fulfilled" [[PromiseResult]]: callbackPromiseVal```
- returns an already rejected promise, **returned promise** gets rejected with that promise's value as its value. ```[[PromiseState]]: "rejected" [[PromiseResult]]: callbackPromiseVal```
- returns another pending promise object, the resolution/rejection of the **returned promise** will be subsequent to the resolution/rejection of the promise returned by the handler. Also, the resolved value of the **returned promise** will be the same as the resolved value of the promise returned by the handler. ```[[PromiseState]]: "pending" [[PromiseResult]]: undefined```

```js
//Rightaway resolved with 'foo'
Promise.resolve('foo')
  //Above resolved value 'foo' is passed in to the below annonymous function.
  .then(string => {
    return new Promise(function(resolve, reject) {
      setTimeout(function() {
        string += 'bar';
        console.log("First Then: " + string)
        resolve(string);
      }, 1);
    });
  })
  // Following fn1 callback is called only when above returned promise is resolved.
  // so the  'foobar' is passed into the below fn1 callback
  .then(string => {
    //Remember: setTimeout doesn't wait.
    setTimeout(() => {
      string += 'baz';
      console.log("Second Then; " + string);
    }, 1)
    return string; //so straight away returned befor 'baz' is added.
    //So the returned promise from then() is resolved with the returnd value. i.e. 'foobar'
  })
 //Since above promise is resolved with 'foobar' its passed into the following fn1 callback
  .then(string => {
    console.log("Last Then: " + string);
  });

//Output is:
//First Then: foobar
//Last Then: foobar
//Second Then; foobarbaz
```
## Promise.all()
Instead of calling f1.then(f2).then(f3)....then(fn) .... you can do ```Promise.all()```

```js
const promise1 = Promise.resolve('Hello World');
const promise2 = 10; //in Promise.all() it is considered as a promise resolved to 10.
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 2000, 'Good Bye'));

Promise.all([promise1, promise2, promise3]).then(values => console.log(values));
```
## Async / Await
A special syntax to work with promises easily.    
an **async** function always returns a promise. Other values are wrapped in a resolved promise automatically.

```js
async function f() {
  return 1;
}
f().then(alert); // 1
```

Without calling ```promise.then()``` we can use **await**. it makes JavaScript wait until that promise settles and returns its result.   
```js
const promise1 = new Promise((resolve, reject) => 
            setTimeout(()=> resolve('Hello World'), 2000));
(async printHelloWorld => {
  const data = await promise1;
  console.log(data);
})();                          
```

## Callback Hell
Asynchronous code is simulated with the setTimeout() function.   
```js
const myFridge = ['Fresh Beef'];
const myOven = ['Soft buns'];

const getBeef = callBack => {
  const beef = myFridge[0];
  setTimeout(()=> {console.log(beef); callBack(beef);}, 2000);
};

const cookBeef = (beef, callBack) => {
  const cookedBeef = 'Cooked the '+ beef;
  setTimeout(()=> {console.log(cookedBeef); callBack(cookedBeef);}, 3000);
};

const getBuns = callBack => {
  const buns = myOven[0];
  setTimeout(()=> {console.log(buns); callBack(buns);}, 1000);
}

const putBeefBetweenBuns = (buns, beef, callBack) => {
  const burger = 'Nice burger made with ' + beef + ' put between ' + buns;
  setTimeout(()=> {console.log(burger); callBack(burger);}, 1000);
}

const serveBurger = (burger) => {
setTimeout(()=> console.log(burger + ' is served!'), 1000);
}

//Callback hell.
(() => {
  getBeef((beef) => {
    cookBeef(beef, (cookedBeef) => {
      getBuns((buns) => {
        putBeefBetweenBuns(buns, cookedBeef, (burger) => {
          serveBurger(burger);
        });
      });
    });
  });
})();

```
### Chaining Promises
The then method returns a Promise which allows for method chaining.    

If the function passed as handler to then returns a Promise, an equivalent Promise will be exposed to the subsequent then in the method chain.   
    

```js
const myFridge = ['Fresh Beef'];
const myOven = ['Soft buns'];
const getBeef = () => {   
  return new Promise((resolve, reject) => {
    if (myFridge) {
      const beef = myFridge[0] + ' form Fridge';     
      setTimeout(()=> {console.log(beef);resolve(beef)}, 2000);
    } else {
      reject(new Error('No more beef!'));
    }
  });
};

const cookBeef = (beef) => {  
  return new Promise((resolve, reject) => {
    if (beef) {
      const cookedBeef = 'Cooked '+ beef;
      setTimeout(()=> {console.log(cookedBeef);resolve(cookedBeef)}, 3000);
    } else {
      reject(new Error('Cannot cook Beef!'));
    }
  });
};

const getBuns  = () => {  
  return new Promise((resolve, reject) => {
    if (myOven) {
      const buns = 'Got ' + myOven[0];
      setTimeout(()=> {console.log(buns);resolve(buns)}, 1000);
    } else {
      reject(new Error('Cannot get buns'));
    }
  });
};

const putBeefBetweenBuns = (buns, beef) => {
  return new Promise((resolve, reject) => {
    if (buns && beef) {
      const burger = 'Nice burger made with ' + beef + ' put between ' + buns;
      setTimeout(()=> {console.log(burger);resolve(burger)}, 1000);
    } else {
      reject(new Error('Cannot do the making'));
    }
  });
}

const serveBurger = (burger) => {
  setTimeout(()=> console.log(burger + ' is served!'), 1000);
}

//Since you have to wait for two promises to complete. Promise.all waits until all promises are completed.
Promise.all([getBeef().then(cookBeef), getBuns()])
              .then(data => putBeefBetweenBuns(data[0],data[1]))
              .then(serveBurger);
              
//Using Async and Await
const makeBurger = async () => {
  const beef = await getBeef();
  const cookedBeef = await cookBeef(beef);
  const buns = await getBuns();
  const burger = await putBeefBetweenBuns(cookedBeef, buns);
  return burger;
};

// Make and serve burger
makeBurger().then(serveBurger);
```
