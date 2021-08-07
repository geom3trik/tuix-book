# Events

So far we've seen how to add, position, and style widgets. However, this isn't very useful without the ability for widgets to communicate with each other.

This is where events come in. Inside of `State` there is a queue which events can be pushed to, and each update cycle the events are sent to the relevant widgets.
