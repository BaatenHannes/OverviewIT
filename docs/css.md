# CSS

## Positioning elements

### Float

```css
float:left;
```

* The element needs to have specified width
* The element is removed from normal flow, but still part of the flow (unlike absolute positioning)
* It is shifted to the left, or right, until it touches the edge of its containing box, or another floated element.
* Other elements wrap around. To make sure elements go below the floated element, use `clear: both`
* Parent with only floated elements will collapse, fix by 
    * creating empty div behind parent element with `clear: both`
    * or give parent element `overflow: auto`

### Flex-box

#### Parent

* **`display: flex;`** --- defines the flex container
* **`flex-direction: row;`** --- defines the direction flex for child items
* **`justify-content: center;`** --- aligns child items on main axis
* **`align-items: flex-start;`** --- aligns child items on cross axis
* **`align-content: flex-start;`** --- aligns rows of items on cross axis (only when there are multipe rows of items)

#### Child

* **`align-self: auto | center ...;`** --- allows default alignment to be overridden for this individual element
* **`order: <integer>;`** --- assigns the order of the item
* **`flex-grow: <integer>;`** --- defines the ability to take in more space, will overwrite the defined width (or height). The default is 0 (no grow, keep defined width). Value only has meaning relative to other flex-grow values, defines the proportion of space to take in relation to other flex objects.
* **`flex-shrink: <integer>;`** --- defines the ability to shrink if the container is too small. Default is 1, which means that every object will shrink equally. 0 will keep the object at its defined width. Higher values will make some object shrink faster than others.

[Nice link with gifs on flex-grow and flex-shrink](https://medium.freecodecamp.org/even-more-about-how-flexbox-works-explained-in-big-colorful-animated-gifs-a5a74812b053)

### Centering

#### Horizontal

* Block element: `margin: 0 auto` or flexbox
* Text: `text-align: center` 

#### Vertical

* Block element: flexbox
* Text: line-height

#### Perfect centering with flexbox

* Parent element: `display: flex` and define width and height
* Child element: `margin: auto` and define width and height
