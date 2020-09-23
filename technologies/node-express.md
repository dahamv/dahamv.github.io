**[Home](../index.md)**  

# NodeJS
- Its a javascript runtime. To run javascript on serverside. Its not a framework like expressJS.  
- NPM - Node package manger to install 3rd party libraries.  
- The installed packages are store in node_modules folder   
- All dependencies are listed in package.json   
- NPM scripts can be created to run certain tasks like - run a server.   

## Nodes Event loop
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
   
   ```js
      const EventEmiter = require('events');
      //JS ES6 class
      class MyEmiter extends EventEmiter {
      
      }
      //Init Emitter object
      const myEmitter = new MyEmiter();
      //EventListner 
      myEmitter.on('myEvent', () => console.log('Evnet Fired!')); // register an event named myEvent with its callback()
      //Init event
      myEmitter.emit('myEvent'); // will fire event and the callback() will run -> Event Fired! printed on console.
   ```
- Http Module - can create an HTTP server.
   ```js
      const http = require('http');
      http.createServer((req, res) => {
         res.write('Hello World');
         res.end();
      }).listen(5000, () => console.log('Server running ...'));
   ```
   
# Express JS

```npm install express```   
A framework that runs on node.js   

```js
//in server.js - added to the script in package.json as the start script.
const express = require('express');
const app = express();
// create the route for the / for a GET.
app.get('/', (req, res) => {
   res.send('<h1>Hello World!</h1>');
});
const PORT = process.env.PORT || 5000; // first check the env var for the port number.
app.listen(PORT, () => console.log(`Server started on port: ${PORT}`))
// Now can run the express with command : node server
```

## Midleware functions in Express (like mediators in Synapse)

```js
...
const app = express();
//middleware function. All middleware funcitons are put on a stack.
const logger = (req, res, next) => {
   console.log('Hello ');
   next(); // You have to call the next middleware function on the stack.
}
```

## Router
To handle HTTP requests.
```js
//in membersapi.js file add the routes to handle GET, POST etc.
const express = require('express');
const router = express.Router();
const members = require(./members); //in memebers.js file it has all members in an array : const members = [...]
//handle GET request
router.get('/', (req,res) => res.json(members));//send back all members in JSON array
module.exports = router; //have to export it to use from the app.

//in index.js or server.js
...
//members api routes.
app.use('/api/members', require('membersapi');
```
