# Paragraph Text

_Inherits from_ [_Xamarin.Forms.StackLayout_](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/stack-layout)\_\_

Using a normal Label for text works great, but if you have several paragraphs worth of text it won't have the best typography. Correctly styled text should have a good amount of spacing between paragraphs. This helps individuals to read the content quicker. Using a single Label will give you the the proper line returns, but it won't have the best spacing. 

The ParagraphText control allows you provide a text string that contains several paragraphs worth of content. It will parse this an ensure each paragraph get's it's own Label. These labels will have a CSS style class of `body` applied to them. It's this class that applies to correct amount of margin below the Label. You can also override this class to provide a different sized text/margin.

#### Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The text to show. This can also be provided via the content of the tag. |
| LabelCssClass | string | A string of the CSS classes to apply to each Label that represents a paragraph. If no value is applied each paragraph will have a `body` class added to it. |

#### Example

```text
<Rock:ParagraphText>
    Leverage agile frameworks to provide a robust synopsis for high level 
    overviews. Iterative approaches to corporate strategy foster collaborative 
    thinking to further the overall value proposition. Organically grow the
    holistic world view of disruptive innovation via workplace diversity 
    and empowerment.
    Bring to the table win-win survival strategies to ensure proactive 
    domination. At the end of the day, going forward, a new normal that has 
    evolved from generation X is on the runway heading towards a streamlined 
    cloud solution. User generated content in real-time will have multiple 
    touchpoints for offshoring.
    Capitalize on low hanging fruit to identify a ballpark value added 
    activity to beta test. Override the digital divide with additional 
    clickthroughs from DevOps. Nanotechnology immersion along the information 
    highway will close the loop on focusing solely on the bottom line.
</Rock:ParagraphText>
```

_Note, don't worry if your paragraphs have more than one line-break between them. All extra line breaks will be ignored._

