# HTML

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)\_\_

This view allows you to render HTML into the page. HTML rendering is limited on mobile applications and not as performant as it could be. Therefore it is recommended you limit your HTML usage. You don't need to avoid it, just try not to render a few dozen paragraphs of text and expect it to be snappy.

A limited subset of HTML tags is supported. Any non-supported tags will be rendered as their plain text contents \(meaning, the HTML tags are stripped\).

* `H1` - `H6`
* `DIV`
* `P`
* `SPAN`
* `STRONG`
* `B`
* `EM`
* `I`
* `CODE`
* `PRE`
* `OL`
* `UL`
* `LI`

In addition, in-line styles are supported, but again only a few styles are supported:

* `color`
* `background-color`

It should be noted that because you are defining your HTML content in an XML file you cannot simply do something like `Text="{{ Item.Html }}"` and expect it to work. You will need to escape the HTML text, or you can use the `CDATA` trick as shown in the example.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The text content that contains the HTML. |
| Lava | bool | If **true** then the Text will be processed for any Lava before final rendering happens. _Defaults to **false**._ |
| FontFamily | string | The font family to use by default. |
| FontSize | double | The size of the font to use by default. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color of the text to use by default. _Default value is **Black**._ |

**Example**

```text
<Rock:Html TextColor="Red">
    <![CDATA[
    <p>This is some <span style='color: blue'>sample</span> text.</p>
    <p>This is a second paragraph.</p>
    ]]>
</Rock:Html>
```

![](../../.gitbook/assets/html-1.png)

