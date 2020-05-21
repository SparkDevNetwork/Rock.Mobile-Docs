# Redirect

_Inherits from_ [_Xamarin.Forms.View_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.view)

While technically a View, this control does not actually render any content. Instead, it is used as a way for you to redirect the user to another page. This works similarly to the `ReplacePage` command. The primary difference is that the `Redirect` view does not require user interaction. This allows you to simply put the `Redirect` view in your XAML and the user is automatically redirected to the target page before the current page content is ever displayed.

If the `Redirect` view is encountered before the page is fully rendered then the redirect happens after the page has finished initializing but before the page is displayed. On the other hand, if it is encountered after the page is fully rendered then the redirect happens immediately.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The page identifier that the user will be redirected to. |
| QueryString | string | An optional set of query string parameters that will be passed to the page. _Note: This value must be XML escaped for the XAML to be parsed properly._ |

**Example**

```text
<Rock:Redirect PageGuid="8beefd7c-9939-4892-9cb2-243e58fca457"
               QueryString="ItemType=Person&amp;ItemId=4823" />
```

### 

