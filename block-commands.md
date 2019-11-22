# Block Commands

Xamarin, and the Rock Mobile application shell, use a concept called Commands to handle most actions and events. When you tap a button, it executes a Command. These are usually available through a `Command` property, though sometimes a view might support multiple different commands in which case the names will vary. Additionally, each Command can take a single parameter to provide additional details about how it should perform it's task.

Since all these Commands implement the same structure, this means you can use any command anywhere. So just like you can setup a button to open a browser window, you can also use that same command and tie it to a swipe gesture so if the user swipes on the screen it also opens a browser window.

Further down you will find descriptions about the various commands available, but before that you probably want to see a quick example of how to actually use commands. First, lets look at a simple button that we want to open up a browser window when the user taps on it.

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
            <Rock:OpenBrowserParameters Url="https://www.google.com/search">
                <Rock:Parameter Name="q"
                                Value="{Binding Path=Text, Source={x:Reference SearchTerm}}" />
            </Rock:Action>
        </Button.CommandParameter>
    </Button>
</StackLayout>
```

So what we did above is create an inline object in XAML and place it inside the Button's `CommandParameter` property. This object is the `<Rock:OpenBrowserParameters>...</Rock:OpenBrowserParameters>` object. It specifies what we are going to do in the command, specifically, the `Url` in this case is the base URL to go to. Contained inside is a number of parameter values that will be passed as query string parameters.

We have defined a parameter called `q` which is what Google uses for the search term. For the value, you can enter a static value if you wish, but we wanted a dynamic one. So we used XAML binding to reference the text box's value. `{Binding Source={x:Reference SearchTerm}, Path=Text}` means: Take the value from the object whose name is `SearchTerm` and use the property found at the path `Text`. Note: The reason it's called `Path` is because you can specify a property path \(e.x. `Text.Length`\) that will process a tree of properties.

So, what we have finally achieved is a Search button that uses a textbox on screen to collect the user input and then go to a search results page - and we did it without a single line of code!

It's worth being aware that most commands support a short form of their CommandParameter. You saw this above with the `OpenBrowser` command. If you don't need to pass any custom parameters and just need to specify a static URL, you can just pass the URL string into the `CommandParameter` itself. Each command will note what forms the `CommandParameter` can take.

## Parameter

While not a command itself, we will mention this here as it is used by nearly all the below commands. Sometimes you need to pass an additional parameter value to a command. This value might be static, or it could be dynamic based on another view on screen. To do this you use the `Parameter` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Name | string | The name of the parameter to be passed to the command. |
| Value | object | The value to be passed for this parameter. Supports data binding. |

**Examples**

```xml
<Rock:Parameter Name="GroupId" Value="18" />
```

```xml
<Rock:Parameter Name="GroupId" Value="{Binding Text, Source={x:Reference tbGroup}}" />
```


## OpenBrowser

This command allows you to open a web address using the built-in browser inside the application.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List\<[Parameter](#Parameter)\> | Any additional query string parameters to be included with the URL. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding OpenBrowser}"
        CommandParameter="https://www.google.com/" />
```

```xml
<Button Text="Tap"
        Command="{Binding OpenBrowser}">
    <Button.CommandParameter>
        <Rock:OpenBrowserParameters Url="https://www.google.com/">
            <Rock:Parameter Name="q" Value="rockrms" />
        </Rock:OpenBrowserParameters>
    </Button.CommandParameter>
</Button>
```


## OpenExternalBrowser

