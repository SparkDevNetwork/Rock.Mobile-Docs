# Custom CSS

You can add your own custom CSS to your application in the `Styles` tab of the application settings under the `Advanced Options` section.

![](../.gitbook/assets/css-styles.jpg)

Here you can add custom classes, target specific views, etc. There are several custom variables you can use in your CSS to represent colors and settings. These variables are listed below.

### Application Colors

| Variable | Value |
| :--- | :--- |
| ?color-primary | The primary color that is selected in the `Style` settings. |
| ?color-secondary | The secondary color that is selected in the `Style` settings. |
| ?color-success | The success color that is selected in the `Style` settings. |
| ?color-danger | The danger color that is selected in the `Style` settings. |
| ?color-warning | The warning color that is selected in the `Style` settings. |
| ?color-light | The light color that is selected in the `Style` settings. |
| ?color-dark | The dark color that is selected in the `Style` settings. |

### Palette Colors

[Palette colors](text/color.md#palette-colors) can be referenced with the following notation.

`?color-{color}-{intensity}`

Example  
`?color-orange-400`

## Alert Colors

Alert boxes are made up of three different colors:

* Background Color
* Text Color
* Border Color

These three colors are generated based of the Bootstrap alert recipe from the application colors above. The patterns for usage are:

`?color-{application-color}-{property}`

Example

`?color-primary-background`  
`?color-primary-border`  
`?color-primary-text`

## Misc Variables

Below are some additional variables you can reference:

| Variable | Value |
| :--- | :--- |
| ?radius-base | The base radius that is configured. |
| ?spacing-base | The default value to use for margins and padding. |
| ?font-size-default | The default font size. |
| ?color-text | The default color for text. |
| ?color-heading | The default color for use with headings. |
| ?color-background | The default background color. |

