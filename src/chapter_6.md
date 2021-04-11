# Widget Styling

As with layout, this quick start guide does not aim to cover all of the intricacies of styling and the properties available. A more comprehensive guide can be found in the [styling section]().

## Inline and Shared Styling

So far we have defined our style properties directly on the widgets using the builder, also known as *inline* styling. Tuix also offers the ability to define style rules to allow *shared* styling between multiple widgets. The widgets affected by these shared style rules are determined by *selectors* which should be familiar to web developers using css and work in the same way.

The following code defines a style rule which acts on any widgets with a class name of `"my_class"`, and also gives this class name to our two widgets:

```rs
use tuix::*;



fn main() {
    let app = Application::new(|state, window| {
        
        window.set_title("Custom Title").set_inner_size(300,300);

        // Create a shared style wich applies to all widgets with class name "my_class"
        let style_rule: StyleRule = StyleRule::new()
            .selector(Selector::new().class("my_class"))
            .set_height(Units::Pixels(30.0))
            .set_background_color(Color::rgb(80,200,20));

        // Add the shared style rule to state
        state.add_style_rule(style_rule);

        let container = Element::new().build(state, window.entity(), |builder| 
            builder
                .set_width(Units::Pixels(100.0))
                .set_space_left(Units::Stretch(1.0))
                .set_space_right(Units::Stretch(1.0))
                .set_space_top(Units::Stretch(1.0))
                .set_space_bottom(Units::Stretch(1.0))
                .set_background_color(Color::rgb(20,80,200))

                // Add a class name "my_class"
                .class("my_class")


        );

        Button::new().build(state, container, |builder| 
            builder
                .set_width(Units::Pixels(30.0))

                // Add a class name "my_class"
                .class("my_class")
        );

    });

    app.run();
}
```

Note that the style rule has to be added to the app using `state.add_style_rule()`. Note also that *inline* properties override *shared* properties, so although both widgets are affected by the shared style, the button keeps its blue color as it comes from an inline style rule. The height property, on the other hand, is shared between the two widgets. Below is the output of this code:

![widget_styling_01](../images/widget_styling_01.png)
