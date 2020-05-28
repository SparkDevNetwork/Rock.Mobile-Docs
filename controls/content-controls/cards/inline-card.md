---
description: >-
  The Inline Card places the image behind the content and provides several
  options to create striking designs.
---

# Inline Card

_Inherits from_ [_StyledView_ ](../styled-view.md)_&gt;_ [_PancakeView_](https://github.com/sthewissen/Xamarin.Forms.PancakeView)

![Inline Card](../../../.gitbook/assets/image%20%2838%29.png)

Below is a listing of the various properties for the inline card.

| Property | Type | Description |
| :--- | :--- | :--- |
| AdditionalContent | object | The text or controls to add to the content of the card.  If text is provided it will be converted to paragraphs based on line breaks. |
| BackgroundMask | string | The URL or name of the image to use for the mask. |
| BackgroundMaskColor | double | The color to use to tint the mask. |
| BackgroundMaskOpacity | double | The opacity to use for the mask. |
| BorderThickness | float | The thickness of the border. |
| BorderColor | Color | The color of the border. |
| CardRatio | string | Determines the size of the image based on the width of the parent container. The format is height:width with a default of 4:3. |
| Command | ICommand | The command to use when the card is clicked. |
| CommandParameter | object | The parameters to set to the command when clicked. |
| Elevation | int | The Material Design elevation desired. |
| HasShadow | bool | Determines if a shadow should be shown behind the card. |
| Image | string | The URL of the image. |
| ImageLocation | VerticalAlignment | Determines where the image should be placed on the card. \(**Top**, Bottom\) |
| ImageOpactity | double | The opacity of the image. |
| ImageSaturation | double | The saturation of the image. |
| LeftDescription | object | The text or controls to place in the left description. |
| RightDescription | object | The text or controls to place in the right description. |
| Tag | string | The text for the card's tag. |
| TagBackgroundColor | Color | The background color of the tag. |
| TagTextColor | Color | The text color of the tag. |
| Tagline | string | The text to display for the tag line. |
| Title | string | The text to display for the title. |
| TitleMaxLines | int | The maximum number of lines the title can be \(default 2\). |
| TitleJustification | HorizontalAlignment | The text alignment for the title \(**Left**, Right, Center\) |

