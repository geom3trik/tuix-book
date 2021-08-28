# Reactivity and Binding
In the previous sections we built a simple counter widget. This widget could now be embedded into a more complex gui and work just fine, keeping its own count. However, this isn't always the most practical solution. What if we had two counters and we wanted them to be synced to the same value? Okay we could send some events from one to the other but this isn't always so easy, especially if the widgets are 'far' away from each other within the tree.

The example of two counters being synced is a bit contrived but the issue of sharing application data between widgets is important and leads to reactivity as the solution. What is reactivity? For tuix, reactivity is the ability for widgets to 'bind' to application data, so that when the data changes, all the bound widgets update their state automatically. It's this ability which allows for more scalable applications.

In the following parts of this section we'll modify our counter by adding another separate label, and syncing both our counter and the label to some shared data. 

## Model Data

We'll start with the counter example as we left it in the previous section. First we'll add a struct for the shared application data:

```rs
#[derive(Default)]
struct CounterState {
    value: i32
}
```
Just like the counter widget before, this struct contains an `i32` value to represent the count.

To allow this data to be embedded within the gui tree we need to derive the `Model` trait for it:

```rs
impl Model for CounterState {

}
```

### Mutating the Model

This `Model` is similar to the `Widget` trait, containing an `on_event` method to respond to events, and allowing us to `build` the data into the tree. Modify the implmentation of `Model` on `CounterState` so that it updates the value in response to a `CounterEvent` within the `on_event` method:

```rs
impl Model for CounterState {
    fn on_event(&mut self, state: &mut State, entity: Entity, event: &mut Event) {
        if let Some(counter_event) = event.message.downcast() {
            match counter_event {
                CounterEvent::Increment => {
                    self.value += 1;
                    entity.emit(state, BindEvent::Update);
                }

                CounterEvent::Decrement => {
                    self.value -= 1;
                    entity.emit(state, BindEvent::Update);
                }
            }
        }        
    }
}
```

Note the call to emit a `BindEvent::Update` event at each point the value is changed. Tuix doesn't (yet) provide a built-in method to detect changes of application data, so this event must be sent manually so bound widget receive an update.

### Building Model Data

Now that we have our application data we need to build it into the app. Insert this code into the application closure above the call to create and build the counter widget:

```rs
let app_data = CounterState::default().build(state, window);
```

Note that the `build` function on `CounterState` does not contain a builder closure because this isn't a visual widget.

Next, make sure to change the parent of the counter to the application data:

```rs
Counter::default().build(state, app_data, |builder| builder);
```

We have now inserted the app data just below the root of the application (the window). This means that any events that are sent up from widgets in the app will make their way to the app data thanks to it being an ancestor of everything below it. Pressing the buttons of the counter will now modify the app data, but the label is still showing the internal value of the `Counter`.

Let's remove the `on_event` of the `Counter` and the `value` field as well. Now the counter contains no count value, so we need to hook it up to the app data count value. For this we will use lenses.

Modify the declaration of the `CounterState` struct to add the `Lens` derive macro like so:

```rs
#[derive(Default, Lens)]
struct CounterState {
    value: i32,
}
```

## Binding

Now we can bind the label in our counter to the value in the counter state. Change the label to the following by adding the `bind` method:

```rs
Label::new("0")
    .bind(CounterState::value, |value| value.to_string())
    .build(state, row, |builder| 
        builder
            .set_width(Pixels(100.0))
            .set_height(Pixels(30.0))
    );
```

Note that we no longer need to assign the return to a local variable, so we can now also remove `label` from `Counter`, leaving it empty.

Let's break down what's going on here. The `bind` method is placed between the creation of the label instance and the call to `build`. It has two arguments, a lens and a closure:

1. The first argument is a lens. What's a lens? A lens can be thought of as something which 'selects' a piece of a larger structure, in this case the `value` field of the `CounterState` struct. To see how this works in more detail, see the book section on [lenses](../binding/lens.md);
2. The second argument is a closure which acts as a converter for the lensed data. The lens returns an `i32` but the label expects a `String`, so we use the `.to_string()` method to convert the value to the expected type.

### Binding Another Label

Now when we run the code we seemingly get the same counter behaviour as before. The buttons update the value, via events, and the label receives the new value and updates its display. So why did we do this? Well, now we can do somthing more interesting, like bind another widget to the same value and display it in a different way, like printing the value as english text. Add the following dependency to the `Cargo.toml` file:

```sh
english-numbers = "0.3.3"
```

Then add a label to the application below the counter with the following `bind` method:

```rs
Label::new("Zero")
    .bind(CounterState::value, |value| english_numbers::convert_all_fmt(*value as i64))
    .build(state, data_widget, |builder| 
        builder
            .set_space(Pixels(5.0))
    );
```
We bind the label to the same value and use the conversion closure to call a method from the `english_numbers` crate to convert the value to an english word. Now when the buttons are pressed to increment and decrement the counter, both labels update to show the value in two different ways.

The full code for this reactive counter can be found in the `/examples/counter_reactive.rs` file in the tuix repository and can be run with `cargo run --release --example counter_reactive`.

<p align="center"><img src="../images/quick_guide/counter_reactive.png" alt="tuix app"></p>

# Conclusion

This concludes the tuix 'quick' guide! If you made it this far, thanks for reading! The rest of this book attempts to cover the concepts shown in this guide in more detail, demonstrating what features are available and how they can be used to build complex user interfaces.

## Coming Soon...

Coming soon will be an 'advanced' guide in which a todo app will be created with more advanced features such as:
    - Binding to lists of items
    - 'Local' app data for syncing widgets in a sub-section of the tree
    - Animations (almost all style and layout properties are animatable)
    - Using more complex built-in widgets
    - Constructing more complex custom widgets
