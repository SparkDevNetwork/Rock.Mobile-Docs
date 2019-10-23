# Padding

Applies to: [Button](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/button), [ImageButton](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/imagebutton), [Layout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/), [Page](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/)

> _Note: Many elements support margins, but few support padding. You'll be tempted at first to apply padding to controls like Labels. You'll find that this does not work. Instead you'll need to wrap the Label in a Layout \(e.g. StackLayout\) and apply the padding to that._

Below are the utility classes for adding padding.

The format for specifying margins is `.p{sides}-{size}`.

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
<StackLayout StyleClass="p-3">
    <Label Text="Lots of trouble. Lots of bubble." />
</StackLayout>
```

This would provide a 10 unit padding on all sides.

```text
<StackLayout StyleClass="pt-3">
    <Label Text="Lots of trouble. Lots of bubble." />
</StackLayout>
```

This would provide a 10 unit padding on the top.

