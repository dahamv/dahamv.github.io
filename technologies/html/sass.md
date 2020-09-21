**[Home](../../index.md)**  

## SASS is a CSS preprocessor

There are others like Less.   
There are 2 extentions .sass and .scss(recomended since its backword compatible with CSS)   

- Had variables: eg - $primary-color:blue;
- supports nesting.
- supports Modules: Can brakeup CSS into seperate files and import. Regular CSS can also have seperate CSS files but the browser does multiple HTTP calls to get those files.   
  SASS preprocessor can collect all files and make one big CSS file. **@use 'base'** (to use base.scss)   
  make the file as **_base.scss** so that SCSS compiler doesn't compile it. and you can use it with @use.
- supports **Functions** and **Mixins**. Functions return something. Mixins only do placing the CSS.
- supports Inheritance. 

### Mixins and Functions

```css
@mixin myMixin {
  color: red;
  font-size: 25px;
}
.mycssclass {
  @include myMixin;
  background-color: green;
}
```

### Inheritence

```css
/* This CSS will print because %message-shared is extended. */
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.message {
  @extend %message-shared;
}
.success {
  @extend %message-shared;
  border-color: green;
}
```
