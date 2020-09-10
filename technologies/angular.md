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
<img src="/assets/images/AngularBigPicture.png" alt="drawing" width="400" height="300"/>
  
**HTML template** --> Derectives/child componenets to do the redering       
**Selector** = ```<app-custormer></app-custormer>```

### Componenets and Modules
- **Imports** - ES2015 import statements
- **Decorators** - Matadata about the components. ```@Componenet({selector:..,templateUrl:..})```
  - **selector** - tag name for this componenet.```selector:app-custormers```
  - **template** - ```templateUrl:./customers/componenet.html```
- **Class** - Get data in and out of the template thats rendered in the UI. ```export class CustomersComponenet```. Another componenet can import.

### main.ts
First thing that really fires up at runtime is **main.ts**. (Normally don't have to mess with this much).
```typescript
platformBrowserDynamic().bootstrapModule(AppModule).catch(err => console.log(err)); // Whats the first bucket(module) to fire up ?
```
### App Module
```typescript
import { NgModule }      from '@angular/core';     // is a decorator
import { BrowserModule } from '@angular/platform-browser';  //has Derectives functionality used in databinding
import { AppComponent }  from './app.component';
@NgModule({
  imports:      [ BrowserModule ],   // We get another bucket(module) that angular provides.
  declarations: [ AppComponent ], // This is declaring what is inside this module.
  bootstrap:    [ AppComponent ]  // Whats the first Componenet to be fired up? The first to be displayed in the UI?
})
export class AppModule { }
```
### index.html
```html
   <app-root>   <!-- UI renders this. Has to be declared in an angular componenet -->
       Loading...
   </app-root>
```
### App Componenet

```typescript
@Component({ 
  selector: 'app-root',
  template: `
    <h1>{{ title }}</h1> //use double curly braces (interpaltion) to push data from code to HTML template.
  `
})
export class AppComponent implements OnInit { 
title = HelloWorld
...
}
```




