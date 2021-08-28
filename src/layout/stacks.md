# Size

## Width and Height
The size of a widget is determined by its `width` and `height` properties, which both have the type of `Units` enum, which has four variants:

1. Auto:
    - If the layout type is a column, then auto width will be the maximm child width and auto height will be the sum of the heights of its children.
    - If the layout type is a row, then auto width will be the sum of the widths of its children and auto height will be the maximum child height.
2. Stretch: 
    - The width/height will stretch to fill some proportion of the remaining available space. The remaining available space is the space left after subtracting the width/height of non-stretch children.
3. Percentage:
    - The width/height is a specified proportion of the parent width/height, unless overriden by min_/max_ width/height.
4. Pixels:
    - The width/height is a specified number of pixels, unless overriden by min_/max_ width/height.

## Size Constraints
The width and height of a widget can be constrained by speifying a minimum and maxium using `min_width`, `max_width`, `min_height`, and `max_height`. These properties override the width and height properties and can be specified in `Units`:

1. Auto:
    - If the layout type is a column then the min_width 
2. Stretch:

3. Percentage:

4. Pixels:

# Position

Child widgets added to a parent are arranged into either a vertical column, a horizontal row, or a grid and is determined by he `layout_type` property:

1. Col:
    - Child widgets are arranged into a verical column.
2. Row:
    - Child widgets are arranged into a horizontal row.
3. Grid:
    - Child widgets are positioned by a row and column indices and their size is determined by row and column spans.

## Position Type
The `position_type` property specifies whether a widget should be affected by the position of the other child widgets.

1. Parent-directed:
    - The widget is positioned by the parent relative to its usual position within a column, row, or grid.
2. Self-directed:
    - The widget is positioned relative to the top-left corner of the parent and is not affected by sibling widgets.

## Space

The position of a widget can be modified by adding space to the `left`, `right`, `top`, and `bottom`, and is also specified in `Units`. All four properties can be set simultaneously with the `space` property.

1. Auto:
    - The space is determined by the parents `child_space` properties. For example, an auto `left` is overriden by the parents `child_left` property.
2. Stretch:
    - The space is a specified proportion of the available remaining space.
3. Percentage:
    - The space is a specified proportion of the parent width/height, unless overridden by min/max constraints. 
4. Pixels:
    - The space is a specified number of pixels, unless overridden by min/max constraints.

## Space Constraints
The space properties can also be constrained with minimums and maximums, also using `Units`:

1. Auto: 
2. Stretch:
3. Percentage:
4. Pixels:

## Child Space
While `space` is used to set the spacing of individual widgets, `child_space` is used to set the spacing of all child widgets in one go, as long that the child space properties are set to auto. Child space can be considered similar to padding and is also specified in `Units`:



