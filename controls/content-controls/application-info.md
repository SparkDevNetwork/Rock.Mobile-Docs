# Application Info

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Displays various bits of information regarding the running application. This is the type of stuff you might put on your About page. When you are trying to diagnose the problem that a user might be having, knowing this information can help determine if it is a known issue and if an update will fix it.

The information displayed is:

* Package Version - This is the version of the site package.
* Shell Version - The shell version is the version of the DLL that provides the core mobile app code.
* App Store Version - This is the version of _your_ application bundle that will match the version specified in the app store.

**Properties**

_This view has no properties of it's own._

**Example**

```text
<Rock:ApplicationInfo />
```
