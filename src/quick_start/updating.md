# Making the Counter Reusable

Okay so we've got a reactive counter. It's reactive because when the value changes, via a button press, both labels update to show the new value without us having to explicitly send events to them.

However, with the way we've done it, our counter widget can't be used in another application unless it too has the same `CounterState` application data. We need to make our counter more generic.

When using our `Counter` widget in an app we won't have access to the label as it's built into the widget. So instead of binding to the label, we need to be able to bind to the counter widget itself.

## The Data Associated Type

This is where the `Data` associated type comes in from the `Widget` trait, which we previously set to `()`. This associated type is used to specify the type of data a widget is expected to receive. It was mentioned before that a `Label` expects a `String`, which is because its associated data type is `type Data = String`.

Let's modify our counter to remove the `bind` method on the `Label` within the `Counter` widget, and change the data associated type to `type Data = i32`. 

## On Update

For actually updating the counter we'll use the `on_update` method provided by the `Widget` trait. Add the following code to the implementation of `Widget` for our `Counter` below the `on_update` method:

```rs
fn on_update(&mut self, state: &mut State, entity: Entity, data: &Self::Data) {
    self.label.set_text(state, &data.to_string());
}
```

As with the `on_build` and `on_event` methods, the first three arguments are the same: a mutable reference to `Self` so internal widget data can be modified, and a mutable reference to `State` followed by an `Entity` id, for modifying style and layout properties as well as for sending events.

In this case we use the internal `label` field (which is the entity id of label) to set its text to the `data` value (converted from `i32` to a string) received when our counter widget is updated.

The last step is to add a call to `bind()` on the counter widget. Change the building of the counter widget to the following:

```rs
Counter::default()
    .bind(CounterState::value, |value: &i32| *value)
    .build(state, app_data, |builder| builder);
```

We pass the same lens as before to the value of the `CounterState` and the conversion closure converts the reference to the value into a copy.

So how does this work? 
1. Building the `Model` created a `Store`, which itself is a widget which contains the application data as well as a list of observer widgets (using entity ids).
2. The `bind` method sends an event to the store which registers the widget as an observer.
3. Pressing a button sends an event up the tree to the store, which then mutates the data in response.
4. The store then calls the `on_update` method of all bound widgets, passing the new updated value.

And we're done!

When we build and run our app now we get the same behavior as before but our counter widget is now re-usable! We could now add this counter widget into any application, and we just need to bind it to a piece of data, using the conversion closure to convert that data in some way to an `i32`. The label would then display the value. The only other part required is to respond to `CounterEvent` messages emitted by the counter's buttons.

The full code for this reactive counter can be found in the `/examples/counter_reactive.rs` file in the tuix repository and can be run with `cargo run --release --example counter_reactive`.

# Conclusion

This concludes the tuix 'quick' guide! If you made it this far, thanks for reading! The rest of this book attempts to cover the concepts shown in this guide in more detail, demonstrating what features are available and how they can be used to build complex user interfaces.

## Coming Soon...

Coming soon will be an 'advanced' guide in which a todo app will be created with more advanced features such as:
- Binding to lists of items
- 'Local' app data for syncing widgets in a sub-section of the tree
- Animations (almost all style and layout properties are animatable)
- Using more complex built-in widgets
- Constructing more complex custom widgets