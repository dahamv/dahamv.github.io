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
- **Decorators** - Matadata about the components. ```@Componenet({selector:..,templateUrl:..})```. All decorators are functions.
  - **selector** - tag name for this componenet.```selector:app-custormers```
  - **template** - ```templateUrl:./customers/componenet.html```
- **Class** - Get data in and out of the template thats rendered in the UI. ```export class CustomersComponenet```. Another componenet can import.

### main.ts
First thing that really fires up at runtime is **main.ts**. (Normally don't have to mess with this much).
```typescript
platformBrowserDynamic().bootstrapModule(AppModule).catch(err => console.log(err)); // Whats the first bucket(module) to fire up ?
```
## App Module
______________
```typescript
import { NgModule }      from '@angular/core';     // is a decorator
//has Derectives functionality used in databinding. The other sub modules in the app can use this import.No need to import again.
import { BrowserModule } from '@angular/platform-browser';  
import { AppComponent }  from './app.component';
@NgModule({
  // We get another bucket(module) that angular provides.
  imports:      [ BrowserModule, , CustomersModule ],  
  // This is declaring what's inside this module. The registered Componenets in this Module.
  // We can declare Pipes, Componenets and Directives.
  declarations: [ AppComponent, FooComponenet ], 
  // Whats the first Componenet to be fired up? The first to be displayed in the UI?
  bootstrap:    [ AppComponent ]  
})
export class AppModule { }
```
### index.html
```html
   <app-root>   <!-- UI renders this first. Has to be declared in an angular componenet selector-->
       Loading...
   </app-root>
```
### App Componenet

```typescript
@Component({ 
  selector: 'app-root',
  //use double curly braces (interpaltion) to push data from code to HTML template.
  template: `
    <h1> \{\{ title \}\} </h1> 
    <app-customers></app-customers>
  `
})
export class AppComponent implements OnInit { 
title = HelloWorld
...
}
```

## Customers Module ( a seperate child module)
_______________________________________________
```typescript
import { NgModule }      from '@angular/core';
// Browsermodule is already inherited from App module.
//  CommonModule is where Derectives live in
import { CommonModule } from '@angular/common';
import { CustomersComponent }  from './customers.component';

@NgModule({
  imports:      [ CommonModule ],
  declarations: [ CustomersComponent ]
  //bootsrap is not needed. There should be only one 
  // export CustomersComponenet so that App component HTML template can use it as <app-customers></app-customers>
  exports: [ CustomersComponent ]
})
export class CustomersModule { }
```
### Customers Componenet
```typescript
@Component({
    selector: 'app-customers',
    templateUrl: './customers.component.html'
})
export class CustomersComponent implements OnInit {...}
```

## DataBinding
--------------
<img src="/assets/images/AngularDataBinding.png" alt="drawing" width="300" height="200"/>

-Write value using Interpolation: ```{{ propName }}```  
-Bind DOM properties using ```[property]``` or ```bind-property```  
-Bind DOM events using ```(event)``` or ```on-event```  

### Property and Event Binding
```typescript
@Componenet({metadata..})
class Customer {
firstName= 'John';
lastName= 'Doe';
details='Customer details ...'
isVisible=true; isActive=true; showIcon=true; isEnabled=true;
textColor=#f44336; // Red 
clickMe() { ..impl.. }
save() { ..impl.. }
}
```
```hidden``` is a DOM property. Use square brackets [] for propety biding. ```click``` is a DOM event. Use () for event binding. **clickMe()** is the callback function.
```HTML
<div [hidden]="!isVisible">
  <span (click)="clickMe()">
    Section3
  </span>
</div>
Interpolation: {{ customer.firstName }}
<button [disabled]="!isEnabled" [style.color]="textColor" (click)="save()"> Save </button>
<!-- add and remove a css class-->
<div [hidden]="!isVisible" [class.active]="isActive">...</div>
```

## Angular Directives
**ngFor, ngIf, ngClass, ngStyle, ngModel**  
Structural directives - changes the structure of the DOM. Directives with star \* eg: **ngFor, ngIf**
```HTML
<div \*ngFor="let customer of customers">
  <!-- active and icon are CSS classes. isActive and showIcon are variables in componenet class-->
  <span [ngClass]="{active: isActive, icon: showIcon}">{{customer.firstName}}</span>
  <span>{{customer.lastName}}</span>
</div>
<div *ngIf="customer">{{ customer.details }}</div>
<!-- two way databinding. name variable in componenet is shown in input fild. 
When its changed componenet property dynamically changes. -->
<input type="text" [(ngModel)]="customer.firstname" />
```

## Input properties (getters and setters) 
To get data from a parent component to a child component. **(Hey I as a child accept input from my parent)**
```typescript
import { Component, OnInit, Input } from '@angular/core';
//This is the app-customers-list child componenet of Customer Componenet.
export class CustomersListComponent implements OnInit {
    private _customers: ICustomer[] = [];
    @Input() get customers(): ICustomer[] {
      return this._customers;    
    }
    
    set customers(value: ICustomer[]) {
        if (value) {
            this.filteredCustomers = this._customers = value;
        }    
    }
```
Now from the parents HTML template we can pass the people array into customers property in the child componenet. uses the Setter method.
```HTML
<!-- customers is the input property in the child componenet-->
<app-customers-list [customers]="people"></app-customers-list>
```
## SimpleChanges
Usually when there are many properties that we have to check for changes. If there is only one, then use Input property getter and setter.
```typescript
import { Component, OnInit, Input, SimpleChanges } from '@angular/core';
export class CustomersListComponent implements OnInit {
    @Input() customers: any[];
    //watch for changes in a property.
    ngOnChanges(changes: SimpleChanges) { .. }
}
```

## Pipes
Using pipes in the HTML templeate. **uppercase** is an angular pipe to make all charactors to upercase. We can use that with | (pipe symbol)
```HTML
<a> {{ cust.name | uppercase }} </a>
<a> {{ cust.name | titlecase }} </a> <!-- first letter of words are uppercase -->
```

## Custom Pipes.
Creating **capitalize.pipe.ts** same behavior as with titlecase angular pipe.
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'capitalize' })
export class CapitalizePipe implements PipeTransform {
    transform(value: any) {
        if (value) {
            return value.charAt(0).toUpperCase() + value.slice(1);
        }
        return value;
    }
}

