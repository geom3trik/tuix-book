# Lens

In the previous sections we demonstrated binding a label to some data in a `Model` and used events to mutate the model and the text of the label updated automatically. This all seems to be possible simply by calling the `bind` method on a widget, so what exactly does this do, and what are its arguments?

For the basic example the binding looked like this:

```rs
.bind(AppData::value, |value| value.to_string())
```

The first argument to this method is a `Lens`. You can think of a lens as a function which takes some data as input and returns a piece of that data as output, usually as references. For example, what we need for the basic example is a function which takes an `AppData` and returns the value, an `i32`.

But the lens in the `bind` method above doesn't look like a function. So what's going on here?

This is where the `#[derive(Lens)]` macro comes in. There is a function but it's within a trait called `Lens` which looks like this:

```rs
pub trait Lens {
    type Source;
    type Target;

    fn view<'a>(&self, data: &'a Self::Source) -> &'a Self::Target;
}
```

The derive macro creates for us a zero-sized static type and then implments the `Lens` trait, which might look something like this:

```rs
pub struct SomeGeneratedType;

impl Lens for SomeGeneratedType {
    type Source = AppData;
    type Target = i32;

    fn view<'a>(&self, data: &'a AppData) -> &'a i32 {
        &data.value
    }
}
```

The other thing that the derive macro does is to create a static instance of the generated type, with the same name as the field (`value`), within a module called `AppData`. This is what allows us to use `AppData::value` to refer to the lens.

The second argument to the bind method is a converter closure which has as input the target type of the lens, in this case a reference to an `i32` value, and has as output the expected input of the label, in this case an owned `String`. Therefore, to convert between the two types we use the `.to_string()` method on the value.