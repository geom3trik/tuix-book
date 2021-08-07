# Handling Events

Each update cycle, events in the queue will be dispatched to their target widgets. There are two primary methods for handling events sent to widgets:

1. **Event Handlers** allow for handling events in the same way for all intances of a particular widget type. For example, all button widgets should become 'active' when pressed.

2. **Callbacks** allow for handling events on a per instance basis. For example, one button widget might trigger the window to close while another button might trigger the window to go fullscreen.

## Event Handlers
To receive and respond to an event, a widget type must implement the `on_event()` method of the `Widget` trait.

Here is an example from the `Button` widget for responding to a left mouse button press on the widget:

```rs
...
fn on_event(&mut self, state: &mut State, entity: Entity, event: &mut Event) {
    
    if let Some(window_event) = event.message.downcast::<WindowEvent>() {
        match window_event {
            WindowEvent::MouseDown(button) if *button == MouseButton::Left => {
                // Code which runs when the left mouse button is pressed 
                // on the button widget (left out for brevity)
                ...
            }

            ...
        }
    }
}
...
```

Becuase the messages within events are boxed dynamic objects, the message must first be cast to the desired type with the `downcast()` method. Here we have specified the message type to cast to but Rust can actually infer this from the match statement that follows.

Once the message is the correct type, we can do things like match on the message type (if it's an enum) to respond to different message variants. In the above example the `WindowEvent` message contains a `MouseDown` variant which contains the mouse button which was pressed. A match guard is used to check if the mouse left button was pressed in which case it runs the contained code (left out for brevity).

Since the `on_event` method provides mutable access to the local properties of the widget, through `self`, and mutable access to the global state, through `state` using `entity`, there are a number of things that can be done within an event handler, including:

- Setting local widget properties through `self`.
- Setting global widget properties through `state` using the widget `entity`.
- Sending events.
- Adding resources to State.

## Callbacks
Callbacks are closures (functions), stored within a widget, which are triggered when a particular event is received. For example, the `Button` widget contains `on_press` and `on_release` callbacks which are triggered when the button is pressed (with the left nouse button) and released respectively.

Internally, this is acheived by handling the `WindowEvent::MouseDown` event within the event handler (`on_event` method) of the button which then calls the stored closures.

This example creates a new button with a callback which closes the window when pressed:

```rs
Button::new()
    .on_press(|widget: &mut Button, state: &mut State, button: Entity| {
        button.emit(state, WindowEvent::CloseWindow);
    })
    .build(state, parent, |builder| builder);
```

For clarity, the closure argument types have been added. 

A callback can have many forms but for the standard callbacks provided by the default widgets within tuix, the arguments to the closure mirror the arguments to the `on_event` method, allowing for the modification of local and global properties. 

In the above example the first arguement is unused as no local properties are required. However, a widget such as the `Slider` contains the current value, which can be used within one of the callbacks of the slider, such as the `on_changing` callback:

```rs
// Prints the current value of the slider while the slider value is changing,
// either by pressing the track or dragging the thumb along the track.
 Slider::new()
    .on_changing(|slider, state, entity| {
        entity.emit(WindowEvent::Debug(format!("Value: {}", slider.value)))
    })
    .build(state, parent, |builder| builder)

```


