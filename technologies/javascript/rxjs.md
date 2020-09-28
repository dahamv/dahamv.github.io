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
  const array$ = rx.Observable.from(array)
  //f1- run for all events
  //f2- run if error
  //f3- run when completed.
  array$.subscribe(f1,f2,f3); 
  ```
- Observables from scratch
	```js
  const source$ = new rx.Observable(observer => {
  	console.log('Creating observable');
	observer.next('Hello World'); //emiting an event
	observer.next('Hello World2');
	setTimeout(()=> observer.next('Hello World3'), 3000); //emit next value after 3 secs.
	observer.complete(); // this will call f3 of the subscriber.
  });
  source$.subscribe(x => console.log(x), f2, f3);
  //To throw an error
  observer.error(new Error('Something wrong...')); // calls the f2 of the subscriber. But will not hit f3
  //to hit f3 even when there is an error.
  source$.catch(err => rx.Observable.of(err)).subscribe(f1,f2,f3); // of takes anything and fires an observalbe.
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
const source$ = Rx.Observable.fromPromise(myPromise);
source$.subscribe(x => console.log(x));
//same as myPromise.then(x => console.log(x));
```
## Interval, Timer and Range (RxJs operators)

See full list of operators: https://rxjs-dev.firebaseapp.com/guide/operators

These methods are valueble when testing. 

```js
//Interval emits numbers 0,1,2,3,... every 1 sec and continues.
const source$ = Rx.Observable.interval(1000);
//to emit only 5 numbers
const source$ = Rx.Observable.interval(1000).take(5);

//Timer is similer. Start emitting after 5secs from page load and emits
//emits numbers 0,1,2,3, .... in 1sec interval.
const source$ = Rx.Observable.timer(5000,1000).take(5);

//Range emits 10 numbers starting from 5. Emited all at once. 
const source$ = Rx.Observable.range(5,10);
```

## Map and Pluck

```js
const source$ = Rx.Observable.interval(1000)
		.take(5)
		.map(v => v *2); //Similar to Array.prototype.map()
```
Error handling  

https://youtu.be/ei7FsoXKPl0?t=2177
