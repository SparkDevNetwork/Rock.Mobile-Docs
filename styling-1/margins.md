# Margins

Applies to: [View](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view?view=xamarin-forms)

Many elements inherit from [View ](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view?view=xamarin-forms)and therefore can have margin applied to them. Below are the utility classes for adding margins to your views.

The format for specifying margins is `.m{sides}-{size}`. 

Where the sizes is one of the follow:

* `t` - for top
* `b` for bottom
* `l` for left
* `r` for right
* `x` for left and right
* `y` for top and bottom
* blank for all

And the size is one of the following:

* `0` for none
* `1` for 2.5
* `2` for 5
* `3` for 10
* `4` for 20
* `5` for 30

## Examples

```text
<Label StyleClass="m-3" Text="Lots of trouble. Lots of bubble." />
```

This would provide a 10 unit margin on all sides.

```text
<Label StyleClass="mt-3" Text="Lots of trouble. Lots of bubble." />
```

This would provide a 10 unit margin on the top.

