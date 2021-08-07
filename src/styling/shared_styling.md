# Shared Styling

Tuix provides a way to define styles which can be shared between several widgets. This can reduce both complexity and memory usage since only the style data is stored in memory.

Shared styles in tuix follow the same rules as css stylesheets, with selectors determining which widgets are affected by which style rules. This section covers how to define shared styles in tuix. For an overview of selectors, see the [next section.]().

## Style Rules in Rust

TODO

## Stylsheets

An easier way to define shared styles is with the use of css. While tuix can parse a string or file containing css, not that many of the features of css are not supported, inlcuding the cascading which gives css a part of its name.

To add a css string to tuix, call `state.add_theme(css_string: &str)`. Tuix will then parse the supplied string for style rules and add them to the application. The css string can be defined as a constant, for example:

```rs
const STYLE: &str = #r"
    button {
        width: 100px;
        height: 30px;
        background-color: red;
    }
"#
```

Or the css string can be included from a file using the `include_str!()` macro. For example:

```rs
const STYLE: &str = include_str!("path_to_css_file");
```

Then, to include the styles in the tuix application call:

```rs
state.add_theme(STYLE);
```

### Hot Reloading of Stylesheets
Inlcuding the css string as a constant means that it cannot be updated while the program is running.

Tuix provides another method of including an external stylesheet within a .css file which can be modified and reloaded. To add a reloadable stylesheet call `state.add_stylesheet(path_to_css_file)`. This will load the contents of the file and parse any style rules. To reload the stylsheet while the application is running, press the F5 key.