# Image

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

One of the most common controls you'll want to use is the image control. Because of it's importance effort has been invested into ensuring that it has a all the power you need. Let's start with the basics.

```text
<Rock:Image Source="https://server.com/photo.jpg" />
```

Is that it? No, we're just getting started. Below are all of the properties you can add to images.

![Basic Image](../../.gitbook/assets/image%20%2821%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Aspect</td>
      <td style="text-align:left">Aspect</td>
      <td style="text-align:left">
        <p>Determines how the image will fill the space allotted. Valid values are:</p>
        <p>AspectFill - Fill the space with the image, some parts of the image may
          be cropped.</p>
        <p><b>AspectFit</b> - Scale the image to fit the space, some parts of the
          space may be left empty.</p>
        <p>Fill - Scale the image to exactly fill the space, this may warp the image.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BackgroudColor</td>
      <td style="text-align:left">Color</td>
      <td style="text-align:left">The color to use for the background. This is useful if you&apos;d like
        to show a placeholder color while the image downloads.</td>
    </tr>
    <tr>
      <td style="text-align:left">HeightRequest</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The height you&apos;d like your image to be.</td>
    </tr>
    <tr>
      <td style="text-align:left">WidthRequest</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The width you&apos;d like your image to be.</td>
    </tr>
    <tr>
      <td style="text-align:left">HorizontalOptions</td>
      <td style="text-align:left">LayoutOption</td>
      <td style="text-align:left">
        <p>This describes how the element is laid out horizontally within the parent
          , and how this element should consume leftover space on the X axis. Common
          values would be:</p>
        <p>Center - centered and does not expand.</p>
        <p>FillAndExpand - Fills the whole area.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Ratio</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Determines the size of the image based on the width of the parent container.
        The format is height:width (e.g. &apos;4:2&apos;).</td>
    </tr>
    <tr>
      <td style="text-align:left">VerticalOptions</td>
      <td style="text-align:left">LayoutOption</td>
      <td style="text-align:left">This describes how the element is laid out vertically within the parent
        , and how this element should consume leftover space on the Y axis.</td>
    </tr>
    <tr>
      <td style="text-align:left">Margin</td>
      <td style="text-align:left">Thickness</td>
      <td style="text-align:left">Images, like most controls can have margins. This is typically note as <code>Margin=&quot;Left,Top,Right,Bottom&quot;</code>.</td>
    </tr>
  </tbody>
</table>

Done now? Nope still have much more to consider.

## Image Transformations

You can apply several different transformations on your images. Each is discussed below.

**Blur**

You can easily add a blur to your image with this simple transformation.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:BlurTransformation Radius="4" />
</Rock:Image>
```

| **Property** | Type | Description |
| :--- | :--- | :--- |
| Radius | Float | The amount of blur to add. |

![Blur Transform](../../.gitbook/assets/image%20%284%29.png)

**Circle**

The circle transformation masks your images into a circle shape. The syntax for this is below.

```text
<Rock:Image 
    Source="https://server.com/photo.jpg" 
    BackgroundColor="#c4c4c4"
    HeightRequest="128"
    WidthRequest="128"
    Aspect="AspectFill" 
    HorizontalOptions="Center" 
    VerticalOptions="Fill">
        <Rock:CircleTransformation 
            BorderSize="4" 
            BorderColor="rgba(255, 255, 255, 0.58)" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| BorderSize | int | The size of the optional border around the image. |
| BorderColor | Color | The color of the border around the image. |

![Circle Transform](../../.gitbook/assets/image%20%2810%29.png)

**Drop Shadow**

The filter adds a customizable drop shadow to your images.

```text
<Rock:Image 
    Source="https://server.com/photo.jpg" 
    BackgroundColor="#c4c4c4"
    HeightRequest="300"
    WidthRequest="300"
    Aspect="AspectFill" 
    HorizontalOptions="Center" 
    VerticalOptions="Fill">
        <Rock:DropShadowTransformation 
            Distance="8"
            Angle="135"
            Radius="4"
            Color="#c4c4c4"
            Opacity="0.5" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Distance | double | Determines how far the drop shadow should extend below the image. |
| Angle | double | Sets the direction the drop shadow should extend from. |
| Radius | double | Determines the level of blur the drop shadow should use. |
| Color | Color | The color of the drop shadow. |
| Opacity | double | The opacity of the shadow. |

![Drop Shadow Transformation](../../.gitbook/assets/image%20%2826%29.png)

`Note: When using the drop shadow transformation be sure you do not have a background color. Otherwise the background color will cover the drop shadow.`

**Fill Color**

This fills the image with the selected color. Not sure why you'd ever use this? Well there's a great usage for this when used with masks on layered images.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:FillColorTransformation Color="#41BFD0" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Color | Color | The color to fill the image with. |

![Fill Transformation](../../.gitbook/assets/image%20%288%29.png)

**Flip**

Flips the image either horizontally, vertically or both.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:FlipTransformation Direction="Both" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Direction | FlipDirection | Valid values include: Horizontal, Vertical or Both |

![Flip Transformation](../../.gitbook/assets/image%20%289%29.png)

**Grayscale**

Converts the image to gray scale.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:GrayscaleTransformation Saturation="0" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Saturation | double | Determines this level of color saturation. A value of `1.0` will not change the original image. Using `0.0` will make the image fully grayscale. You can also provide `-1.0` to invert the image. |

![Grayscale Transformation](../../.gitbook/assets/image%20%2812%29.png)

**Reflection**

Draws a reflection of the image as if the image were sitting on a glass surface.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:ReflectionTransformation Size="96" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Size | double | The size of the reflection. |

![Reflection Transformation](../../.gitbook/assets/image%20%2827%29.png)

**Rounded**

Rounds the corners of the image and optionally adds a border.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:RoundedTransformation 
        CornerRadius="120, 0, 0, 120" 
        BorderSize="4" 
        BorderColor="rgba(255, 255, 255, 0.58)" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| CorderRadius | CornerRadius | You can provide a specific radius for each corner, or provide one value to be used for all of them. |
| BorderSize | float | The size of the border to optionally apply. |
| BorderColor | Color | The color of the border to apply. |

![Rounded Transformation](../../.gitbook/assets/image%20%2816%29.png)

**Tint**

Tints the image using the provided color.

```text
<Rock:Image Source="https://server.com/photo.jpg" >
    <Rock:TintTransformation Color="#41BFD0" />
</Rock:Image>
```

| Property | Type | Description |
| :--- | :--- | :--- |
| Color | Color | The color to use to tint the mid-tones of the image. |

![Tint Transformation](../../.gitbook/assets/image%20%287%29.png)

Keep in mind you can use more than one transformation on a single image.

Now are we done? Not quite, what's the rush?

## Layering Images

Want to go to the next level with you're images? Layer them! Look at the sample below:

```text
<RelativeLayout>
    <Rock:Image 
        Source="https://rockrms.blob.core.windows.net/resources/mobile-demo/image-mountain.jpg" 
        Aspect="Fill"
        HeightRequest="360"
        HorizontalOptions="FillAndExpand"
        RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,
             <Rock:TintTransformation Color="#41BFD0" />
     </Rock:Image>
     <Rock:Image 
         Opacity="1"
         Source="https://rockrms.blob.core.windows.net/resources/mobile-demo/image-mask.png" 
         Aspect="Fill"
         HeightRequest="360"
         HorizontalOptions="FillAndExpand"
         RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,
               <Rock:FillColorTransformation Color="#41BFD0" />
      </Rock:Image>
</RelativeLayout>
```

This produces the image below.

![Results of Layering Images](../../.gitbook/assets/image%20%2818%29.png)

To make this we just stack the original mountain under a mask. Note how the mask is just a PNG with an alpha channel. Notice how the mask is black. Applying a fill transformation allows us to match the tint we added to the mountain photo producing a nice color vignette effect.

![](../../.gitbook/assets/image%20%2824%29.png)

