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

We have defined a parameter called `q` which is what Google uses for the search term. For the value, you can enter a static value if you wish, but we wanted a dynamic one. So we used XAML binding to reference the text box's value. `{Binding Source={x:Reference SearchTerm}, Path=Text}` means: Take the value from the object whose name is `SearchTerm` and use the property found at the path `Text`. Note: The reason it's called `Path` is because you can specify a property path (e.x. `Text.Length`\) that will process a tree of properties.

So, what we have finally achieved is a Search button that uses a textbox on screen to collect the user input and then go to a search results page - and we did it without a single line of code!

It's worth being aware that most commands support a short form of their CommandParameter. You saw this above with the `OpenBrowser` command. If you don't need to pass any custom parameters and just need to specify a static URL, you can just pass the URL string into the `CommandParameter` itself. Each command will note what forms the `CommandParameter` can take.

## Shorthand

All of the command parameter objects discussed in this section support being used as a XAML extension. Said another way, there is a shorthand to creating them. This shorthand does not work if you need to pass an array of items into the parameters (such as a bunch of query string parameters), but it does work for other use cases. Let's compare a "standard" usage as follows.

```xml
<Button Text="Scroll">
        Command="{Binding ScrollToVisible}">
    <Button.CommandParameter>
        <Rock:ScrollToVisibleParameters Anchor="{x:Reference myLabel}"
                                        Position="Start" />
    </Button.CommandParameter>
</Button>
```

This is a rather verbose way to do it, but it allows us to pass in a whole POCO class of `ScrollToVisibleParameters` that then specifies the anchor element to be made visible and where to place it after scrolling. This can be shortened using XAML extensions to the following.

```xml
<Button Text="Scroll">
        Command="{Binding ScrollToVisible}"
        CommandParameter="{Rock:ScrollToVisibleParameters Anchor={x:Reference myLabel}, Position=Start}" />
```

As you can see, this is much more succinct. Though again, it does not work when you need to supply an array of elements as a property. Do notice that the syntax changes ever so slightly for setting properties inside the shorthand syntax. First, the properties are separated by a comma. Second, the property values are not enclosed in quotation marks (since we are already inside a quotation for the outer property). This does mean if you are trying to set a string value inside this shorthand that you cannot have the string contain a comma since it will be interpreted as a property separator.


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


## Commands

Most of the command below will work in the context of a block's content (the one exception is the [Callback](#callback) command, that only works inside a block that derives from the Content block). Many of them will work outside the context of a block as well. For example, in the flyout menu. However, some do require knowing what page you are on and thus will not work in that context. One example of this is the [ShowActionPanel](#showactionpanel) command. It needs to know what page to display the action panel over and if used from the flyout menu that context is not available to it.

### OpenBrowser

