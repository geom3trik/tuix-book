# Creating a Tuix Application

The most basic tuix app looks like this:

```rs
use tuix::*;

fn main() {
    let mut app = Application::new(WindowDescription::new(), |state, window| {});

    app.run();
}
```

The first argument passed to `Application::new()` is a new instance of a `WindowBuilder`. This allows us to set the initial properties of the root window created for us by tuix. 

The second argument passed to the new method is a closure which provides us with two variables:

1. `state` - This is a mutable reference to the UI `State`, which represents the 'global' data of the widgets in a gui application, such as layout and style. A mutable reference to state is passed around when building widgets, handling events, and drawing widgets.

2. `window` - This is an `Entity` id to the window widget created for us by tuix. Every widget has an entity id which is used with state to modify UI properties.

Running this code with `cargo run` will produce an empty gray window with a width of 800 pixels and a height of 600 pixels. This isn't very interesting, so in the next section we'll cover changing window properties like size, title and icon.


