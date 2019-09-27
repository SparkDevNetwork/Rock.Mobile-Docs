# Block Commands

Each block implements a number of standard commands to help you navigate around to various sections of the application:

* `OpenBrowser` allows you to open a web address using the built-in browser. If you include an `rckipid` parameter with no value then an impersonation token will be generated. This token is good for 30 mins.
* `OpenExternalBrowser` allows you to open a web address using the system default web browser \(for example Chrome\). If you include an `rckipid` parameter with no value then an impersonation token will be generated. This token is good for 30 mins.
* `PushPage` will push a new page onto the navigation stack, which allows the user to use the back button to return to the current page.
* `ReplacePage` will replace the current page with the new page. If the current page has a back button then the new page will also have a back button that will still go to the previous page.
* `ShowPage` will replace the entire navigation stack and show only the specified page.
* `PlayVideo` allows you to begin playing a video in full screen. Parameter is the URL pointing to the video file.
* `PlayAudio` allows you to begin playing an audio file in full screen. Parameter is the URL pointing to the audio file.
* `ReloadApplication` will cause the application to essentially restart. Primarily useful as an administrative task for development purposes.

First, lets look at a simple button that we want to open up a browser window when the user taps on it.

```xml
<StackLayout>
    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding OpenBrowser}"
            CommandParameter="https://www.google.com/search?q=rockrms" />
</StackLayout>
```

What we are doing is binding the `Command` parameter of the button to the `OpenBrowser` handler built into all blocks. Next we are passing the URL to open via the `CommandParameter`. Now, lets say we want to be able to have a textbox on screen for the user to put in a search term and then use that as the search term for Google to use.

```xml
<StackLayout>
    <Rock:FormGroup Title="Search">
        <Rock:FormField>
            <Rock:TextBox x:Name="SearchTerm" />
        </Rock:FormField>
    </Rock:FormGroup>

    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding OpenBrowser}"
            CommandParameter="https://www.google.com/search?q=rockrms" />
</StackLayout>
```

Okay, that renders a nice text box but our search button is still using the hard coded URL. To fix that we are going to change the button definition so we can create a special object as the `CommandParameter`:

```xml
<StackLayout>
    <Rock:FormGroup Title="Search">
        <Rock:FormField>
            <Rock:TextBox x:Name="SearchTerm" />
        </Rock:FormField>
    </Rock:FormGroup>

    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding OpenBrowser}">
        <Button.CommandParameter>
            <Rock:Action Url="https://www.google.com/search">
                <Rock:ActionParameter Name="q"
                                      Value="{Binding Path=Text, Source={x:Reference SearchTerm}}" />
            </Rock:Action>
        </Button.CommandParameter>
    </Button>
</StackLayout>
```

So what we did above is create an inline object in XAML and place it inside the Button's `CommandParameter` property. This object is the `<Rock:Action>...</RockAction>` object. It specifies what we are going to do in the action, specifically, the `Url` in this case is the base URL to go to. Contained inside is a number of action parameters that will be passed as query string parameters.

We have defined a parameter called `q` which is what Google uses for the search term. For the value, you can enter a static value if you wish, but we wanted a dynamic one. So we used XAML binding to reference the text box's value. `{Binding Source={x:Reference SearchTerm}, Path=Text}` means: Take the value from the object whose name is `SearchTerm` and use the property found at the path `Text`. Note: The reason it's called `Path` is because you can specify a property path \(e.x. `Text.Length`\) that will process a tree of properties.

So, what we have finally achieved is a Search button that uses a textbox on screen to collect the user input and then go to a search results page - and we did it without a single line of code!

