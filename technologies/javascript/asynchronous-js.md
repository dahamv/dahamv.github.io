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
createPost = (post, callback) => setTimeout(()=>{
                                        posts.push(post);
                                        callback();
                                        },2000);
const post3 = {title:'post 3', body: 'post body 3'};
createPost(post3,getPosts()); //console logs all 3 posts after 3 secs (1+2)
```