This command allows you to open a web address using the built-in browser inside the application.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List<[Parameter](#Parameter)> | Any additional query string parameters to be included with the URL. |

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


### OpenExternalBrowser

Similar to the [OpenBrowser](#OpenBrowser) command, this one opens a URL in a browser window. The difference between the two is that this command uses the devices native web browser and opens the URL in that application. This means your user leaves your mobile app and gets sent over to Safari or Chrome.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenExternalBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List<[Parameter](#Parameter)> | Any additional query string parameters to be included with the URL. |

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


### PushPage

This command pushes a new page onto the navigation stack. This type of navigation allows the user to use the back button to return to the page that pushed the new page.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be pushed onto the navigation stack. |
| Parameters | List<[Parameter](#Parameter)> | Any additional query string parameters to be passed to the page. |

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


### ReplacePage

This command replaces the current page with a new page. This differs from the [PushPage](#PushPage) command. Using the ReplacePage command, if the user then taps the back button then they will be taken back to the page they were on _before_ the page that called the ReplacePage command.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used to replace the current page. |
| Parameters | List<[Parameter](#Parameter)> | Any additional query string parameters to be passed to the page. |

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


### ShowPage

This command replaces the entire navigation stack with a new page. This means there will be no back button to return to any previous page.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them (for example, your `&` must become `&amp;`).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used as the new page. |
| Parameters | List<[Parameter](#Parameter)> | Any additional query string parameters to be passed to the page. |

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


### PopPage

This command performs the reverse action of the [PushPage](#PushPage) command. In simple terms, this behaves just like the user tapped the back button in the navigation toolbar at the top. The current page is popped and removed from the stack and the previous page becomes visible. Think in terms of pushing a page that asks the user for confirmation on something and you want to have a Cancel button that goes back to the previous page.

One additional feature is that you can request that the previous page in the stack should be reloaded before it's displayed. This is accomplished by passing `true` to the `CommandParameter`.

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding PopPage}" />
```

```xml
<Button Text="Tap"
        Command="{Binding PopPage}"
        CommandParameter="true" />
```


### PlayVideo

This command initiates the playing of a video in full-screen. Usually it is better to use the `VideoPlayer` view instead, but there are times you just want to play a video when the user taps a button.

The `CommandParameter` consists of a string which contains the URL of the video to be played.

**Example**

```xml
<Button Text="Tap"
        Command="{Binding PlayVideo}"
        CommandParameter="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4" />
```


### PlayAudio

Like the [PlayVideo](#PlayVideo) command, this initiates a full-screen playback of an audio file. Given that it's audio, there won't be much to see.

The `CommandParameter` consists of a string which contains the URL of the audio file to be played.

**Example**

```xml
<Button Text="Tap"
        Command="{Binding PlayVideo}"
        CommandParameter="http://www.noiseaddicts.com/samples_1w72b820/2541.mp3" />
```


### ReloadApplication

Don't use this. No seriously, you can use it but it has a very special purpose. Basically, this instructs the application to reload as if you had just force quit and started it again. This is a development tool that saves us a few seconds when we are debugging stuff, though you may find it useful when debugging your own blocks.

The `CommandParameter` is not used and will be ignored.

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding ReloadApplication}" />
```


### ScrollToVisible

There are two ways to initiate this command, we'll show them below. But the basic syntax is you specify the Anchor element that should be scrolled until it becomes visible. Think of this like the HTML Anchor href, or a "jump to" button.

You can either pass the view to be made visible directly by reference in the `CommandParameter` or you can pass an instance of the `ScrollToVisibleParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Anchor | VisualElement | The view which resides inside a ScrollView that should be made visible. |
| Position | ScrollToPosition | The position to put the view at after scrolling (see below). _Defaults to **MakeVisible**._ |

The options you have with the `Position` parameter are as follows.

* MakeVisible - Just make sure the anchor is visible on screen.
* Start - Attempt to scroll until the anchor is at the start (top or left) of the screen.
* Center - Attempt to scroll until the anchor is at the center of the screen.
* End - Attempt to scroll until the anchor is at the end (bottom or right) of the screen.

**Examples**

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

Finally, due to the way XAML works, there is a shorthand to the XAML we wrote above.

```xml
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}"
        CommandParameter="{Rock:ScrollToVisibleParameters Anchor={x:Reference myLabel}, Position=Start}" />
```


### ShowActionPanel

This action will show an action panel (think action sheet in iOS terms). This is basically a popup that contains a short message and a number of buttons the user can choose from. A common example of this would be a "reply" button in a mail application. When tapping the button it might then popup an action sheet which contains a few buttons to help you decide what you intend to do: Reply, Reply All, Forward.

These popups usually have a Cancel button (though it's not strictly required. Additionally, you can specify a single "destructive" button that stands out to the user. Often this would be a Delete type action and is usually styled red.

The `CommandParameter` must specify an instance of the `ShowActionPanelParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Title | string | The title of the action panel, keep it short! |
| CancelTitle | string | The text to display in the cancel button (optional). _Defaults to empty string._ |
| DestructiveButton | ActionPanelButton | Defines the button that implies a destructive operation, for example on iOS this button becomes red (optiona). _Defaults to **null**._ |
| Buttons | ICollection&lt;ActionPanelButton&gt; | A collection of buttons to be shown, this is the default content property meaning you would just add ActionPanelButton nodes as child elements. |

**Examples**

```xml
<Button Text="Action" Command="{Binding ShowActionPanel}">
    <Button.CommandParameter>
        <Rock:ShowActionPanelParameters Title="My Action Sheet"
                                        CancelTitle="Do Nothing">
            <Rock:ShowActionPanalParameters.DestructiveButton>
                <Rock:ActionPanelButton Title="Delete"
                                        Command="{Binding PushPage}"
                                        CommandParameter="c258265c-9645-46f1-a69b-0e0f149e5e83" />
            </Rock:ShowActionPanelParameters.DestructiveButton>
            <Rock:ActionPanelButton Title="First Button"
                                    Command="{Binding OpenBrowser}"
                                    CommandParameter="https://www.google.com" />
            <Rock:ActionPanelButton Title="Second Button"
                                    Command="{Binding OpenExternalBrowser}"
                                    CommandParameter="https://www.google.com/" />
        </Rock:ShowActionPanelParameters>
    </Button.CommandParameter>
</Button>
```

Thanks to the magic of XAML, we can simplify the definition of the destructive button a bit if we want, it's up to you.

```xml
<Button Text="Action" Command="{Binding ShowActionPanel}">
    <Button.CommandParameter>
        <Rock:ShowActionPanelParameters Title="My Action Sheet"
                                        CancelTitle="Do Nothing"
                                        DestructiveButton="{Rock:ActionPanelButton Title=Delete, Command={Binding PushPage}, CommandParameter=c258265c-9645-46f1-a69b-0e0f149e5e83}">
            <Rock:ActionPanelButton Title="First Button"
                                    Command="{Binding OpenBrowser}"
                                    CommandParameter="https://www.google.com" />
            <Rock:ActionPanelButton Title="Second Button"
                                    Command="{Binding OpenExternalBrowser}"
                                    CommandParameter="https://www.google.com/" />
        </Rock:ShowActionPanelParameters>
    </Button.CommandParameter>
</Button>
```


### PageEvent

You learned, or will learn, elsewhere that you can use Lava on the mobile shell to handle certain page events and respond to them. Normally these page events are just ones generated by the system for you. However, you can trigger your own custom page events using this command.

The `CommandParameter` can either be a plain string that indicates the event name to be triggered, or it can be an instance of the `PageEventParameters` object. This latter method allows you to pass parameters into your lava logic.

| Property | Type | Description |
| :--- | :--- | :--- |
| Event | string | The name of the event to be triggered. |
| Parameters | List<[Parameter](#Parameter)> | Any parameters that will be passed to the Lava engine, these manifest as lava variables. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding PageEvent}"
        CommandParameter="UserTap" />
```

```xml
<Button Text="Tap"
        Command="{Binding PageEvent}">
    <Button.CommandParameter>
        <Rock:PageEventParameters Event="UserTap">
            <Rock:Parameter Name="GroupId" Value="18" />
        </Rock:PageEventParameters>
    </Button.CommandParameter>
</Button>
```


### Callback

Some blocks, currently just the `Content` block, support what is called Callbacks. You can learn more about these in the `Advanced: Dynamic Content` and the `Developer` chapters. But, for our purposes here, you can think of these as an API call back to the server's logic for the block.

If the `CommandParameter` is a plain string, then it is used as the name of the callback function to be queried on the server. If you need to pass any parameters to that function then `CommandParameter` should point to an instance of the `CallbackParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Name | string | The name of the callback to be called on the server. |
| Parameters | List<[Parameter](#Parameter)> | Any parameters that will be passed to the callback function. |

**Examples**

```xml
<Button Text="Tap"
        Command="{Binding Callback}"
        CommandParameter="UserTap" />
```

```xml
<Button Text="Tap"
        Command="{Binding Callback}">
    <Button.CommandParameter>
        <Rock:CallbackParameters>
            <Rock:Parameter Name="GroupId" Value="18" />
        </Rock:CallbackParameters>
    </Button.CommandParameter>
</Button>
```
