**[Home](../../index.md)**  
Javascript is not a classbased language, its a prototype based language. 
## JS prototypes
Even JS obj will have a prototype object attached. Its gonna have all the default things that parent obj has. The prototype object can have a prototype object.    
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

## This in JS
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
## Callbacks

```js
const posts = [
    {title:'post 1', body: 'post body 1'},
    {title:'post 2', body: 'post body 2'}
];
getPosts = () => setTimeout(()=>posts.forEach((post)=>console.log(post)),1000); //console log posts after 1 sec
//use a callback in createPost.
createPost = (post, callback) => setTimeout(()=>{
                                        posts.push(post);
                                        callback();
                                        },2000);
const post3 = {title:'post 3', body: 'post body 3'};
createPost(post3,getPosts()); //console logs all 3 posts after 3 secs (1+2)
```
### Callback Hell
```js
const getBeef = nextStep => {
  const beef = 'Fresh Beef';
  console.log(beef);
  nextStep(beef);
};

const cookBeef = (beef, nextStep) => {
  const cookedBeef = 'Cooked the '+ beef;
  console.log(cookedBeef);
  nextStep(cookedBeef)
};

const getBuns = nextStep => {
  const buns = 'Soft buns';
  console.log(buns);
  nextStep(buns);
}

const putBeefBetweenBuns = (buns, beef, nextStep) => {
  const burger = 'Nice burger made with ' + beef + ' put between ' + buns;
  console.log(burger);
  nextStep(burger);
}

const serveBurger = (burger) => {
console.log(burger + ' is served!')
}

//Callback hell.
const makeBurger = nextStep => {
  getBeef(function(beef) {
    cookBeef(beef, function(cookedBeef) {
      getBuns(function(buns) {
        putBeefBetweenBuns(buns, beef, function(burger) {
          serveBurger(burger);
        });
      });
    });
  });
};
makeBurger();
```
## Promises
First of all, a Promise is an object. There are 3 states of the Promise object:

- **Pending**: Initial State, before the Promise succeeds or fails
- **Resolved**: Completed Promise ```.then()```
- **Rejected**: Failed Promise ```.catch()```

```js
const myFridge = ['Fresh Beef'];
const myOven = ['Soft buns'];
const getBeef = (fridge) => {   
  return new Promise((resolve, reject) => {
    if (fridge) {
      const beef = 'Got '+ fridge[0];
      console.log(beef);
      resolve(beef);
    } else {
      reject(new Error('No more beef!'));
    }
  });
};

const cookBeef = (beef) => {  
  return new Promise((resolve, reject) => {
    if (beef) {
      const cookedBeef = 'Cooked the '+ beef;
      console.log(cookedBeef);
      resolve(cookedBeef);
    } else {
      reject(new Error('Cannot cook Beef!'));
    }
  });
};

const getBuns = (oven) => {  
  return new Promise((resolve, reject) => {
    if (oven) {
      const buns = 'Got ' + oven[0];
      console.log(buns);
      resolve(buns);
    } else {
      reject(new Error('Cannot get buns'));
    }
  });
}

const putBeefBetweenBuns = (buns, beef) => {
  return new Promise((resolve, reject) => {
    if (buns && beef) {
      const burger = 'Nice burger made with ' + beef + ' put between ' + buns;
      console.log(burger);
      resolve(burger);
    } else {
      reject(new Error('Cannot do the making'));
    }
  });
}

const serveBurger = (burger) => {
console.log(burger + ' is served!')
}

const makeBurger = () => {
  const cookedBeef = getBeef(myFridge).then(cookBeef)
  .then((cookedBeef) => console.log(cookedBeef) );
  //console.log(cookedBeef);
  //.then(getBuns(myOven)).then(putBeefBetweenBuns);
};
```

```js
//Same code using promises. Leave getPosts as it is.
//no use of callbacks
createPost = post => {
    //A promise takes in a function with two parameters.
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            posts.push(post);
            const error = false; //if there is an error set this to true.
            //soon as the timeout is done, its gonna resolve. (if no error). Once it resolve, then it will call getPosts.
            if(!error) resolve(); 
            else reject('Error: something went wrong...');
        },2000);
    });
};

createPost(post3).then(getPosts);
//Error catching, if there is an error
const error = true;
...
createPost(post3).then(getPosts).catch(err => console.log(err));
```
**If f1, f2,f3...fn all return Promises we can do**     
```f1.then(f2).then(f3)....then(fn).then(()=> {//something to be done when fn resolves})```

## Promise.all()
Instead of calling f1.then(f2).then(f3)....then(fn) .... you can do ```Promise.all()```

```js
const promise1 = Promise.resolve('Hello World');
const promise2 = 10; // how the heck?
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 2000, 'Good Bye'));

Promise.all([promise1, promise2, promise3]).then(values => console.log(values));
```


```javascript
//insertion sort
var array = [5, 6, 3];
let insertionSort = (inputArr) => {
    let length = inputArr.length;
    for (let i = 1; i < length; i++) {
        let key = inputArr[i];
        let j = i - 1;
        while (j >= 0 && inputArr[j] > key) {
            inputArr[j + 1] = inputArr[j];
            j = j - 1;
        }
        inputArr[j + 1] = key;
    }
    return inputArr;
};

console.log(insertionSort(array));
