# Styled View

_Inherits from_ [_PancakeView_](https://github.com/sthewissen/Xamarin.Forms.PancakeView)

Ever wished you could make your layouts do... more? A little more pizzazz, a little more wow factor, a little more... style. This view lets you do just that. Most of it's functionality centers on borders and what you can do with them. However, it's not just about adding a border. Because you can do that now. The power really comes when you start to apply things like corner radius to individual corners, add gradients to your border color, gradient background colors, and much more.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| BackgroundGradientAngle | int | A value between 0-360 to define the angle of the background gradient. |
| BackgroundGradientStartColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The start color of the background gradient. |
| BackgroundGradientEndColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The end color of the background gradient. |
| BackgroundGradientStops | GradientStop\[\] | A list of GradientStop objects that define a multi color background gradient. |
| BorderColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color of the border. |
| BorderGradientAngle | int | A value between 0-360 to define the angle of the border gradient. |
| BorderGradientStartColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The start color of the border gradient. |
| BorderGradientEndColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The end color of the border gradient. |
| BorderGradientStops | GradientStop\[\] | A list of GradientStop objects that define a multi color border gradient. |
| BorderIsDashed | boolean | Whether or not the border needs to be dashed. |
| BorderThickness | float | The thickness of the border. |
| CornerRadius | CornerRadius | The radius of each of the four corners. _Specified as "topLeft,topRight,bottomLeft,bottomRight"._ |
| HasShadow | bool | Whehter or not to draw a shadow beneath the control. _Note: For this to work we need to clip the view. This means that individual corner radii will be lost. In this case the TopLeft value will be used for all corners._ |
| Elevation | int | The Material Design elevation desired. _Note: For this to work we need to clip the view. This means that individual corner radii will be lost. In this case the TopLeft value will be used for all corners._ |
| OffsetAngle | double | The rotation of the StyledView when `Sides` is not its default value of 4. |
| Sides | int | The number of sides to the shape. _Default is 4._ |

**Example**

```text
<Rock:StyledView BackgroundColor="#bc91d7"
                 CornerRadius="60,0,0,60"
                 IsClippedToBounds="true"
                 HorizontalOptions="FillAndExpand"
                 HeightRequest="150" />
```

