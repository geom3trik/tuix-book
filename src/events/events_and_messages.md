# Overview

Tuix has a built in system for generating and dispatching events to widgets in the hioerarchy. This is how widgets can respond to interaction, such as mouse clicks and keyboard presses, and also allows for widgets to communicate with each other.

Dispatched events are placed in a queue, and then the event manager sends the events to the relevant widgets which can respond by sending their own events. This process of receiving and sending events runs within a loop called the *Update Cycle* until there are no more events in the queue, at which point the application may redraw before waiting for the next OS event.

Unlike a game where the application is continuously updating and rerendering at a constant frame rate, a tuix application will only update when it receives an event from Operating System (except when an animation is playing). 



## Messages

All events in tuix are wrapped in an`Event` type which contains meta data, such as the origin, target, and propagation type of the event, as well as the **message** which is a boxed dynamic `Message` object.

Any type which implments `Debug`, `Clone`, and `PartialEq` automatically implements the `Message` trait and can be used within an `Event`. For example:

```rs

// Can be used as a message
#[derive(Debug, Clone, PartialEq)]
pub enum CustomEvent {
    DoSomething,
    DoSomethingWithValue(String),
}

// Create a new event with a message of CustomEvent::DoSomething with a target of entity
let event = Event::new(CustomEvent::DoSomething).target(entity);
```

## Event Propagation

The propagation path determines which widgets will receive an event when it is dispatched by the event manager.

There are four types of event propagation:

- **DownUp** - The event is sent from the root to the target and then back up to the root. This means that, unless the event is consumed, many widgets along the path, except for the target, will receive the event twice.
- **Down** - The event propagates down from the root to the target.
- **Up** - The event propagates up from the target to the root.
- **Fall** - The event propagates from the target down the branch to the last leaf widget.
- **Direct** - The event is sent directly to the target and no other widgets.

## Sending Events

Sending or dispatching an event is the process of adding the event to the internal event queue within `State`.

The `insert_event()` method on `State` allows for an event to be added to the event queue. The origin, target, and propagation type should be specified on the event before adding it to the queue.

### Convenience Functions
Because the process of dispatching an event using the `insert_event()` method can be quite verbose, tuix provides a set of convenience methods for sending events with a particular target and propagation type and can be called directly on an entity, which then becomes the origin. 

The following convenience functions take a message and generate the `Event` for you: 

- `entity.emit(message: impl Message)` - Sends an event with a message of `message`, with default propagation type (`DownUp`), and with `entity` as both the target and origin.
- `entity.emit_to(target: Entity)` - Sends an event with a message of `message`, with default propagation type (`DownUp`), with `target` as the target, and `entity` as the origin.





