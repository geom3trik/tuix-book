# Setting Window Properties

Window properties can be set using the `WindowDescription`, which is the second argument of the closure passed to `Application::new()`.

```rs
fn main() {
    let app = Application::new(
        WindowDescription::new().with_title("Custom Title"), 
        |state, window|{}
    );
}
```

Using the builder pattern, setting window properties can be chained together:

```rs
fn main() {
    let app = Application::new(
        WindowDescription::new().with_title("Custom Title").with_inner_size(300, 300),
        |state, window|{}
    );
}
```

To see the full list of window properties that can be set, see the docs page on `WindowDescription`.