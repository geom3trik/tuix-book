# Display Properties

There are three properties which affect whether a widget is displayed or not:

 1. display
 2. visibility
 3. opacity

## Display
The `display` property determines whether or not a widget is included within both layout and rendering. The `display` property can be set to either `none` (`Display::None`), or `flex` (`Display::Flex`) which is the default. If the `display` property is set to `none` then the widget will not be included in layout and will not be rendered.

## Visibility
The `visibility` property affects only whether a widget will be included during rendering. Unlike `display`, the `visibility` of a widget does *not* affect layout. The `visibility` property can be set to either `visible` (`Visibility::Visible`), the default, or `