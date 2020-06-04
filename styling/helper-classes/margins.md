# Margins

Applies to: [View](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view?view=xamarin-forms)

Many elements inherit from [View ](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view?view=xamarin-forms)and therefore can have margin applied to them. Below are the utility classes for adding margins to your views.

The format for specifying margins is `.m{sides}-{size}`.

Where the sizes is one of the follow:

* `t` - top
* `b` - bottom
* `l` - left
* `r` - right
* `x` - left and right
* `y` - top and bottom
* blank for all

And the size is one of the following:

* `0`   - none
* `1`   - 0.25rem
* `2`   - 0.5rem
* `4`   - 0.75rem
* `8`   - 2rem
* `12` - 3rem
* `16` - 4rem
* `24` - 6rem
* `32` - 8 rem
* `64` - 16rem

## Examples

```text
<Label StyleClass="m-3" Text="Lots of trouble. Lots of bubble." />
```

This would provide a 10 unit margin on all sides.

```text
<Label StyleClass="mt-3" Text="Lots of trouble. Lots of bubble." />
```

This would provide a 10 unit margin on the top.

