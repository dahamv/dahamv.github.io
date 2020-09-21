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
Note: see float clearing hack (https://www.w3schools.com/howto/howto_css_clearfix.asp)  

```HTML
<!-- by default they are placed on top of eachother. -->
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

<!-- Elements after a floating element will flow around it and will be a mess. Use the "clearfix" hack to fix the problem: -->
<div class="clr"></div>
<!--Now can do other floting and stuff-->
```
```css
.clr{
   clear:both; /*this will clear any floats above it*/
}
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

### Creating a sidebar with floating

```HTML
<div id="main-block">
   <h3>Heading</h3>
   <p>some paragraph ....</p>
</div>
<div id="sidebar">
   <p>some other paragraph ....</p>
</div>
<div class="clr"></div>
```
```css 
#main-block{
   float:left;
   width:70%;  
   padding:15px;
   box-sizing:border-box; /*To stop sidebar comming down because of the padding*/
}
#sidebar{
   float:right;
   width:30%;
   background-color:#333;/*dark gray*/
   color:#fff; /*text color is white*/
   padding:15px;
   box-sizing:border-box; /*To stop sidebar comming down because of the padding*/
}
```

## Positioning in CSS

Position property values:   
- Static(default) - renders the document inorder of the document flow.
- Reletive - element is positioned relative to its normal position. Can add properties like top, right, left, bottom.
- Absolute - Will allow us to target whatever possition we want inside of a relative element.
- Fixed - Fixed possition to the browser window. No matter how much we scroll it will always be in the same position.
- Initial - sets the property to its default value.
- Inherit - will inherit the property of its parent element.

```HTML
<div class="p-box">
   <h1>Heading</h1>
   <h2>Good Bye</h2>
   <p>some paragraph ....</p>
</div>

<a class="fix-me button" href="#">Mylink</a>
```
```css
.p-box{
   width:800px;
   hight:500px;
   border:1px solid #000; /*black*/
   margin-top:30px;
   /*if position:reletive is not defined for this div, its child h1 will be positioned 40px from the very top of the 
   page(html document) since it doesn't have a relative parent element. Will mess up the layout of the webpage.*/
   position:reletive;
   background-image:url('./path/to/img');
   /*pushed from the border. The first value is the horizontal position and the second value is the vertical.*/
   background-position:100px 200px; 
   background-possition:center center; /*Can also do this or "center top" etc*/
   backgroup-repeat:no-repeat; /*since the image is pushed it will repeat inside the box. to stop that*/
}
.p-box h1{   
   /*will be possitioned 40px from the border of the parent div. since the parent is positioned reletive*/
   position:absolute:
   top:40px; 
   left:200px; /*push 200px from the left.*/
}
.p-box h2{
   position:absolute:
   bottom:40px; 
   right:200px;   
}
.fix-me{
   position:fixed;
   top:400px; /*will be fixed 400px from the top of the browser no matter how much we scroll*/
}
```

## Psudo Classes

```HTML
<ul class="my-list">
   <li>List 1</li>
   <li>List 2</li>
   <li>List 3</li>
   ...
   <li>List 10</li>
</ul>
```

```css
.my-list li:first-child{
   background:red; /*changes the bg color of List 1. No need to say background-color. since its red CSS knows.*/   
}
.my-list li:last-child{
   background:blue;
}
.my-list li:nth-child(5){ /*5th child*/
   background:yellow;
}
.my-list li:nth-child(even){ /*all the even children*/
   background:grey;
}
```

## Sample webpage

Using html 5 symantic elements (element name is self discriptive) like <header> <nav> <aside> <section> etc.   

<img src="/assets/images/sampleWebpageCSS.png" alt="drawing" width="600"/> 

```HTML
<!DOCTYPE html>
<html>
<head>
	<title>My Website</title>
	<link rel="stylesheet" type="text/css" href="css/style.css">
</head>
<body>
	<header id="main-header">
		<!--container should be inside header since we can set the bg-color to go across the entire page.-->
		<div class="container">
			<h1>My Website</h1>
		</div>
	</header>

	<nav id="navbar">
		<div class="container">
			<ul>
				<li><a href="#">Home</a></li>
				<li><a href="#">About</a></li>
				<li><a href="#">Services</a></li>
				<li><a href="#">Contact</a></li>
			</ul>
		</div>
	</nav>

	<section id="showcase">
		<div class="container">
			<h1>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
			</h1>
		</div>
	</section>

	<div class="container">
		<section id="main">
			<h1>Welcome</h1>
			<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed.....</p>
		</section>
		<aside id="sidebar">
			<p>Lorem ipsum dolor sit amet, consectetur.......</p>
		</aside>
	</div>

	<footer id="main-footer">
		<p>Copyright &copy; 2017 My Website</p>
	</footer>
</body>
</html>
```

```css
body{
	background-color:#f4f4f4; /*light grey*/
	color:#555; /*dark gray*/
	font-family:Arial, Helvetica, sans-serif;
	font-size:16px;
	line-height:1.6em;
	margin:0;
}
.container{
	width:80%;
	margin:auto;
	overflow:hidden; /*There will be no scrollbars even if something goes outside of its container*/
}
#main-header{
	background-color:coral; /*light orenge*/
	color:#fff; /*white*/
}
#navbar{
	background-color:#333; /*dark gray*/
	color:#fff; /*white*/
}
#navbar ul{
	padding:0;
	list-style: none; /*get rid of the bullet points*/
}
#navbar li{
	display:inline; /*change the display from block(default) to inline. So that all links will be in one line.*/
}
#navbar a{
	color:#fff; /*white*/
	text-decoration: none; /*To remove the underlines.*/
	font-size:18px;
	padding-right:15px; /*To add space between nav items.*/
}
#showcase{
	background-image:url('../images/showcase.jpg');
	background-position:center right; /*sets the bg-image possition to the center-horizontally, right-vertically*/
	/*with min-hight the size of showcase will responsively grow when the browser windows is made small. Otherwise the text will over flow*/
	min-height:300px; 
	margin-bottom:30px; /*push whats under the showcase down a bit*/
	text-align: center; /*text will be in the center even when the window is made small.*/
}
#showcase h1{
	color:#fff; /*white*/
	font-size:50px;
	line-height: 1.6em;
	padding-top:30px;
}
#main{
	float:left;
	width:70%;
	padding:0 30px;
	box-sizing: border-box;
}
#sidebar{
	float:right;
	width:30%;
	background: #333; /*dark gray*/
	color:#fff; /*white*/
	padding:10px;
	box-sizing: border-box;
}
#main-footer{
	background: #333;
	color:#fff;
	text-align: center;
	padding:20px;
	margin-top:40px;
}
/*This is a CSS3 rule to include a block of CSS properties only if a certain condition is true.*/
/*only effective when the width of the document width is 600px*/
@media(max-width:600px){
	/*main and sidebar go on top of eachother*/
	#main{
		width:100%;
		float:none;
	}

	#sidebar{
		width:100%;
		float:none;
	}
}
```
