**[Home](../../index.md)**  

## Flex Box
<img src="/assets/images/flexbox.png" alt="drawing" width="600"/>  

A CSS3 layout to arrange items within a container easily.
- No floats
- Responsive and Mobile friendly
- Positioning child elements is easy 
- Container margins do not collapse within margins of its contents.
- Order of elements can be changed without changing HTML


### Flex container properties

- flex-direction - Row or column
- flex-wrap - Get the contents in the container to the next line if browser window is shrinked and there's not enough space.
- flex-flow - This is a shorthand property for setting both the **flex-direction** and **flex-wrap** propertie (eg: column nowrap)
- justify-content - get the items in the container to the left end(flex-start), center, right end(flex-end) or distribute evenly. (Horizontally)
- align-items - similer to justify-content, but considers vertically also. eg: center
- align-content - The baseline (horizontal line through the middle) aligns the flex items (of different sizes) such as their baselines aligns:

### Flex item properties

- order - specifies the order of the flex items.
- flex-grow - how much a flex item will grow relative to the rest of the flex items.
- flex-shrink - how much a flex item will shrink relative to the rest of the flex items.
- flex-basis - specifies the initial length of a flex item.
- flex - a shorthand property for the flex-grow, flex-shrink, and flex-basis properties.
- align-self - specifies the alignment for the selected item inside the flexible container.

https://www.w3schools.com/css/tryit.asp?filename=trycss3_flexbox_image_gallery
https://www.w3schools.com/css/tryit.asp?filename=trycss3_flexbox_website2

## CSS Grids 

Flex-box - one dimentional layout      
CSS Grids - two dimentional layout     
      
There are Grid container properties and Grid item properties.      
align properties - vertically   
justify properties - horizontally    

<img src="/assets/images/cssGridLines.png" alt="drawing" width="600"/>  

```css
/*To make Items span across grids*/
/*As an item property*/
.item1 {
  grid-column: 1 / 5; /*span from column line 1 to 5*/
  grid-column: 1 / span 3; /*start from column line 1 and span 3 column lines*/
}
```
https://www.w3schools.com/css/css_grid.asp

