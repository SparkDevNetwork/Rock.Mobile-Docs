# Performance

As you get comfortable with XAML you'll start to see the power of what you can achieve. With this power comes the responsibility to understand the performance implications of your actions. Each view \(Grids, StackLayouts, Labels, etc.\) adds overhead to the page as it is rendered. You'll want to make sure you limit how many views you add.

## Layout performance optimization <a id="dd06"></a>

The most important aspect of layout-level optimization is knowing when you should be using which layout. As a XAML developer you should be aware of how each of these layouts work and what the drawbacks are of using each of them.

### General: <a id="a53a"></a>

* A layout that’s capable of displaying multiple children, but that only has a single child, is wasteful. For example, a `StackLayout` __with a single child does not make sense.
* In addition, don’t attempt to reproduce the appearance of a specific layout by using a combination of other layouts, as this results in unnecessary layout calculations being performed, and at the end of the day it doesn’t make much sense. For example, don’t attempt to reproduce a `Grid` __layout by using a combination of `StackLayouts`_._
* Don’t set the [`VerticalOptions`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view.verticaloptions#Xamarin_Forms_View_VerticalOptions) and [`HorizontalOptions`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view.verticaloptions#Xamarin_Forms_View_VerticalOptions) of a layout unless required. The default values for it, i.e.[`LayoutOptions.Fill`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layoutoptions.fill) and [`LayoutOptions.FillAndExpand`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layoutoptions.fillandexpand), allow for the best layout optimization.
* Changing these properties has a cost and consumes memory, even when setting them to the default values. Hence, if your requirement is fulfilled using `Fill/FillAndExpand`, not mentioning it during the views creation is the best practice.
* Avoid using a `RelativeLayout` ****if at all possible. It has significant overhead and is never recommended.
* When using an `AbsoluteLayout`, avoid using the `AbsoluteLayout.Autosize` __property whenever possible.
* Pack your views in the constructor rather than `OnAppearing`.
* Bypass transparency—if you can achieve the same \(or close enough\) effect with full opacity, do so.

### Grid: <a id="fcd1"></a>

* Reduce the depth of layout hierarchies by specifying [`Margin`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view.margin#Xamarin_Forms_View_Margin) property values, allowing the creation of layouts with fewer wrapping views. For more information, check out the [Margins and Padding documentation](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/margin-and-padding).
* When using a `Grid`, try to ensure that there as few rows and columns as possible that are set to `Auto`, which makes the engine perform additional calculations. Instead, use fixed-size rows and columns if possible.
* Set rows and columns to occupy a proportional amount of space with the `GridUnitType.Star`.

### StackLayout: <a id="8f48"></a>

* When using a [`StackLayout`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.stacklayout), ensure that only one child is set to [`LayoutOptions.Expands`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layoutoptions.expands#Xamarin_Forms_LayoutOptions_Expands). This property ensures that the specified child will occupy the largest space that the `StackLayout` can give to it. **Note:** It is wasteful to perform these calculations more than once.
* Don’t call any of the methods of the [`Layout`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout) class, as they result in the performance of expensive layout calculations. Instead, it's likely that the desired layout can be obtained by setting the [`TranslationX`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.visualelement.translationx#Xamarin_Forms_VisualElement_TranslationX) and [`TranslationY`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.visualelement.translationy#Xamarin_Forms_VisualElement_TranslationY) properties. Alternatively, you can subclass the [`Layout<View>`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout-1) class to achieve the desired layout behavior.
* Avoid calling `Layout()` \(and especially `ForceLayout()`\).

### Label: <a id="1715"></a>

* Update `Label` only when required, as changing the size of a label can result in the entire screen layout being recalculated.
* Don’t set a Label’s `VerticalTextAlignment`**/**`HorizontalTextAlignment` property unless required.
* Set the [`LineBreakMode`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.label.linebreakmode#Xamarin_Forms_Label_LineBreakMode) of any label to [`NoWrap`](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.linebreakmode#Xamarin_Forms_LineBreakMode_NoWrap) whenever viable.

### ListView <a id="57f8"></a>

* Including a `ListView` __inside a `ScrollView` __is a very bad practice. Always avoid it. Use the `ListView`_'s_ `Header` and `Footer` properties instead.
* Do not use `TableView` __where you can use a `ListView`. `TableView`s __are usually recommended for a setting like UI.
* Use `ListViewCachingStrategy.RecycleElement` when you can. This is _not_ the default caching strategy.
* Use `DataTemplate` selectors to facilitate heterogeneous views within a single `ListView`. Don’t override `OnBindingContextChanged` to update and achieve the same effect.
* Avoid passing `IEnumerable<T>` as a data source to `ListView`s. Instead, try to use `IList<T>`, because `IEnumerable<T>` collections don't support random access.
* Nesting `ListView`s is a bad practice. Instead, use groups within a single `ListView`. Nesting is explicitly unsupported and will break your application.
* **DO** use `HasUnevenRows` where your `ListView` has rows of differing sizes. If the content of the cell is modified dynamically \(perhaps after loading it from the database\), be sure to call `ForceUpdateSize()` on the cell.
* Avoid deeply nested layout hierarchies. Use `AbsoluteLayout` or `Grid` to help reduce nesting.
* Avoid specific `LayoutOptions` other than `Fill` \(`Fill` is the cheapest to compute\).

### Images <a id="4342"></a>

* Images on Android do not down-sample. Always remember this as it’s one of the reasons your app reaches OOM.
* Set `Image.IsOpaque` to `true` if possible.
* Load images from _Content_ instead of _Resources_.

### CarouselPage <a id="84b0"></a>

* Avoid ****using the `CarouselPage` __as much as possible; instead use a `CarouselView` within a `ContentPage`. The reason for this is that `CarouselPage` loads all the data at once, which hampers performance at extreme levels.

List above comes from [Techniques for Improving Performance in a Xamarin.Forms Application by Gulam Ali Hakim](https://heartbeat.fritz.ai/techniques-for-improving-performance-in-a-xamarin-forms-application-b439f2f04156).

Additional Resources:

* [Jason Smith's Xamarin Forms Performance Tips](https://kent-boogaart.com/blog/jason-smith's-xamarin-forms-performance-tips)