//The pipe should be registered in a Module so that other modules can import it.
@NgModule({
    declarations: [ CapitalizePipe ],
    exports: [ CapitalizePipe ]
})
export class SharedModule { }
```
You can pass values to Pipes. currency is an angular pipe.
```HTML
<td>{{ cust.orderTotal | currency:'USD' }}</td> <!-- you can hard code the currency code -->
<td>{{ cust.orderTotal | currency: curencyCode }}</td> <!-- curencyCode='USD' is a property in the class-->
<!-- symbol and code are angular currency pipe parameters.
<td>{{ cust.orderTotal | currency: curencyCode:'symbol' }}</td> <!-- use the symbol ($) for the currencyCode=USD -->
<td>{{ cust.orderTotal | currency: curencyCode:'code' }}</td> <!-- use the code (USD) for the currencyCode=USD -->
```

## Output, EventEmitter
EventEmitter is used to send data from a child up to a parent.
```typescript
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

//FilterTextBox is a child componenet of CustomerList componenet.
@Component({
    selector: 'filter-textbox',
    //Two way databinding. Value property of input filed should be <-- filter. The value got from the getter().
    //input event of input fild should be --> filter. The value set from the setter().
    //Can also use [(ngModel)] to do the same thing. its from FormsModule in angular.
    template: `
        Filter text box:        
        <input type="text" [value]="filter" (input)="filter=$event.target.value" />
        .....
        <input type="text" [(ngModel)]="filter" />
    `
})
export class FilterTextboxComponent implements OnInit {
    
    private _filter: string;
    @Input() get filter() {
        return this._filter;
    }
    
