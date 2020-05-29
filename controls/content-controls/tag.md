# Tag

_Inherits from_ [_RockMobile.StyledView_](../#styledview)\_\_

Tags are essentially labels which help to mark and categorize content. They usually consist of relevant keywords which make it easier to find and browse the corresponding piece of content. These are not a direct correlation with Rock Tags, but often Rock Tags will be presented as mobile tags.

## Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The text to display on the tag. |
| Type | string | The type \(color\) of the tag \(Primary, Secondary, Success, Info, Warning Danger\) |
| Size | string | The size of tag to display \(Default, Large, Small\) |
| TextColor | Color | Sets the color of the text using any of the supported formats. |
| BackgroundColor | Color | Sets the background color using any of the supported formats. |
| CornerRadius | float | The size of the corner radius to use for the tag. This can also be customized via CSS with the `border-radius` property. |
| BorderColor | Color | Sets the border color using any of the supported formats. |
| BorderThickness | Double | Sets the thickness of the border. This can be a single value or a specific value for Left, Top, Right, Bottom. |

```text
<Rock:Tag Type="Primary" Text="Articles" />
```

![](../../.gitbook/assets/image%20%286%29.png)

## CSS Tips

When styling tags via CSS it's helpful to understand how this control is built. The Tag contains two underlying controls:

1. StyledView - This is the container control.
2. Label - This is the text for the tag.

All tags have a `.tag` class attached to them. If you add a type an additional class with the pattern of `.tag-[typename]` will be applied. Similarly, size variations \(small & large\) will have appended `.tag-small` and `.tag-large` appended.

To style the text of a tag you'd want to have a style similar to:

```text
.tag ^label {
    color: red;
}
```

