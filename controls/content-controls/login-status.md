# Login Status

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Okay let's face it. You aren't going to be using this view on any old page. It's a one-off. But you are going to need to know how to customize it for the flyout.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| WelcomeMessage | string `Lava` | The welcome message that is displayed when a user is logged in. _Defaults to **Hello !**_ |
| LoginMessage | string `Lava` | The login message that is displayed if a user is not logged in. _Defaults to **Login**._ |
| EditProfileLabel | string `Lava` | The edit profile subtext that is displayed if a user is logged in. _Defaults to **Edit Profile**._ |
| LoginLabel | string `Lava` | The login subtext that is displayed if a user is not logged in. _Defaults to **Tap to Personalize**._ |
| NoProfileIcon | string | The no profile icon that is displayed if a user is logged in but has no profile photo. _Defaults to **resource://Rock.Mobile.Resources.profile-no-photo.png**._ |
| NotLoggedInIcon | string | The image that is displayed if no user is logged in. _Defaults to **resource://Rock.Mobile.Resources.person-no-photo-unknown.svg**._ |
| ProfilePageGuid | Guid | The profile page unique identifier that the user will be directed to when they tap on the Edit Profile text. |
| ImageSize | double | The width and height of the profile image to be displayed. _Defaults to **80**._ |
| ImageBorderSize | double | The width of the profile image border to be displayed. _Defaults to **0**._ |
| ImageBorderColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when drawing the border around the profile image. _Defaults to **White**._ |
| TitleFont | [Font](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.font) | The font to use when rendering the Title text. |
| TitleFontSize | double | The size of the font to use when rendering the Title text. |
| TitleTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when rendering the Title text. |
| SubTitleFont | [Font](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.font) | The font to use when rendering the SubTitle text. |
| SubTitleFontSize | double | The size of the font to use when rendering the SubTitle text. |
| SubTitleTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when rendering the SubTitle text. |

**Example**

```text
<Rock:LoginStatus />
```

![](../../.gitbook/assets/loginstatus-1.png)

