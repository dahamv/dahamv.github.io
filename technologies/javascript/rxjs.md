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
  const source$
  ```
Filtering / Transforming Observables
Promises to Observables
Many helpful operators
Error handling