Similar to the [OpenBrowser](#OpenBrowser) command, this one opens a URL in a browser window. The difference between the two is that this command uses the devices native web browser and opens the URL in that application. This means your user leaves your mobile app and gets sent over to Safari or Chrome.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenExternalBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List\<[Parameter](#Parameter)\> | Any additional query string parameters to be included with the URL. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding OpenBrowser}"
        CommandParameter="https://www.google.com/" />
```

```xml
<Button Text="Tap"
        Command="{Binding OpenBrowser}">
    <Button.CommandParameter>
        <Rock:OpenExternalBrowserParameters Url="https://www.google.com/">
            <Rock:Parameter Name="q" Value="rockrms" />
        </Rock:OpenExternalBrowserParameters>
    </Button.CommandParameter>
</Button>
```


## PushPage

This command pushes a new page onto the navigation stack. This type of navigation allows the user to use the back button to return to the page that pushed the new page.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be pushed onto the navigation stack. |
| Parameters | List\<[Parameter](#Parameter)\> | Any additional query string parameters to be passed to the page. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding PushPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```xml
<Button Text="Tap"
        Command="{Binding PushPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```xml
<Button Text="Tap"
        Command="{Binding PushPage}">
    <Button.CommandParameter>
        <Rock:PushPageParameters PageGuid="e4d80e57-da60-4822-bc22-c071f02958e8">
            <Rock:Parameter Name="GroupId" Value="18" />
            <Rock:Parameter Name="Mode" Value="Edit" />
        </Rock:PushPageParameters />
    </Button.CommandParameter>
</Button>
```


## ReplacePage

This command replaces the current page with a new page. This differs from the [PushPage](#PushPage) command. Using the ReplacePage command, if the user then taps the back button then they will be taken back to the page they were on _before_ the page that called the ReplacePage command.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used to replace the current page. |
| Parameters | List\<[Parameter](#Parameter)\> | Any additional query string parameters to be passed to the page. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding ReplacePage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```xml
<Button Text="Tap"
        Command="{Binding ReplacePage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```xml
<Button Text="Tap"
        Command="{Binding ReplacePage}">
    <Button.CommandParameter>
        <Rock:ReplacePageParameters PageGuid="e4d80e57-da60-4822-bc22-c071f02958e8">
            <Rock:Parameter Name="GroupId" Value="18" />
            <Rock:Parameter Name="Mode" Value="Edit" />
        </Rock:ReplacePageParameters />
    </Button.CommandParameter>
</Button>
```


## ShowPage

This command replaces the entire navigation stack with a new page. This means there will be no back button to return to any previous page.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used as the new page. |
| Parameters | List\<[Parameter](#Parameter)\> | Any additional query string parameters to be passed to the page. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding ShowPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```xml
<Button Text="Tap"
        Command="{Binding Showage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```xml
<Button Text="Tap"
        Command="{Binding ShowPage}">
    <Button.CommandParameter>
        <Rock:ShowPageParameters PageGuid="e4d80e57-da60-4822-bc22-c071f02958e8">
            <Rock:Parameter Name="GroupId" Value="18" />
            <Rock:Parameter Name="Mode" Value="Edit" />
        </Rock:ShowPageParameters />
    </Button.CommandParameter>
</Button>
```



* `PlayVideo` allows you to begin playing a video in full screen. Parameter is the URL pointing to the video file.
* `PlayAudio` allows you to begin playing an audio file in full screen. Parameter is the URL pointing to the audio file.
* `ScrollToVisible` lets you cause a specific view to become visible inside it's ScrollView.
* `ShowActionPanel` gives you the ability to display an action panel (action sheet on iOS), this is a short text message with buttons the user can tap on to initiate actions.
* `ReloadApplication` will cause the application to essentially restart. Primarily useful as an administrative task for development purposes.


## ScrollToVisible

There are two ways to initiate this command, we'll show them below. But the basic syntax is you specify the Anchor element that should be scrolled until it becomes visible. Thing of this like the HTML Anchor href, or a "jump to" button.

```xml
<StackLayout>
    <Button Text="Scroll"
            Command="{Binding ScrollToVisible}"
            CommandParameter="{x:Reference myLabel}" />
    
    <BoxView Color="Red"
             HeightRequest="1200" />
    
    <Label Text="My Label"
           x:Name="myLabel" />
    
    <BoxView Color="Blue"
             HeightRequest="1200" />
```

The above will scroll the first ScrollView in the view tree above the Label we specified as our anchor so that the label is visible. This by itself may be just fine for what you need, but it may not end up doing what you want. By default the anchor will be scrolled until it becomes "just visible". In this case, since we need to scroll down, the label will end up at the bottom of the screen.

To accomodate those situations, you are able to specify a parameters object like so.

```xml
<Button Text="Scroll">
        Command="{Binding ScrollToVisible}">
    <Button.CommandParameter>
        <Rock:ScrollToVisibleParameters Anchor="{x:Reference myLabel}"
                                        Position="Start" />
    </Button.CommandParameter>
</Button>
```

This still indicates the same Label we want to use as the Anchor, but it also specifies the position we want it to end up at. Now, this is not a forced position. If there were nothing below the label, it wouldn't be possible for it to end up at the top. But since we have a lot of content below as well, there is enough room to scroll.

The options you have with the `Position` parameter are as follows.

* MakeVisible - Just make sure the anchor is visible on screen.
* Start - Attempt to scroll until the anchor is at the start (top or left) of the screen.
* Center - Attempt to scroll until the anchor is at the center of the screen.
* End - Attempt to scroll until the anchor is at the end (bottom or right) of the screen.

Finally, due to the way XAML works, there is a shorthand to the XAML we wrote above.

```xml
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}"
        CommandParameter="{Rock:ScrollToVisibleParameters Anchor={x:Reference myLabel}, Position=Start}" />
