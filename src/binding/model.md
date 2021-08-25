# Model

For convenience, repeated here is the basic binding example from the previous section:

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

The first part of this example is the creation of an `AppData` struct which contains a value with type `i32`. You've probably already spotted the `#[derive(Lens)]` macro but we'll save that for the next section.

Immediately after the declaration of this struct the trait `Model` is implmented for it. `Model` represents a piece of application state and allows us to embed it within the visual tree of widgets. However, a `Model` is not itself a `Widget`, and does not appear visually, but it can be used as a parent for a widget and can respond to events.

## Mutating the Model

In the basic example the value is hard-coded when an instance of the `AppData` is created, and there's currently no way to modify this value. This is where events come in. A `Model` can respond to events in the same way as a regular widget:

```rs
#[derive(Debug, Clone, PartialEq)]
pub enum AppEvent {
    Increment,
    Decrement,
}

impl Model for AppData {
    fn on_event(&mut self, state: &mut State, entity: Entity, event: &mut Event) {
        if let Some(app_event) = event.message.downcast() {
            match app_event {
                AppEvent::Increment => {
                    self.value += 1;
                    entity.emit(state, BindEvent::Update);
                }

                AppEvent::Decrement => {
                    self.value -= 1;
                    entity.emit(state, BindEvent::Update);
                }
            }
        }
    }
}
```

In the above code we've created an enum to represent some events (don't forget to derive `PartialEq`), and the implmentation of the `Model` trait for `AppData` has been modified to respond to these events, incrementing or decrementing the value.

Notice also the call to `entity.emit(state, BindEvent::Update)`. This event tells tuix that the model data has been modified and to update any widgets which are bound to it. Don't forget to call this when data has been modified!

With the above changes the model now has a way to be mutated. Now we just need to an `AppEvent` to the model. The easiest way to do this is to add some button and use the `on_press` callback. For example:

```rs
Button::with_label("Increment")
    .on_press(|data, state, button|{
        button.emit(state, AppEvent::Increment);
    })
    .build(state, app_data, |builder|
        builder
            .set_width(Pixels(100.0))
            .set_height(Pixels(30.0))
            .set_background_color(Color::rgb(50,50,50))
            .set_space(Stretch(1.0))
            .set_child_space(Stretch(1.0))
    );

Button::with_label("Decrement")
    .on_press(|data, state, button|{
        button.emit(state, AppEvent::Decrement);
    })
    .build(state, app_data, |builder|
        builder
            .set_width(Pixels(100.0))
            .set_height(Pixels(30.0))
            .set_background_color(Color::rgb(50,50,50))
            .set_space(Stretch(1.0))
            .set_child_space(Stretch(1.0))
    );

```

Notice that the buttons have the `AppData` as the parent. As long as the `AppData` instance is an ancestor of both the buttons and the label then the reactivity will work. Events are sent up to the model to mutate it and updates are sent back down to the widgets which are bound to it.

The complete code can be found in `examples/binding/model.rs`. Pressing the increment or decrement buttons causes the value to increase or decrease by one and the label changes automatically to show the new value.

