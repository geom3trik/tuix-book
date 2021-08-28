# Reactivity and Model Data
In the previous sections we built a simple counter widget. This widget could now be embedded into a more complex gui and work just fine, keeping its own count. However, this isn't always the most practical solution. What if we had two counters and we wanted them to be synced to the same value? Okay we could send some events from one to the other but this isn't always so easy, especially if the widgets are 'far' away from each other within the tree.

The example of two counters being synced is a bit contrived but the issue of sharing application data between widgets is important and leads to reactivity as the solution. What is reactivity? For tuix, reactivity is the ability for widgets to 'bind' to application data, so that when the data changes, all the bound widgets update their state automatically. It's this ability which allows for more scalable applications.

In this section and the next we will modify our counter by adding another separate label, and syncing both our counter and the label to some shared data. 

## Model Data

We'll start with the counter example as we left it in the previous section. 

First we need to add a struct for the shared application data:

```rs
#[derive(Default)]
struct CounterState {
    value: i32
}
```
Just like the counter widget before, this struct contains an `i32` value to represent the count.

To allow this data to be embedded within the gui tree we need to implement the `Model` trait for it:

```rs
impl Model for CounterState {

}
```

The `Model` trait is similar to the `Widget` trait but is used for non-visual data. It also contains an `on_event` method to respond to events, and allows us to `build` the data into the tree. 

Modify the implmentation of `Model` on `CounterState` so that it updates the value in response to a `CounterEvent` within the `on_event` method, just like we did for our custom counter widget before:

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

Note the call to emit a `BindEvent::Update` event at each point the value is changed. Tuix doesn't (yet) provide a built-in method to detect changes of application data, so this event must be sent manually so bound widgets so they receive an update.

### Building Model Data

Now that we have our application data in a separate struct we need to build it into the app. Insert this code into the application closure above the call to create and build the counter widget:

```rs
let app_data = CounterState::default().build(state, window);
```

Note that the `build` function on `CounterState` does not contain a builder closure because this isn't a visual widget.

Next, make sure to change the parent of the counter to the application data:

```rs
Counter::default().build(state, app_data, |builder| builder);
```

We have now inserted the app data just below the root of the application (the window) but above everything else. This means that any events that are sent up from widgets in the app will make their way to the app data thanks to it being an ancestor of everything below it. 

Pressing the buttons of the counter will now modify the app data, but the label is still showing the internal value of the `Counter`. In the next section we'll modify our counter to remove the internal value and hook it up to our shared app data.