```

## ShowActionPanel

```xml
<Button Text="Action" Command="{Binding ShowActionPanel}">
    <Button.CommandParameter>
        <Rock:ActionPanelParameters Title="My Action Sheet"
                                    CancelTitle="Do Nothing">
            <Rock:ActionPanalParameters.DestructiveButton>
                <Rock:ActionPanelButton Title="Delete"
                                        Command="{Binding PushPage}"
                                        CommandParameter="c258265c-9645-46f1-a69b-0e0f149e5e83" />
            </Rock:ActionPanelParameters.DestructiveButton>
            
            <Rock:ActionPanelButton Title="First Button"
                                    Command="{Binding OpenBrowser}"
                                    CommandParameter="https://www.google.com" />
            <Rock:ActionPanelButton Title="Second Button"
                                    Command="..."
                                    CommandParameter="..." />
        </Rock:ActionPanelParameters>
    </Button.CommandParameter>
</Button>
```

The above is a bit verbose, but let's dissect it a bit. On the Button, we are setting the `CommandParameter` to a custom object, of type `ActionPanelParameters`.

| Property | Type | Description |
| :--- | :--- | :--- |
| Title | string | The title of the action panel, keep it short! |
| CancelTitle | string | The text to display in the cancel button (optional). _Defaults to empty string._ |
| DestructiveButton | ActionPanelButton | Defines the button that implies a destructive operation, for example on iOS this button becomes red (optiona). _Defaults to **null**._ |
| Buttons | ICollection\<ActionPanelButton\> | A collection of buttons to be shown, this is the default content property meaning you would just add ActionPanelButton nodes as child elements. |

So in our above example, we gave our action panel a cancel button called `Do Nothing`. It also has a destructive button called `Delete` that will cause the user to be sent to another page. Next we have our list of additional buttons we want. The first one we defined to open a browser web page. The second button we only partially defined just to give you an idea of what multiple buttons looks like.

Thanks to XAML, we can simplify the definition of the destructive button a bit if we want, it's up to you.

```xml
<Rock:ActionPanelParameters Title="My Action Sheet"
                            CancelTitle="Do Nothing"
                            DestructiveButton="{Rock:ActionPanelButton Title=Delete, Command={Binding PushPage}, CommandParameter=c258265c-9645-46f1-a69b-0e0f149e5e83}">
    
    <Rock:ActionPanelButton Title="First Button"
                            Command="{Binding OpenBrowser}"
                            CommandParameter="https://www.google.com" />
    <Rock:ActionPanelButton Title="Second Button"
                            Command="..."
                            CommandParameter="..." />
</Rock:ActionPanelParameters>
```

This simplified form only works with simple actions. For example, if your destructive button CommandParameter needed to contain an object to pass multiple query string parameters, you would probably need to revert to the more verbose definition.
