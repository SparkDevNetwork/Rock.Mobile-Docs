# XAML Styling

Below are various styling rules we recommend when writing XAML.

1. When using a StyleClass place it as the first parameter when defining Labels, or when the CSS class name helps define the intent of the control.   Good: `<Label StyleClass="h1" Text="My Title" />` Bad: `<Label Text="My Title" StyleClass="h1" />` 
2. When using a control \(like a Label\) that does not need a body do not provide an end tag.  Good: `<Label Text="My Label" />` Bad: `<Label Text="My Label"></Label>` 

## Laying Out Your Application

### Horizontal Alignments

When you need to align controls horizontally you have quite a few options. We recommend using the following guidelines when selecting when selecting a strategy.

1. [StackLayout ](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/stacklayout)- StackLayouts are a performant away to align controls horizontally \(Orientation="Horizontal"\). StackLayouts don't offer a lot in the way of placement options. _Web Equivalent: DIVs with floats_ 
2.  ResponsiveLayout - ResponsiveLayouts are a Rock Mobile addition that create column grids much like Bootstrap's grid in CSS. It allows you to provide a different number and size of columns based on the size of the view. _Web Equivalent: Bootstrap Grids_ 
3. [Grid ](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/grid)- The Xamarin Forms Grid is a very powerful layout tool. It can slow down performance a bit if they become too numerous or complex.    _Web Equivalent: HTML Tables \(on steroids\)_ 
4. Others \([AbsoluteLayout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/absolute-layout)/[RelativeLayout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/relative-layout)/[FlexLayout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/flex-layout)\) - There are numerous other options to look at. Their uses are more advanced. 

