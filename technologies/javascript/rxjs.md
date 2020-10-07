**[Home](../../index.md)**  
# RXJS - Reactive Extention for JS
Reactive Programming

- Works with asynchronous data streams
- Data streams can be created for
  - UI Events
  - Http requests
  - File systems
  - Array like objects
  - Memory/Cache
  
## Streams
<img src="/assets/images/rxjs-stream.png" alt="drawing" width="600"/>

## Observables and operators
In short, an observable is simply a **function** that are able to give multiple values over time, either synchronously or asynchronously.   
operators are functions that return Observables. There are two types of operators    
- Pipable operators - eg: ```map(), filter()```. Can be piped with ```myObserver.pipe(operator())```     
		 they take one Observable as input and generate another Observable as output      
- Creation operators - standalone functions to create a new Observable. eg: ```of(1,2,3), from(['a','b','c'])```

## Examining data streams from events

- Can create observables from Array like objects - arrays, sets, maps 
  ```js
  //adding $ for a stream variable is a convention.
  const array$ = from(['a','b','c']);
  //f1- run for all events
  //f2- run if error
  //f3- run when completed.
  array$.subscribe(f1,f2,f3); 
  ```
- Observables from scratch

```js
const source$ = new Observable(observer => {
  	console.log('Creating observable');
	observer.next('Hello World'); //emiting an event
  	observer.next('Hello World2');
    	observer.error(new Error('Something wrong...')); // calls the f2 of the subscriber. But will not hit f3
	setTimeout(()=> {
      	    observer.next('Hello World3');
            observer.complete(); // this will call f3 of the subscriber.
      	}, 3000); //emit next value after 3 secs. 
  });
  source$.subscribe(f1, f2, f3);
  //to hit f3 even when there is an error.
  source$.pipe(catchError(val => of(`I caught: ${val}`)))
      .subscribe(f1, f2, f3);
```
  
## Observables from a Promise

```js
const myPromise = new Promise((resolve, reject) =>{
   //simulate async operation
   setTimeout(() => {
   	resolve('Hello from promise');
   },3000);
});
//create an observable.
const source$ = fromPromise(myPromise);
source$.subscribe(x => console.log(x));
//same as myPromise.then(x => console.log(x));
```
## RxJs operators

See full list of operators: https://rxjs-dev.firebaseapp.com/guide/operators

These methods are valueble when testing. 

### interval, timer and range
```js
//emits numbers 0,1,2,3,... every 1 sec and continues.
const source$ = interval(1000);
//to emit only 5 numbers
const source$ = interval(1000).pipe(take(5));

//Timer
//Similer to interval. Start emitting after 5secs from page load and emits
//emits numbers 0,1,2,3, .... in 1sec interval.
const source$ = timer(5000,1000).pipe(take(5));

//Range
//Emits 10 numbers starting from 5. Emited all at once. 
const source$ = range(5,10);
```
### map
```js
//Similar to Array.prototype.map(). It doesn't create a new Observable. The subscriber gets output 0,2,4,6,8
const source$ = interval(1000)
      .pipe(take(5))
      .pipe(map(v => v *2));
```

### pluck

```js
const users = [ {name:'Will', age:34},{name:'Tom', age:33},{name:'Jack', age:35}]
const names$ = from(users).pipe(pluck('name')); //gets Will, Tom, Jack
```

### merge
```js
//can merge two or more Observables
//import { merge } from 'rxjs/operators';
const mergedSource$ = of('Hello')
			.pipe(merge(of('World!'))); // Outputs 'Hello' and 'World' seperately
			
interval(500).pipe(take(10))  //emits 10 integers every 1/2 sec
        .pipe(merge(interval(1000).pipe(take(25)))) // emits 25 integers every sec
		.subscribe(x => console.log(x)) // Both emit values together
//you can also do
//import { merge } from 'rxjs';
const merged$ = merge(source1$, source2$, ... ) //All source observables emit at the same time.
```
### concat

```js
//can concatnate observables one right after another.
const concatnated$ = concat(source1, source2); // source2 starts emiting once source1 is finished.
```

## Rxjs Flattening Operators

Eg:  ```switchMap```, ```concatMap```, ```mergeMap```, and ```exhaustMap```. 
- Like all pipeable operators, they begin with a source observable that is emitting values.
- Each emitted value from the source observable is mapped to another observable. (note that ```map``` doesn't create a new observable)
- The inner observable is then **"lifted"** up into the primary pipeline, so what comes out of the other side of a flattening operator is always the **inner observable**.
- **If there is only one value** emitted from the source or **there is a long time between emissions** to do inner calculations before there is a new value from the source observable, **there is no difference in how the flattening operators behave**
- The difference come when there are **multiple overlapping emissions from the source**.

### An analogy
Source observable is your boss, and you are the flattening operator.    
When the boss gives some work (source emits a new value), you begin to work on a new task (creating a new inner observable).     
If you are still in the middle of one task and your boss gives you something else to do, how do you handle the situation?    

### mergeMap

```js
//The overachieving multitasker. You immediately begin working on everything your boss gives you as soon as he/she assigns it.
//Without mergemap you have to use double subscribes. eg:

//returns an observable which emits strings 'A{param}', 'B{param}' ...
const getDataObsrv = (number) => {
    const letters$ = from(['A','B','C','D']);
    return letters$.pipe(map(letter => `${letter}${number}`))
}
// an observable which emits numbers 1,2,3,4
const numbersObsrv = from([1,2,3,4]);

// using a regular map but has to use subscribe() twice
numbersObsrv.pipe(
  map(number => getDataObsrv(number)),
).subscribe(dataObsrv =>  dataObsrv.subscribe(val => console.log(val)));

// using map and mergeAll
numbersObsrv.pipe(
  map(number => getDataObsrv(number)),
  mergeAll()
).subscribe(val =>  console.log(val));

// Better solution: using mergeMap
numbersObsrv.pipe(
  mergeMap(number => getDataObsrv(number))
).subscribe(val => console.log(val));
```

### switchMap
```js
//Drop everything you were already doing and immediately begin the new task. This means only the latest and greatest values are provided.
```
### concatMap
```js
//You add your boss’s request to the end of your to-do list, but you completely finish whatever you were currently working on, and then you begin work on the next task. 
//You eventually finish everything, and you do so in order.
```
### exhaustMap
```js
//You completely ignore new requests from your boss until you are done with what you are working on, and only then do you begin listening for new tasks.
```
