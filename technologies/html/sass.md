**[Home](../../index.md)**  

## SASS is a CSS preprocessor

There are others like Less.   
There are 2 extentions .sass and .scss(recomended since its backword compatible with CSS)   

- Had variables: eg - $primary-color:blue;
- supports nesting.
- supports Modules: Can brakeup CSS into seperate files and import. Regular CSS can also have seperate CSS files but the browser does multiple HTTP calls to get those files.
 >>  SASS preprocessor can collect all files and make one big CSS file. @use 'base' (to use base.scss)
 
