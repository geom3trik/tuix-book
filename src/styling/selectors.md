# Selectors
Selectors determine which shared style rules affect which widgets and work in much the same way as css selectors. The main difference being that tuix does not support all forms of selector.

## Element Selector
Each type of widget can be given a unique name which can then be used to apply styles to all widgets of that type. For example, to set the background color of all `button` elements:

```css
button {
    background-color: #555555;
}
```
The element name is typically set in the `on_build` method of a widget using the `.set_element(state: &mut State, name: &str)` method on the entity id. Unlike most other properties, a setter for the element name is not provided by the builder as the name is designed to be set for all widgets of the same type.

## Class Selector
Widgets can have multiple class names which can be selected using a dot followed by the name. For example, to set the background color of all widgets with a class name of `"item"`:

```css
.item {
    background-color: #445566;
}
```

# Pseudo-Selectors
Shared styles can also contain pseudoselectors which select widgets based on a particular state that the widget is in. In tuix there are 8 pseudoselectors:

1. 