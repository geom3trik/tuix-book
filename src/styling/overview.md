# Styling

Stlying lets us add properties to our widgets that change their visual appearance. Technically, layout propeties fall under styling as well, but for simplicity in learning the layout properties are detailed in a [separate section of the book](/layout/stack_overview.md).

# Available Properties

The table below shows the list of available style properties (layout properties are not shown for brevity). The property names are as they would appear in a css stylesheet. The property names in Rust are the same except dashes are replaced with underscores. For example, `background-color` in css becomes `background_color` in Rust.

| Property                   | Value Type           | Default Value        | Animatable |
|----------------------------|----------------------|----------------------|------------|
| display                    | None \| Flex         | Flex                 | No         |
| visibility                 | Visible \| Invisible | Visible              | No         |
| opacity                    | f32 (0.0 - 1.0)      | 1.0                  | Yes        |
| border-width               | Units                | Units::Auto          | Yes        |
| border-color               | Color                | Color::rgba(0,0,0,0) | Yes        |
| border-radius              | Units                | Units::Auto          | Yes        |
| border-radius-top-left     | Units                | Units::Auto          | Yes        |
| border-radius-top-right    | Units                | Units::Auto          | Yes        |
| border-radius-bottom-left  | Units                | Units::Auto          | Yes        |
| border-radius-bottom-right | Units                | Units::Auto          | Yes        |
| background-color           | Color                | Color::rgba(0,0,0,0) | Yes        |
| background-gradient        | LinearGradient       |                      | No         |
| background-image           | TODO                 |                      | No         |
| font                       | String               |                      | No         |
| color                      | Color                | Color::black()       | Yes        |
| font-size                  | f32                  | 14.0                 | Yes        |
| outer_shadow_h_offset      | Units                | Units::Auto          | Yes        |
| outer_shadow_v_offset      | Units                | Units::Auto          | Yes        |
| outer_shadow_blur          | Units                | Units::Auto          | Yes        |
| outer_shadow_color         | Color                | Color::rgba(0,0,0,0) | Yes        |
| inner_shadow_h_offset      | Units                | Units::Auto          | Yes        |
| inner_shadow_v_offset      | Units                | Units::Auto          | Yes        |
| inner_shadow_blur          | Units                | Units::Auto          | Yes        |
| inner_shadow_color         | Color                | Color::rgba(0,0,0,0) | Yes        |