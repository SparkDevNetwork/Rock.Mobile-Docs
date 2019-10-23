# Color

Applies to: [Button](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.button?view=xamarin-forms), [DatePicker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.datepicker?view=xamarin-forms), [Editor](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.editor?view=xamarin-forms), [Entry](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.entry?view=xamarin-forms), [Label](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout?view=xamarin-forms), [Picker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.picker?view=xamarin-forms), [SearchBar](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.searchbar?view=xamarin-forms), [TimePicker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.timepicker?view=xamarin-forms), [Span](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.span?view=xamarin-forms)

We've seen the [colors that the Downhill framework provides](../colors.md) us for use in our applications. These colors can be used with text also. Below are a listing of the various ways we can colorize the text of our applications.

## Links

You can give links a different color using the `.link` class. This is defaulted to the `Primary` Application Color. This class is applied for you when converting from HTML or Markdown.

## Application Colors

Note that the actual colors you select will be substituted for the `?color-value`.

| Class | Property |
| :--- | :--- |
| .text-primary | color: ?color-primary; |
| .text-secondary | color: ?color-secondary; |
| .text-success | color: ?color-success; |
| .text-danger | color: ?color-danger; |
| .text-warning | color: ?color-warning; |
| .text-info | color: ?color-info; |
| .text-light | color: ?color-light; |
| .text-dark | color: ?color-dark; |
| .text-white | color: ?color-white; |

## Palette Colors

Each of the palette colors can also be used for text. Use the notation of `.text-color-intensity` to specify the color.

```text
<Label StyleClass="text-gray-400" Text="Lots of trouble. Lots of bubble." />
```

