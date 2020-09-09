**[Home](../index.md)**  
# Angular

## Course Agenda
- **Componenets** - For the UI  
- **Services and Http** - For any reusable code. eg. Fontend calculations / validations / Http calls to server. / use websckets with the server.
- **Modules** - Like buckets to organize the code.( to put services and components in - they are going to be registers/declared with a Module ). AppModule is the main module. 
- **Data Binding** - get data in and out of the screen
  - input and output properties - eg parent and child componenet  
- **Routing** 
  - load different screens dynamically without refreshing the entire page.
  - How Routes can be defined /  How routes are registered with modules / how to use routes to load different componenets dynamically.

## Angular project files overview
  
 - **tslint.json** - To define coding rules for the linter (code analysis tool) to drive consistency across teams (eg. use === instead of ==)
 - **tsconfig.json** - Typescript compilation settings.
 - **protractor.json** - for end-end testing and goes hand-in-hand with e2e folder. (Used by QA team)
 - **package.json** - dev dependencies and runtime dependencies.
 - **package-lock.json** - created whith ```npm install``` and will have all the scripts and tools needed. It locks down the exact versions that got installed so that when another person gets the source code he will also get the same versions of the dependencies. Don't usually mess with this file.
 - **karma.conf.json** - for unit testing. *Karma* is a test runner.
 - **.editerconfig** - config for code editers. Some editers understand this and some don't. ( eg: Do I want spaces vs tabs when I press Tab)
 - **angular-cli.json** - Has things like - source code root folder/ where the build files should go to / whats the home page(index.html) / whats the first script that starts up (main.ts)
 
## Big picture 
![](/assets/images/AngularBigPicture.png =250x300)
App Module --> Routes (to dynamically load ) --> Componenets -->> HTML template and Typescript Code  
Typescript Code <--> Services  
HTML template --> Derectives/child componenets to do the redering   
App Module <-- Other modules that angular provides.
