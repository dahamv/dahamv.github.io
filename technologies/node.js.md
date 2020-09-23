**[Home](../index.md)**  

## NodeJS
- Its a javascript runtime. To run javascript on serverside. Its not a framework like expressJS.  
- NPM - Node package manger to install 3rd party libraries.  
- The installed packages are store in node_modules folder   
- All dependencies are listed in package.json   
- NPM scripts can be created to run certain tasks like - run a server.   

### Nodes Event loop
- Single threaded
- Supoorts concurrency via events and callbacks
- EventEmitter class is used to bind events and listners.   

CommonJS way to import modules -> **const Person = require('./person')**;   
ES6 way -> **import Person from './person'**;   

## REPL
Read Eval Print Loop - represents console to run javascript

## Wrapper functions
All code you write in Node will be wrapped in functions.
```js
//Module wrapper function.
(function (exports, require, module, __filename, __dirname) {
//my code has access to these 5 variables.
})

eg:
module.exports=myClass;
console.log(__dirname, __filename);
```
## Node core modules
All node.js core modules can be fount at - https://nodejs.org/docs/latest-v13.x/api/  
- Path module - ```const path = require('path');``` to deal with file paths
- FS module - to deal with filesystem. CRUD files/folders etc. The methods eg: ```mkdir()``` are async so takes callback fn. There is the sync versions of methods as well.
- OS module - gives info about the OS.
- url module - analizes a given url. (get the hostname/parameters etc. and add parameters.)
- Events module
   - can create Events using EventEmiters
