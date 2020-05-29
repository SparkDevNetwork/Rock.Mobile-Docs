# Application Info

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Somewhere in your application you normally have some sort of _About_ page that gives information about your app, such as version numbers. This view provides those version numbers in a quick way without you having to worry about styling. It simply lays out each item as a single Label inside a vertical StackLayout.

The values displayed are:

* Package Version - This displays the version number of the site package. This is the file that is built when you click the _Deploy_ button when viewing your site. This can tell you if the user is somehow using an outdated site package.
* Shell Version - The shell is what provides all the standard code and functionality across all Rock Mobile applications. This helps you know what features are supported on the user's device.
* App Store Version - This will match the version number of your application bundle as reported by the Google Play Store and Apple App Store.

**Properties**

_This view contains no properties of it's own._

