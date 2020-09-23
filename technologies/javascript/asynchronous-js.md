**[Home](../../index.md)**  

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
## Promises

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
```
