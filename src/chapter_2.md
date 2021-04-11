# Setting Window Properties

Window properties can be set using the `WindowBuilder`, which is the second argument of the closure passed to `Application::new()`.

```rs
fn main() {
    let app = Application::new(|state, window|{
        window.set_title("Custom Title");
    })
}
```

Setting properties can also be chained together:

```rs
fn main() {
    let app = Application::new(|state, window|{
        window.set_title("Custom Title").set_inner_size(400,300);
    })
}
```

## List of Window Properties

#### Window Title
Sets the title of the window.
```rs
set_title(title: &str)
```

#### Window Inner Size
Sets the inner size of the window.
```rs
set_inner_size(width: u32, height: u32)
```

#### Window Inner Size
Sets the minimum inner size of the window.
```rs
set_min_inner_size(width: u32, height: u32)
```

#### Window Icon
Sets the window icon.
```rs
set_icon(&mut self, icon: Vec<u8>, width: u32, height: u32)
```

The icon must first be loaded using the image crate. Example:

```rs
let icon = image::open("resources/icons/calculator_dark-128.png").unwrap();

window.set_icon(icon.to_bytes(), icon.width(), icon.height());
```