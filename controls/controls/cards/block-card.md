---
description: >-
  The block card is very similar to the Contained Card but the content is not in
  a clearly visible frame.
---

# Block Card

Below is a listing of the various properties for the block card.

| Property | Type | Description |
| :--- | :--- | :--- |
| AdditionalContent | object | The text or controls to add to the content of the card.  If text is provided it will be converted to paragraphs based on line breaks. |
| Command | ICommand | The command to use when the card is clicked. |
| CommandParameter | object | The parameters to set to the command when clicked. |
| Image | string | The URL of the image. |
| ImageLocation | VerticalAlignment | Determines where the image should be placed on the card. \(**Top**, Bottom\) |
| ImageMask | string | The URL or name of the image to use for the mask. |
| ImageMaskColor | Color | The color to use to tint the mask. |
| ImageMaskOpacity | double | The opacity to use on the mask. |
| ImageOpactity | double | The opacity of the image. |
| ImageRatio | string | Determines the size of the image based on the width of the parent container. The format is height:width with a default of 2.1:4. |
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

