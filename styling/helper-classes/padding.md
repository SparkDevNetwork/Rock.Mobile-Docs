# Padding

Applies to: [Button](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/button), [ImageButton](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/imagebutton), [Layout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/), [Page](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/)

> _Note: Many elements support margins, but few support padding. You'll be tempted at first to apply padding to controls like Labels. You'll find that this does not work. Instead you'll need to wrap the Label in a Layout \(e.g. StackLayout\) and apply the padding to that._

Below are the utility classes for adding padding.

The format for specifying margins is `.p{sides}-{size}`.

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

