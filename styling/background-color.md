# Background Color

Applies to: [VisualElement](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.visualelement?view=xamarin-forms)

We've seen the [colors that the Downhill framework provides](colors.md) us for use in our applications. These colors can be used to specify background colors. 

## Application Colors

Note that the actual colors you select will be substituted for the `?color-value`.

| Class | Property |
| :--- | :--- |
| .bg-primary | color: ?color-primary; |
| .bg-secondary | color: ?color-secondary; |
| .bg-success | color: ?color-success; |
| .bg-danger | color: ?color-danger; |
| .bg-warning | color: ?color-warning; |
| .bg-info | color: ?color-info; |
| .bg-light | color: ?color-light; |
| .bg-dark | color: ?color-dark; |
| .bg-white | color: ?color-white; |

## Palette Colors

Each of the palette colors can also be used for text. Use the notation of `.bg-color-intensity` to specify the color. 

```text
<Label StyleClass="bg-gray-400" Text="Lots of trouble. Lots of bubble." />
```