    set filter(val: string) { 
        this._filter = val;
        //Emits the changed data to the parent componenet as an event
        this.changed.emit(this.filter); 
    }
    //The characters typed in the Filter text box are pushed in to the customersList componenet using the event emitter..
    @Output() changed: EventEmitter<string> = new EventEmitter<string>();
    ....
}
```
When the filtering string is got from the FilterTextBox componenet, filterFunction() in CustomersList component should be called. 
```TypeScript
    //This is in CustomersList component. To process the data in the event sent by FilterTextBox child componenet
    filterFunction(data: string) {
        if (data) {
            //customers is an array ICustomer[]. It has all the customers. use the array filter() function to filter. And
            // fill the filteredCustomers ICustomer[] array.
            this.filteredCustomers = this.customers.filter((cust: ICustomer) => {
                return cust.name.toLowerCase().indexOf(data.toLowerCase()) > -1 ||
                       cust.city.toLowerCase().indexOf(data.toLowerCase()) > -1 ||
                       cust.orderTotal.toString().indexOf(data) > -1;
            });
        } else { //if nothing is typed in the filterTextBox(child componenet) show all the customers.
            this.filteredCustomers = this.customers;
        }
    }
```
In CustomersList HTML template.
```HTML
<!-- You can set a starting value to in the filterTextBox component. Uses the @Input setter().  -->
<filter-textbox [filter]="'Phoenix'"></filter-textbox>
<!-- the emited output event in Filtertextbox component is $event. call filterFunction($evnet) in parent
component CustomersList. this event object has the changed data sent by the child component. 
Note that changed is an event because of @Output. not a propery. so its event binding-->
<filter-textbox (changed)="filterFunction($event)"></filter-textbox>
```

## Services

Acts as a singleton. We don't declare services. We provide them. Service singletons are stored in the Angular injecter container.   
- Provider = cheff
- Injector container = waiter who delivers the food.

The waiter (Injecter container) will contact the cheff (the provider) and say "Hey I need a data service instance". He will pass the serivice singleton to the waiter (injector container). And then the waiter will pass that into who ever request the object.
```typescript
//The module which has the services registerd.
@NgModule({
    imports: [  ],
    providers: [ DataService, SorterService ]
})
export class CoreModule { }
...............

//The services should be injectable so that we can inject other services into the constructor.
@Injectable()
export class DataService {
    constructor(private http: HttpClient) { }
    //we need observables for async operations.
    getCustomers() : Observable<ICustomer[]> { }
}
```
## Typescript Arrow functions
In the above example, the sum is an arrow function, "a: number, b: number" is a parameter type, ": number" is the return type, the arrow notation => separates the function parameter and the function body.
```typescript
let sum = (a: number, b: number): number => {  
            return a + b;  
}  
console.log(sum(20, 30)); //returns 50  
```

## Obervables
Not part of angular, but from rxjs (reactive extention for java script). Used for Async operations. You can subscribe to observables. Its a wrapper arround some data source(stream of values). Can be **synchronous and asynchronous.**  
With HTTP observable observes for 1 response. But observables can be used with WebSockets and can observe a stream of data.   
Simily:    
Walking up to a convayer belt at airport. You are subscribing to the convayer belt. In case of HTTP there is only one piece of luggage that drops on the convayer belt. In case of WebSockets where data is pushed to the client by the server, that'd be like luggage flowing arround and we pick off what luggage we want and we ignore the luggage we dont
want as it flows arround the convayer belt. **We are observing the luggage as it flows by**   
You can also use **Promisses**. you can convert an Observable to a Promise. But Observables work in angular.  
### **Observer**s can subscribe to observables
Observers subscribe to Observables. An Observer has 3 methods. ```next(), error() and complete()```. Observable calls these methods accordingly. Observable calls the ```next()``` method when it gets a new value. Some observables never finish, so ```complete()``` is never called. eg. If the observable is wrapped arround a click listner on a button you don't know when it will finish, since the user could always click the button. 
```typescript
const myObservable = of(1, 2, 3);
// Create observer object
const myObserver = {
  next: val => console.log('Observer got a next value: ' + val),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

//The JavaScript way
var myObserver = {
  next: function(val) {console.log(val)},
  error: function(err) {console.log(err)},
  error: function() {console.log('completed')},
}
// Execute with the observer object
myObservable.subscribe(myObserver);
// Using arrow functions
myObservable.subscribe(value => console.log(value))
```

## pipe() function.
Simily:  
Think of it as a phisical pipe that water can flow through and as the water flows we can plug in different types of filters(in RxJs - pipable Operators). ```observableInstance.pipe(operator())``` eg: ```map(), catchError()```
see [list of RsJs operators](https://rxjs-dev.firebaseapp.com/guide/operators)
