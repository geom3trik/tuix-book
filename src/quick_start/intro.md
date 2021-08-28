# Introduction
This quick-start guide will cover the basic concepts for getting started with tuix. By the end of the guide we will have built a simple counter app with buttons to increment and decrement the count, as well as two labels showing different views on the same data. Here's a screenshot of the finished app:

<p align="center"><img src="../images/quick_guide/counter.png" alt="counter app"></p>

## Setup
Start by creating a new rust binary project in a location of your choosing by running the following command:

```sh
cargo new tuix_counter
```

This should create a new folder called `tuix_counter` which should contain a `src` directory, with a `main.rs` file, and a `Cargo.toml` file.

Open the `Cargo.toml` file in your editor of choice and add the following under the `[dependencies]` section:

```sh
tuix = { git = "https://github.com/geom3trik/tuix.git", branch = "reactive" }
```
This tells rust to include tuix as an external dependence.

Next, open the `main.rs` file and remove the hello world example code. When you're ready, move on to the next section where we'll build the simplest tuix app.