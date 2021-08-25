# A Simple Counter

For the rest of this quick start guide we will build a simple counter application.

## Counter State
To start with we will create a struct for the counter widget which will hold some data to keep track of the current count. We will also add a field for a label entity which we will create later, and also add a constructor which gives the counter an initial value of 50 and the label entity a default value.

```rs
struct Counter {
    count: i32,
    label: Entity,
}

impl Counter {
    pub fn new() -> Self {
        Self {
            count: 50,
            label: Entity::default(),
        }
    }
}
```

## Making the Counter a Widget
To turn the counter into a widget, we just need to implement the `Widget` trait for the `Counter` type.

```rs
impl Widget for Counter {
    type Ret = Entity;
    type Data = ();

    fn on_build(&mut self, state: &mut State, entity: Entity) -> Self::Ret {

    }
}
```

The `Widget` trait contains 2 associated types, `Ret` and `Data`, as well as a method, `on_build` which must be implemented.

For now we will ignore the `Data` associated type and return to this later. The `Ret` associated type is used to specify the return type of the `on_build` method, which is returned when `build` is called for an instance of that widget. Usually, the `on_build` method will return the entity id of the widget so it can be used as a parent for other widgets. However, sometimes it is useful to be able to return multiple entities, such as for a tab container which returns the id of the tab bar and the id of the tab view, in which case the `Ret` type would be set to a tuple of entities, `(Entity, Entity)`.

## Adding Widgets to the Counter
The `on_build` method is where we will construct the sub-widgets which make up our custom counter widget. For this example we will add two buttons, one for increment and one for decrement, and a label to show the current counter value.

```rs
impl Widget for Counter {
    type Ret = Entity;
    type Data = ();

    fn on_build(&mut self, state: &mut State, entity: Entity) -> Self::Ret {
        
        Button::with_label("increment")
            .build(state, entity, |builder| builder.class("increment"));

        Button::with_label("decrement")
            .build(state, entity, |builder| builder.class("decrement"));

        self.label = Label::new(&self.value.to_string()).build(state, entity, |builder| builder);

        entity.set_element(state, "counter")
    }
}
```

At the end of the `on_build` method we add a call to `set_element` to give our counter an element name. This will be used later when adding a style to our application.

## Styling the Counter

Now that our `Counter` is a widget, we can create a simple application which contains an instance of our counter widget.

```rs
fn main() {
    let window_description = WindowDescription::new()
        .with_title("Counter")
        .with_inner_size(400, 100);
        
    let app = Application::new(window_description, |state, window| {
        
        Counter::new().build(state, window, |builder| builder);

    });

    app.run();
}
```

Building and running this code will result in a seemingly empty window. To see the counter with its buttons and label we need to add some styling. For this we will use an external stylesheet with the following style rules:

```css
button {
    font-size: 16px;
    child-space: 1s;
    border-radius: 3px;
    width: 100px;
    height: 30px;
}

button {
    background-color: #6200ee;
}

button:hover {
    background-color: #6e14ef;
}

button:active {
    background-color: #a56bf5;
}

label {
    border-color: #9e9e9e;
    color: black;
    border-width: 1px;
    width: 100px;
    height: 30px;
    border-radius: 3px;
    child-space: 1s;
    child-left: 5px;
}

counter {
    layout-type: row;
    child-between: 1s;
    width: 1s;
    child-space: 1s;
}
```

## Adding Counter Events
Currently we can see the buttons and click on them but they do nothing. To fix this we first need a custom event message which we will call `CounterMessage`.

```rs
#[derive(Debug, Clone, PartialEq)]
enum CounterMessage {
    Increment,
    Decrement,
}
```

With this event type defined we can now implement the `on_event` method in the `Widget` trait for our counter to respond to this event and muatate its internal state. In this case to increment/ decrement the count value and update the text of the label accordingly.

```rs
impl Widget for Counter {
    type Ret = Entity;
    type Data = ();

    fn on_build(&mut self, state: &mut State, entity: Entity) -> Self::Ret {
        ... // Unchanged
    }

    fn on_event(&mut self, state: &mut State, entity: Entity, event: &mut Event) {
        if let Some(counter_event) = event.message.downcast::<CounterMessage>() {
            match counter_event {
                CounterMessage::Increment => {
                    self.value += 1;
                    self.label.set_text(state, &self.value.to_string());
                    event.consume();
                }

                CounterMessage::Decrement => {
                    self.value -= 1;
                    self.label.set_text(state, &self.value.to_string());
                    event.consume();
                }
            }
        }
    }
}
```