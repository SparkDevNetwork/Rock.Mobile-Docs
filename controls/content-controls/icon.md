# Icon

_Inherits from_ [_Xamarin.Forms.Label_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.label)

We are all accustomed to displaying nice icons on our pages. Your mobile application should be no different. This view allows you to display an icon from the FontAwesome library on your page.

Unlike on the web, you must specify the icon family that you wish to display from. So for example, you cannot just set `IconClass="car"` and have it work, because the Car icon is not availabe in the Regular font \(which is the default icon font family\). So you would need to display with `IconClass="car" IconFamily="Solid"`.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| IconClass | string | The icon class name to use. This can be found on the FontAwesome website. If it specified `fas fa-car` then you would use `car`. |
| IconFamily | [IconFamily](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#IconFamily) | The icon font family to use to display the icon. _Default value is **FontAwesomeRegular**._ |
| FontSize | double | The size of the icon. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color of the icon to use by default. |
| Command | ICommand | The command to be executed when the user taps on the icon. |
| CommandParameter | object | The object to be passed as the parameter to `Command`. |

**Example**

```text
<StackLayout Orientation="Horizontal">
    <Rock:Icon IconClass="car" IconFamily="FontAwesomeSolid" FontSize="32" />
    <Rock:Icon IconClass="rockrms" IconFamily="FontAwesomeBrands" TextColor="#ee7725" FontSize="32" />
    <Rock:Icon IconClass="user" FontSize="32" />
</StackLayout>
```

![](../../.gitbook/assets/icon-1.png)

## Supported Font Families

Rock Mobile currently supports the font families listed below.

| Font Family | Description |
| :--- | :--- |
| FontAwesomeSolid | The solid set of icons from the FontAwesome free edition version 5.11. For a listing of icons see: [https://fontawesome.com/](https://fontawesome.com/) |
| FontAwesomeRegular | The regular set of icons from the FontAwesome free edition version 5.11. For a listing of icons see: [https://fontawesome.com/](https://fontawesome.com/) |
| FontAwesomeBrands | The brand set of icons from the FontAwesome free editionversion 5.11. For a listing of icons see: [https://fontawesome.com/](https://fontawesome.com/) |
| MaterialDesignIcons | The Material Design icons documented here: [https://material.io/resources/icons/](https://material.io/resources/icons/) |

