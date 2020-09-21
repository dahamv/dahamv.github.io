**[Home](../../index.md)**  

## Flex Box
<img src="/assets/images/flexbox.png" alt="drawing" width="600"/>  

A CSS3 layout to arrange items within a container easily.
- No floats
- Responsive and Mobile friendly
- Positioning child elements is easy 
- Container margins do not collapse within margins of its contents.
- Order of elements can be changed without changing HTML

https://jsfiddle.net/bradtraversy/bu0ecodm/1/

### Flex container properties

- flex-direction - Row or column
- flex-wrap - Get the contents in the container to the next line if browser window is shrinked and there's not enough space.
- flex-flow - This is a shorthand property for setting both the **flex-direction** and **flex-wrap** propertie (eg: column nowrap)
- justify-content - get the items in the container to the left end(flex-start), center, right end(flex-end) or distribute evenly.
- align-items - similer to justify-content, but considers vertically also. eg: center
- align-content - The baseline (horizontal line through the middle) aligns the flex items (of different sizes) such as their baselines aligns:

### Flex item properties

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self
