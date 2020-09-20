**[Home](../../index.md)**  

## CSS selectors
<img src="/assets/images/css-selectors.png" alt="drawing" width="600"/>  

"< a >" is a link.

```css
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
  margin:0; /* there is a margin around the body. make it 0. Otherwise you will see that when the background color changes.*/
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
Wrapping the body contents in a div  
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
   /*5px wide red solid (border width color and style*/
   border: 5px red solid;
}

/*To push the contents to the middle a bit. Otherwise the contents will start from the edge*/
.container{
   width:960px; /*width of the container. 960px is a popular width for websites*/
   /*The best practice is to set it to a percentage so that when the window is made small the container width will be responsive*/
   width:80%;
   margin:auto; /*sets a margin to both sides. And its auto. So the container is pushed to the very middle*/
}
```
