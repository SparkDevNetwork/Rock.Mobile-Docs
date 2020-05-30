# Markdown

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Another way that information is often styled in Rock is with something called [Markdown](https://www.markdownguide.org/cheat-sheet). This syntax allows the user to _indicate_ that they want things formatted in a certain fashion, but it does not give them the ability to specify exactly how that formatting is done. For example, they can specify that they want a heading, but they don't get to pick exactly how that heading is formatted.

Not everything in the Markdown syntax is supported, for example tables and footnotes are not supported. But most of your basic syntax will be supported. As with the [Html](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#Html) view, you will probably want to wrap your content in a `CDATA` tag.

When converting the markdown to XAML the Markdown control will add StyleClasses for you. Headings will get `.heading1-.heading6`, paragraphs will be assigned the `.text` class and links will be assigned the `.link` CSS class.

_Note:_ Links are only clickable at a leaf block level \(Xamarin.Forms formatted strings doesn't support span user interactions\) : if a leaf block contains more than one link, the user is prompted. This is almost a feature since text may be too small to be precise enough! ;\)

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The markdown text to be displayed. |
| Lava | bool | If **true** then the Text will be processed for any Lava before final rendering happens. _Defaults to **false**._ |
| FontFamily | string | The font family to use when rendering the markdown content. |
| FontSize | double | The size of the font to use as the base-size when rendering. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The text color to use for plain text when rendering the content. _Defaults to **Black**._ |
| HeadingTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering markdown headings. |
| CodeTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering inline code snippets and full code blocks. |
| CodeBackgroundColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `BackgroundColor` when rendering inline code snippets and full code blocks. |
| QuoteTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering quoted text. |
| QuoteBackgroundColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | Sets the background color used when rendering quoted text. _Defaults to **Gray at 20% alpha**._ |
| BarColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when any lines and bars need to be rendered. |

**Example**

```text
<Rock:Markdown HeadingTextColor="#3bab3b">
    <![CDATA[
# Heading

This is some **bold** text.
    ]]>
</Rock:Markdown>
```

![](../../../.gitbook/assets/markdown-1.png)

