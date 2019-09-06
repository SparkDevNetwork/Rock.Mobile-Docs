# Text Size

Applies to: [Button](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.button?view=xamarin-forms), [DatePicker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.datepicker?view=xamarin-forms), [Editor](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.editor?view=xamarin-forms), [Entry](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.entry?view=xamarin-forms), [Label](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout?view=xamarin-forms), [Picker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.picker?view=xamarin-forms), [SearchBar](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.searchbar?view=xamarin-forms), [TimePicker](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.timepicker?view=xamarin-forms), [Span](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.span?view=xamarin-forms)

Several utility classes have been created to help you size your text. Apple and Google both have styling standards for various types of text in your application. We've provided a set of classes for their UI patterns as well as a general set if you prefer to go your own way.

## Named Sizes

The following classes allow you to size your text the way that Apple and Google intended.

| Class | iOS | Android |
| :--- | :--- | :--- |
| .text-default | 16 | 14 |
| .text-micro | 11 | 10 |
| text-small | 13 | 14 |
| .text-medium | 16 | 17 |
| .text-large | 20 | 22 |
| .text-body | 17 | 16 |
| .text-header | 17 | 16 |
| .text-title | 28 | 24 |
| .text-subtitle | 22 | 16 |
| .text-caption | 12 | 12 |

## Utility Sizes

If you'd prefer to have additional options to size your text, we've provided the following classes to help.

| Class | Size |
| :--- | :--- |
| .text-xs | 10.5 |
| .text-sm | 22.25 |
| .text-base | 14 |
| .text-lg | 15.75 |
| .text-xl | 17.5 |
| .text-2xl | 21 |
| .text-3xl | 26.25 |
| .text-4xl | 31.5 |
| .text-5xl | 42 |
| .text-6xl | 56 |

The sizes above are based off of a default starting point \(14 which determines `.text-base`\). The rest of the sizes are built off of those using ratios.

## Heading Sizes

Not to add yet another sizing pattern, but we've added utility classes for headings. These are helpful when using content that is being converted from HTML or Markdown. You can easily override these sizes in your own CSS.

| Class | Property |
| :--- | :--- |
| .heading1 | 27 |
| .heading2 | 20 |
| .heading3 | 18 |
| .heading4 | 16 |
| .heading5 | 14 |
| .heading6 | 14 |

