# Introduction

While almost any desired gui behaviour can be acheived by sending events around, this can become hard to acheive when the application becomes more complex. For example, sending an event between two widgets that are far away from each other in the visual tree requires knowing the entity id of the target widget, which isn't always practical.

To solve these problems tuix has a built-in method for reactivity called binding. This section will cover bindings in detail but to begin with let's look at a basic example to see what exactly bindings achieve:

```rs
use tuix::*;

#[derive(Lens)]
pub struct AppData {
    value: i32,
}

impl Model for AppData {}

fn main() {
    let app = Application::new(WindowDescription::new(), |state, window|{
        let app_data = AppData{value: 30}.build(state, window);

        Label::new("")
            .bind(AppData::value, |value| value.to_string())
            .build(state, app_data, |builder|
                builder
                    .set_width(Pixels(100.0))
                    .set_height(Pixels(100.0))
                    .set_space(Stretch(1.0))
                    .set_child_space(Stretch(1.0))
            );
    });

    app.run();    
}
```

This code can be found in `examples/binding/basic.rs` and can be run by calling `cargo run --example basic`.

The example above will show a window with a label in the center dispaying the number 30. This might not seem like much, but that value originates from a piece of state which has been placed into the tree and the label as aquired the value without any manual events needing to be sent, bu just calling the `bind()` method.

There's a lot going on here, so in the next sections we'll take a deep dive into each part of the above example and build on it as we go.