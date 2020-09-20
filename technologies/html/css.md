**[Home](../../index.md)**  

## CSS selectors
<img src="/assets/images/css-selectors.png" alt="drawing" width="600"/>  

"< a >" is a link.

```css
/*for every element*/
*{
   /*Reset the default margin/padding values */
   /* there is a margin around the body. make it 0. Otherwise you will see that when the background color changes.*/
   margin:0; 
   /*h1-6 by default have padding so remove that*/
   padding:0; 
}

body {
   /*colors can be with #fontnumber, rgb(v1,v2,v3), color name - red,green,yellow and html5 color names like coral */
  background-color : #f4f4f4  /* light gray; */
  /*dark gray - for the charactors in the <body/>. applies to all tags in <body> eg; <h1-6>, <p> etc */
  color : #555555; 
  
  /*a family is needed so that if the browser can't load the first, it will go to the next.  */
  /*Can make a gereric font type at the end like : serif */
  font-family : Arial, Helvetica, sans-serif;
  /*of all the fonts in child tags like <p> except <h1-6> */
  font-size : 16px;
  font-weight : bold; /* can do numbers like 400; normal is normal  */
  
  /* Same as above */
  font : bold 16px Arial, Helvetica, sans-serif;
  
  /* em is a unit in css for responsive pages. */
  line-hight : 1.5em; /*hight between two lines in a paragraph. */
  
}
```
## CSS Box Model
<img src="/assets/images/BoxModel.png" alt="drawing" width="400"/>   
Content can be any eliment div, a, p, h etc.  
Padding, Border, Margin are properties.
- Padding - space between the element and its boarder/edge. **Space Inside**
- Border - Not used much but the border-width can be set for top, right, bottom, left.
- Margin - the space outside the border. **Space Outside** 

```css
p{
   margin-top:5px;
   margin-right:10px;
   margin-bottom:5px;
   margin-left:10px;
   /*short way top, rigtht, bottom, left (remember:clockwise)*/
   margin:5px 10px 5px 10px;
   /*if top - bottom and right - left are the same (remember vertical, horizontal*/
   margin:5px 10px;
   /*if all margins are equal*/
   margin:5px;
   /*Same way with PADDING*/
}
```
Wrapping the body contents in a div.  
NOTE: Block level element like divs, h1-6s, inputs always start with a newline.   
      lables are inline. You can change it to become a block level element (display:block)    

```HTML
<body>
   <div class="container">
      <div class="box-1"> 
         <h1>heading</h1>
         <p> some paragraph </p>
      </div>
   </div>
</body>
```
```css
/*to target a class use a dot . */
.box-1{
   background-color : #333 /*shortcut for #333333 dark gray*/
   color : #fff /*sortcut for #ffffff i.e. white*/
   /*5px wide red solid (border width color and style(solid/dotted/dashed)*/
   border: 5px red solid;
   
   padding: 20px; /*add some space between the border and content.*
   margin-top: 20px 0; /*top and bottom 20px. left and right 0*/
}

/*To push the contents to the middle a bit. Otherwise the contents will start from the edge*/
.container{
   width:960px; /*width of the container. 960px is a popular width for websites*/
   /*The best practice is to set it to a percentage so that when the window is made small the container width will be responsive*/
   width:80%;
   margin:auto; /*sets a margin to both sides. And its auto. So the container is pushed to the very middle*/
}

/*Targeting h1 inside box-1*/
.box-1 h1 {
   font-family:Tahoma;
   font-weight:800;
   font-style:italic;
   text-decoration:underline;
   text-transform:uppercase;
   letter-spacing:0.2em; /*space between letters. use em units for this*/
   word-spacing:1em;
}
```

## Styling unorderd lists;

```HTML
<div class="catogories">
   <h2>Catogories</h2>
   <ul>
      <li><a href="#">category 1</a></li>
      ...
      <li><a href="#">category 4</a></li>
   </ul>
</div>
```
```css
.catogories{
   border: 1px #ccc solid; /*color gray*/
   padding: 10px;
   border-radious:15px; /*now border has curved edges*/
}
.catogories h2{
   text-align:center;
}
.catogoris ul{
   /*uls have about 40 px default padding. so make it 0*/
   padding:0;
   padding-left;20px;
   /*bullets style - default is disc(the small circle). Put "none" to getrid of bullets. Can also do this in li*/
   list-style:square;
}
.catogories li{
   padding-bottom:6px;
   border-bottom:dotted 1px #333;
   list-style-image: url('../images/check.png');
}
```

## adding states to elements

```css
/*globally for all links */
a{
   text-decoration:none; /*to get rid of default underlines of links*/
   color:#000 /*defulat links color is bule. change it to black*/
}
/*links can have states. To change hover state styles*/
a:hover{
   color:red; /*when hover over links they turn red.*/
}
a:active{
   color:green; /*When a link is clicked it changes its color to gree for a sec.*/
}
a:visited{
   color:yellow; /*when the link is visited its color changes to yellow*/
}
```

## Styling forms

```HTML
<form class="my-form">
   <div class="form-group">
      <label>Name: </label>
      <input type="text" name="name">
   </div>
   <div class="form-group">
      <label>Email: </label>
      <input type="text" name="email">
   </div>
   <div class="form-group">
      <label>Message: </label>
      <textarea name="message"></textarea>
   </div>
  <input class="button" type="submit" value="Submit">
</form> 
```

```css
.my-form{
   padding:20px; 
}
/*form-group inside my-form*/
.my-form .form-group{
   padding-bottom:15px;
}
.my-form lable{
   /*change all lables in my-form into block level elements*/
   display:block; 
}
/*since the submit button is also an input, when [type="text"] is added it will disregard the button*/
.my-form input[type="text"], .myform textarea{
   padding:8px;
   width:100%; /*of its container*/
}
/*You can add this button class to a link as well. <a class="button" href="#"></a>*/
.button{
   background-color:#333; /*dark gray*/
   color:#fff /*white*/
   padding:10px 15px;
   border:none;
}
/*add a hover state to button class*/
.button:hover{
   background:red;
   color:#fff;
}
```

## Alignment and floating

```HTML
<!-- by default they are placed on top of eachother.
<div class="block">
   <h3>Heading</h3>
   <p>some paragraph ....</p>
</div>
<div class="block">
   <h3>Heading</h3>
   <p>some paragraph ....</p>
</div>
<div class="block">
   <h3>Heading</h3>
   <p>some paragraph ....</p>
</div>
```
```css
.block{
   /*now these blocks are placed horizontally*/
   float:left; 
   width:33.3% /*since there are 3 of them*/
   border:1px solid #ccc;
   padding:10px;
   /*because of the padding and the border eventhough the width is 33.3% the last div will go to the next line. To avoid that*/
   box-sizing:borderbox; /*it will take the border and padding and will apply to this 33.3% */
}
```

## Flex Box
