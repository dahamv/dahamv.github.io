**[Home](../../index.md)**  

## SASS is a CSS preprocessor

There are others like Less.   
There are 2 extentions .sass and .scss(recomended since its backword compatible with CSS)   
You CAN'T include .scss files in <link rel="stylesheet" href="css/mystyle.scss">. It has to be a CSS file and SCSS files will be compiled to CSS.

- Had variables: eg - $primary-color:blue;
- supports nesting.
- supports Modules: Can brakeup CSS into seperate files and import. Regular CSS can also have seperate CSS files but the browser does multiple HTTP calls to get those files.   
  SASS preprocessor can collect all files and make one big CSS file. **@use 'base'**. 
  make the file as **_base.scss** so that SCSS compiler doesn't compile it and you can use it with **@use**. These underscore files are called **partials**.   
  can use **@import** to import an partial file.
- supports **Functions** and **Mixins**. Functions return something. Mixins only do placing the CSS.
- supports Inheritance. 
- can use Operators for calculations. + - / *
- can use Conditionals in mixins and functions.. if else with @if and @else

### Mixins and Functions

```css
@mixin myMixin {
  color: red;
  font-size: 25px;
}
.mycssclass {
  @include myMixin; /*adds the properties in the mixin*/
  background-color: green;
}

/*Set text color based on background. Otherwise the text will be unreadable*/
@function set-text-color($color) {
  @if(lightness($color) > 70) { /*if its a light color*/
    @return #333; /*dard gray*/
  } @else {
    @return #fff; /*white*/
  }
}

/*Set background & text color*/
@mixin set-background($color) {
  background-color: $color;
  color: set-text-color($color) /*since text should be readable*/
}
```

### Using loops to programatically create css classes
```scss
// Margin & padding classes
$spaceamounts: (1,2,3,4,5);

@each $space in $spaceamounts {
  .m-#{$space} {  // creates m-1, m-2 ... m-5
    margin: #{$space}rem; // in rem units
  }
  .my-#{$space} {  // creates my-1, my-2 ... my-5
    margin: #{$space}rem 0; //top and bottom -> $space, left and right -> 0
  }

  .p-#{$space} {
    padding: #{$space}rem;
  }
  .py-#{$space} {
    padding: #{$space}rem 0;
  }
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
