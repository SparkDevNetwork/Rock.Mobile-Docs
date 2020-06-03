# Responsive Layout

_Inherits from_ [_Xamarin.Forms.Layout&lt;T&gt;_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout-1)\_\_

If you are familiar with Bootstrap's responsive grid system, this will be familiar to you. The ResponsiveLayout allows you to quickly build a layout that will be responsive to the size of the device that is currently being used to view the content. Smaller devices can display data in a vertical layout while larger devices can display the same content with multiple columns of content. It even works when rotating tablets to orientations.

The ResponsiveLayout takes any View as a child, but you would normally use the ResponsiveColumn as the direct child views. The ResponsiveColumn view provides additional properties that let you specify the various settings to indicate the width of a column on various device sizes. See the advanced section below for how to do the same with any generic view.

A row is identified as the child views required to "fill up" the number of available columns \(by default, 12\). And a single layout can contain multiple rows. Meaning, you don't need to add up 12 columns and then close the layout and start a new layout, you can just keep going. So if you use the default configuration of 12 available columns and have 4 total sub-views, each one set to use 6 columns on a Medium device, then that will end up laying out a 2x2 grid on a medium device or 4x1 \(4 rows, 1 column\) grid on a small and extra small device.

Speaking of device sizes, we define a number of them for you to use. These are known as the breakpoints. The width of the entire device is used in the calculation of which break-point to use. Below we use the term pixel, but be aware this is the "normalized" pixel. For example, your average Retina iPad actually has 2,048 pixels in landscape, but it reports to the application as 1,024.

* Extra Small: Devices &lt; 576px wide \(phones in portrait mode\)
* Small: Devices &gt;= 576px and &lt; 720px wide \(most phones in landscape, some small tablets in portrait\)
* Medium: Devices &gt;= 720px and &lt; 992px wide \(a few larger phones in landscape, most tablets in portrait\)
* Large: Devices &gt;= 992px and &lt; 1200px wide \(most tablets in landscape\)
* Extra Large: Devices &gt;= 1200px wide \(some larger tablets in landscape, such as the iPad Pro\)

Now, if we assume that you want something to display as a single column on Extra Small and Small devices, but two columns on Medium devices and above, you would only need to specify Medium to be a value of 6. The layout will determine the current break-point that should be used and then check if you have specified a column count. If not it will move down to the next smaller break-point size and repeat the process until finds a specified column count. If a column count is never specified, it is assumed to use the full-width of the layout. So in this example, Extra Small and Small devices would end up using 12 column segments \(one display column\) and Medium, Large and Extra Large devices would use 6 column segments \(two display columns\).

Each ResponsiveColumn can not only specify how many column segments it will take up at various device sizes, it can also optionally specify in what order that column is displayed. If you don't specify an order, then the order you defined the columns is used. But this lets you, for example, specify one order to be used for small devices and a different ordering used for medium and larger devices.

## Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| ColumnCount | int | The column count for this layout. This allows you to override the normal layout calculations and use a custom number of columns to make up an entire row. _Defaults to 12._ |
| ColumnSpacing | double | The amount of space between columns when they are laid out side by side. This lets you have multiple columns of text \(or other content\) in a larger device and ensure there is some margin between them so it doesn't appear to be one giant block of text. But when translated to a smaller device so a single column shows up, that margin goes away. _Defaults to 6.0._ |

## Example

```markup
<Rock:ResponsiveLayout>
    <Rock:ResponsiveColumn Small="7" Medium="9" ExtraSmallOrder="1" SmallOrder="0">
        <Label Text="This is our main body content." />
    </Rock:ResponsiveColumn>
    <Rock:ResponsiveColumn Small="5" Medium="3" ExtraSmallOrder="0" SmallOrder="1">
        <Label Text="This might be our navigation menu" />
    </Rock:ResponsiveColumn>
</Rock:ResponsiveLayout>
```

So the above is going to declare 3 total layout options. Let's cover each in turn.

Extra Small: On the smallest device, we will get two rows, each a single column wide. The first row will contain our "navigation menu". Even though it is declared second, we are overriding the order on the extra small devices so that it appears first. Then below that will come our "body content". Since we didn't specify an ExtraSmall column count \(and there is nothing smaller to check instead\) it defaults to the full-width of the layout.

Small: Next, on the small devices we will get one row of two columns. The left column will contain our "body content" and the right column will contain our "navigation menu". The width of the left column will be roughly 58% and the right column would be roughly 42%. On a small device, we need to give the menu a bit more room after all.

Medium: Finally, on medium and larger devices the layout will be the same, one row of two columns. The "body content" will again be on the left and the "navigation content" will be on the right. This time, because we have more room, we specified that the left column should be 75% wide and the right column only 25% wide.

## Advanced Example

The ResponsiveColumn inherits from a StackLayout, which means you can put multiple views inside the column without having to add your own StackLayout first. While there is nothing wrong with doing it this way even if you only need to specify a single view, performing layout calculations takes time. So if by chance you were using this to display a few dozen columns that each contained a single view, we are processing layout calculations on a few dozen StackLayout's that we don't need to.

You can achieve exactly the same results as our above example by using special attached properties. These are far more verbose, but you can shave some CPU cycles off the rendering of your page. Again, this is really only a concern if you are using dozens of these things.

```markup
<Rock:ResponsiveLayout>
    <Label Text="This is our main body content."
           Rock:ResponsiveLayout.Small="7"
           Rock:ResponsiveLayout.Medium="9"
           Rock:ResponsiveLayout.ExtraSmallOrder="1"
           Rock:ResponsiveLayout.SmallOrder="0" />
    <Label Text="This might be our navigation menu."
           Rock:ResponsiveLayout.Small="5"
           Rock:ResponsiveLayout.Medium="3"
           Rock:ResponsiveLayout.ExtraSmallOrder="0"
           Rock:ResponsiveLayout.SmallOrder="1" />
</Rock:ResponsiveLayout>
```

As you can see, that is indeed far more verbose so it isn't recommended unless you have a lot of these defined on a page and that page has some slowness in rendering. Places you might use this method are things like the Event Calendar page if you were using a responsive layout to define the layout of the individual calendar items in the list. That list might end up containing over a hundred items and that is a lot of CPU cycles to calculate layout that are wasted.

