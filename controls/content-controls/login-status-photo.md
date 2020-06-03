# Login Status Photo

## LoginStatusPhoto

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

The [LoginStatus](../#LoginStatus) view is the one you would normally use in the Flyout shell. But what if you want to just present a small icon that indicates to the user if they are logged in or not? The LoginStatusPhoto view does just that. In fact, [LoginStatus](../#LoginStatus) uses this view itself to present the user's photo.

A number of properties allow you to specify how the profile photo will be displayed, and what photos to use if no profile picture is available or if the user is not logged in. Another set of properties allow you to specify the commands to be executed when the user interacts with the profile picture.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| NotLoggedInPhotoSource | string | The image that is displayed if no user is logged in. _Defaults to **resource://Rock.Mobile.Resources.icon-person-not-logged-in.svg**._ |
| NotLoggedInPhotoFillColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | Provides a way to quickly change the color of the entire image when drawing the "not logged in" image. |
| LoggedInNoPhotoSource | string | The no profile icon that is displayed if a user is logged in but has no profile photo. _Defaults to **resource://Rock.Mobile.Resources.icon-person-no-photo.png**._ |
| LoggedInNoPhotoFillColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | Provides a way to quickly change the color of the entire image when drawing the "logged in but no profile photo" image. |
| ProfilePhotoStrokeColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when drawing the border around the profile image. |
| ProfilePhotoStrokeWidth | double | The width of the profile image border to be displayed. _Defaults to **0**._ |
| ProfilePhotoCircle | boolean | If _true_, then the logged in profile photo image will be drawn with the Circle transformation applied. _Defaults to **false**_. |
| NotLoggedInCommand | ICommand | The command to be executed when the user interacts with the photo when they are not logged in. |
| NotLoggedInCommandParameter | object | The parameter to be passed to the `NotLoggedInCommand`. |
| LoggedInCommand | ICommand | The command to be executed when the user interacts with the photo when they are logged in. |
| LoggedInCommandParameter | object | The parameter to be passed to the `LoggedInCommand`. |

**Example**

```text
<Rock:LoginStatusPhoto />
```

