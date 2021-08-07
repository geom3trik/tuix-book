# Parent Properies (TODO)


## Layout Type

**Rust**
```rs
enum LayoutType {
    Row,
    Column,
    Grid,
}
```
**Stylesheet:**
```css
.container {
    layout-type: row | column | grid;
}
```
This determines how the child elements should be arranged in the parent.

- **Row** - Child elements are arranged into a horizontal stack from left to right
- **Column** - Child elements are arranged into a vertical stack from top to bottom
- **Grid** - Child elements are arranged into a grid (link here)

## Child-Space
This determines the spacing around the elements in a stack (does not apply to grid). 

Each of the child spacing properties (see below), defined on the parent, acts to override the auto-spaced properties of the child elements. For example, the parent `child-left` property will override the `left` property of a child element if the `left` property is set to `Units::Auto`.

There are five child-space properties:
1. **child-left** - determines the space to the left of the stack. Applies to the *first* element in a horizontal stack and *all* elements in a vertical stack.
2. **child-right** - determines the space to the right of the stack. Applies to the *last* element in a horizontal stack and *all* elements in a vertical stack.
3. **child-top** - determines the space to the top of the stack. Applies to the *first* element in a vertical stack and *all* elements in a horizontal stack.
4. **child-bottom** - determines the space to the top of the stack. Applies to the *last* element in a vertical stack and *all* elements in a horizontal stack.
5. **child-between** - determines the space between elements on the main axis. Applies to all elements except the first and last and acts to override child `left` and `right` spacing.

With these child spacing properties it is possible to do a number of alignment configurations:

#### Align Left 
```css
.container {
    child-left: 0px;
    child-right: 1s;
}
```
#### Align Center
```css
.container {
    child-left: 1s;
    child-right: 1s;
}
```
#### Align Right
```css
.container {
    child-left: 1s;
    child-right: 0px;
}
```
#### Align Top
```css
.container {
    child-top: 0px;
    child-bottom: 1s;
}
```
#### Align Middle
```css
.container {
    child-top: 1s;
    child-bottom: 1s;
}
```
#### Align Bottom
```css
.container {
    child-top: 1s;
    child-bottom: 0px;
}
```
#### Space Between
```css
.container {
    child-between: 1s;
}
```
#### Space Evenly (Row)
```css
.container {
    child-left: 1s;
    child-right: 1s;
    child-between: 1s;
}
```