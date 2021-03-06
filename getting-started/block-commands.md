# Block Commands

Xamarin, and the Rock Mobile application shell, use a concept called Commands to handle most actions and events. When you tap a button, it executes a Command. These are usually available through a `Command` property, though sometimes a view might support multiple different commands in which case the names will vary. Additionally, each Command can take a single parameter to provide additional details about how it should perform it's task.

Since all these Commands implement the same structure, this means you can use any command anywhere. So just like you can setup a button to open a browser window, you can also use that same command and tie it to a swipe gesture so if the user swipes on the screen it also opens a browser window.

Further down you will find descriptions about the various commands available, but before that you probably want to see a quick example of how to actually use commands. First, lets look at a simple button that we want to open up a browser window when the user taps on it.

```markup
<StackLayout>
    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding OpenBrowser}"
            CommandParameter="https://www.google.com/search?q=rockrms" />
</StackLayout>
```

What we are doing is binding the `Command` parameter of the button to the `OpenBrowser` handler built into all blocks. Next we are passing the URL to open via the `CommandParameter`. Now, lets say we want to be able to have a textbox on screen for the user to put in a search term and then use that as the search term for Google to use.

```markup
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

```markup
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

## Shorthand

All of the command parameter objects discussed in this section support being used as a XAML extension. Said another way, there is a shorthand to creating them. This shorthand does not work if you need to pass an array of items into the parameters \(such as a bunch of query string parameters\), but it does work for other use cases. Let's compare a "standard" usage as follows.

```markup
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}">
    <Button.CommandParameter>
        <Rock:ScrollToVisibleParameters Anchor="{x:Reference myLabel}"
                                        Position="Start" />
    </Button.CommandParameter>
</Button>
```

This is a rather verbose way to do it, but it allows us to pass in a whole POCO class of `ScrollToVisibleParameters` that then specifies the anchor element to be made visible and where to place it after scrolling. This can be shortened using XAML extensions to the following.

```markup
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}"
        CommandParameter="{Rock:ScrollToVisibleParameters Anchor={x:Reference myLabel}, Position=Start}" />
```

As you can see, this is much more succinct. Though again, it does not work when you need to supply an array of elements as a property. Do notice that the syntax changes ever so slightly for setting properties inside the shorthand syntax. First, the properties are separated by a comma. Second, the property values are not enclosed in quotation marks \(since we are already inside a quotation for the outer property\).

Because of this slightly different syntax there are some things to know. If your value is going to contain a comma, then you need to enclose it in single quotes, inside the double quotes of the outer property as shown below.

```markup
<Button Text="Send"
        Command="{Binding SendSms}"
        CommandParameter="{SendMSmsParameters Message='Hello, Dave.' Recipients=1558881234}" />
```

## Parameter

While not a command itself, we will mention this here as it is used by nearly all the below commands. Sometimes you need to pass an additional parameter value to a command. This value might be static, or it could be dynamic based on another view on screen. To do this you use the `Parameter` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Name | string | The name of the parameter to be passed to the command. |
| Value | object | The value to be passed for this parameter. Supports data binding. |

**Examples**

```markup
<Rock:Parameter Name="GroupId"
                Value="18" />
```

```markup
<Rock:Parameter Name="GroupId"
                Value="{Binding Text, Source={x:Reference tbGroup}}" />
```

## Commands

Most of the command below will work in the context of a block's content \(the one exception is the [Callback](block-commands.md#callback) command, that only works inside a block that derives from the Content block\). Many of them will work outside the context of a block as well. For example, in the flyout menu. However, some do require knowing what page you are on and thus will not work in that context. One example of this is the [ShowActionPanel](block-commands.md#showactionpanel) command. It needs to know what page to display the action panel over and if used from the flyout menu that context is not available to it.

### OpenBrowser

This command allows you to open a web address using the built-in browser inside the application.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any additional query string parameters to be included with the URL. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding OpenBrowser}"
        CommandParameter="https://www.google.com/" />
```

```markup
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

Similar to the [OpenBrowser](block-commands.md#OpenBrowser) command, this one opens a URL in a browser window. The difference between the two is that this command uses the devices native web browser and opens the URL in that application. This means your user leaves your mobile app and gets sent over to Safari or Chrome.

If you are opening a url to your own Rock server and wish to ensure the user is logged in, you can pass an empty `rckipid` parameter and an impersonation token will be automatically generated.

The `CommandParameter` can either be a string, which contains the URL and query string parameters, or it can be a reference to an `OpenExternalBrowserParameters` object which contains the following properties.

| Property | Type | Description |
| :--- | :--- | :--- |
| Url | string | The URL to be opened. May contain query string parameters. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any additional query string parameters to be included with the URL. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding OpenBrowser}"
        CommandParameter="https://www.google.com/" />
```

```markup
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

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them \(for example, your `&` must become `&amp;`\).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be pushed onto the navigation stack. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any additional query string parameters to be passed to the page. |
| WaitForReady | bool | Waits until the page is loaded before displaying it. \(_Defaults to **false**._\) |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding PushPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```markup
<Button Text="Tap"
        Command="{Binding PushPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```markup
<Button Text="Tap"
        Command="{Binding PushPage}">
    <Button.CommandParameter>
        <Rock:PushPageParameters PageGuid="e4d80e57-da60-4822-bc22-c071f02958e8">
            <Rock:Parameter Name="GroupId" Value="18" />
            <Rock:Parameter Name="Mode" Value="Edit" />
        </Rock:PushPageParameters>
    </Button.CommandParameter>
</Button>
```

### ReplacePage

This command replaces the current page with a new page. This differs from the [PushPage](block-commands.md#PushPage) command. Using the ReplacePage command, if the user then taps the back button then they will be taken back to the page they were on _before_ the page that called the ReplacePage command.

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them \(for example, your `&` must become `&amp;`\).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used to replace the current page. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any additional query string parameters to be passed to the page. |
| WaitForReady | bool | Waits until the page is loaded before displaying it. \(_Defaults to **false**._\) |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ReplacePage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```markup
<Button Text="Tap"
        Command="{Binding ReplacePage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```markup
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

If the `CommandParameter` is a string, then it is expected to contain a page's Guid value and, optionally, a set of query string parameters. The first parameter is separated by a `?` and any additional parameters are separated by `&`. Since these query string parameters are inside an XML document, you must escape them \(for example, your `&` must become `&amp;`\).

If you are using data binding you can also bind the `CommandParameter` to a true Guid value, in which case it must be the Guid of a valid page.

Finally, if you need advanced parameter usage you can use a `PushPageParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| PageGuid | Guid | The Guid identifier of the page to be used as the new page. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any additional query string parameters to be passed to the page. |
| WaitForReady | bool | Waits until the page is loaded before displaying it. \(_Defaults to **false**._\) |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ShowPage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8" />
```

```markup
<Button Text="Tap"
        Command="{Binding Showage}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```markup
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

This command performs the reverse action of the [PushPage](block-commands.md#PushPage) command. In simple terms, this behaves just like the user tapped the back button in the navigation toolbar at the top. The current page is popped and removed from the stack and the previous page becomes visible. Think in terms of pushing a page that asks the user for confirmation on something and you want to have a Cancel button that goes back to the previous page.

One additional feature is that you can request that the previous page in the stack should be reloaded before it's displayed. This is accomplished by passing `true` to the `CommandParameter`.

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding PopPage}" />
```

```markup
<Button Text="Tap"
        Command="{Binding PopPage}"
        CommandParameter="true" />
```

### PlayVideo

This command initiates the playing of a video in full-screen. Usually it is better to use the `VideoPlayer` view instead, but there are times you just want to play a video when the user taps a button.

The `CommandParameter` consists of a string which contains the URL of the video to be played.

**Example**

```markup
<Button Text="Tap"
        Command="{Binding PlayVideo}"
        CommandParameter="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4" />
```

### PlayAudio

Like the [PlayVideo](block-commands.md#PlayVideo) command, this initiates a full-screen playback of an audio file. Given that it's audio, there won't be much to see.

The `CommandParameter` consists of a string which contains the URL of the audio file to be played.

**Example**

```markup
<Button Text="Tap"
        Command="{Binding PlayVideo}"
        CommandParameter="http://www.noiseaddicts.com/samples_1w72b820/2541.mp3" />
```

### ReloadApplication

Don't use this. No seriously, you can use it but it has a very special purpose. Basically, this instructs the application to reload as if you had just force quit and started it again. This is a development tool that saves us a few seconds when we are debugging stuff, though you may find it useful when debugging your own blocks.

The `CommandParameter` is not used and will be ignored.

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ReloadApplication}" />
```

### ScrollToVisible

There are two ways to initiate this command, we'll show them below. But the basic syntax is you specify the Anchor element that should be scrolled until it becomes visible. Think of this like the HTML Anchor href, or a "jump to" button.

You can either pass the view to be made visible directly by reference in the `CommandParameter` or you can pass an instance of the `ScrollToVisibleParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Anchor | VisualElement | The view which resides inside a ScrollView that should be made visible. |
| Position | ScrollToPosition | The position to put the view at after scrolling \(see below\). _Defaults to **MakeVisible**._ |

The options you have with the `Position` parameter are as follows.

* MakeVisible - Just make sure the anchor is visible on screen.
* Start - Attempt to scroll until the anchor is at the start \(top or left\) of the screen.
* Center - Attempt to scroll until the anchor is at the center of the screen.
* End - Attempt to scroll until the anchor is at the end \(bottom or right\) of the screen.

**Examples**

```markup
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

```markup
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}">
    <Button.CommandParameter>
        <Rock:ScrollToVisibleParameters Anchor="{x:Reference myLabel}"
                                        Position="Start" />
    </Button.CommandParameter>
</Button>
```

This still indicates the same Label we want to use as the Anchor, but it also specifies the position we want it to end up at. Now, this is not a forced position. If there were nothing below the label, it wouldn't be possible for it to end up at the top. But since we have a lot of content below as well, there is enough room to scroll.

Finally, due to the way XAML works, there is a shorthand to the XAML we wrote above.

```markup
<Button Text="Scroll"
        Command="{Binding ScrollToVisible}"
        CommandParameter="{Rock:ScrollToVisibleParameters Anchor={x:Reference myLabel}, Position=Start}" />
```

### ShowActionPanel

This action will show an action panel \(think action sheet in iOS terms\). This is basically a popup that contains a short message and a number of buttons the user can choose from. A common example of this would be a "reply" button in a mail application. When tapping the button it might then popup an action sheet which contains a few buttons to help you decide what you intend to do: Reply, Reply All, Forward.

These popups usually have a Cancel button \(though it's not strictly required. Additionally, you can specify a single "destructive" button that stands out to the user. Often this would be a Delete type action and is usually styled red.

The `CommandParameter` must specify an instance of the `ShowActionPanelParameters` object.

| Property | Type | Description |
| :--- | :--- | :--- |
| Title | string | The title of the action panel, keep it short! |
| CancelTitle | string | The text to display in the cancel button \(optional\). _Defaults to empty string._ |
| DestructiveButton | ActionPanelButton | Defines the button that implies a destructive operation, for example on iOS this button becomes red \(optiona\). _Defaults to **null**._ |
| Buttons | ICollection&lt;ActionPanelButton&gt; | A collection of buttons to be shown, this is the default content property meaning you would just add ActionPanelButton nodes as child elements. |

**Examples**

```markup
<Button Text="Action" Command="{Binding ShowActionPanel}">
    <Button.CommandParameter>
        <Rock:ShowActionPanelParameters Title="My Action Sheet"
                                        CancelTitle="Do Nothing">
            <Rock:ShowActionPanelParameters.DestructiveButton>
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

```markup
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
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any parameters that will be passed to the Lava engine, these manifest as lava variables. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding PageEvent}"
        CommandParameter="UserTap" />
```

```markup
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
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | Any parameters that will be passed to the callback function. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding Callback}"
        CommandParameter="UserTap" />
```

```markup
<Button Text="Tap"
        Command="{Binding Callback}">
    <Button.CommandParameter>
        <Rock:CallbackParameters>
            <Rock:Parameter Name="GroupId" Value="18" />
        </Rock:CallbackParameters>
    </Button.CommandParameter>
</Button>
```

### SendSms

Don't you wish you could add a button with an icon of a chat bubble to your application that would allow your user to send a text message? Yea, we did too. With this command you can _request_ to create a new text message to be sent to a number of people. Notice that we said "request". This does not immediately send the text message but simply uses the OS standard messaging application. In most cases the user is sent over to the message application and can then later return to your application.

If the `CommandParameter` is a plain string, then it is used as a comma delimited list of phone numbers that the message should be sent to. Alternatively you can specify a `SendSmsParameters` object and supply the initial text to use as the message body as well as the list of recipients. This object is described below. Finally, you can also omit the `CommandParameter` entirely and it will simply open up a new blank message for the user to send.

| Property | Type | Description |
| :--- | :--- | :--- |
| Message | string | The body of the message to be sent, this is optional and the user will be able to edit the content before it is sent. |
| Recipients | List\ | The phone numbers of the people who will receive the message. While this is a list of strings, it also accepts a comma delimited string to specify multiple numbers at once. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding SendSms}"
        CommandParameter="15551239876" />
```

```markup
<Button Text="Tap"
        Command="{Binding SendSms}">
    <Button.CommandParameter>
        <Rock:SendSmsParameters Message="Welcome to Rock!"
                                    Recipients="15551239876,15552224444" />
    </Button.CommandParameter>
</Button>
```

```markup
<Button Text="Tap"
        Command="{Binding SendSms}">
    <Button.CommandParameter>
        <Rock:SendSmsParameters Message="Welcome to Rock!">
            <x:String>15551239876</x:String>
            <x:String>15552224444</x:String>
        </Rock:SendSmsParameters>
    </Button.CommandParameter>
</Button>
```

```markup
<Button Text="Tap"
        Command="{Binding SendSms}"
        CommandParameter="{SendSmsParameters Message=Welcome to Rock!, Recipients='15551239876,15552224444'}" />
```

### SendEmail

With this command you can _request_ to create a new e-mail to be sent to a number of people. Notice that we said "request". This does not immediately send the e-mail but simply uses the OS standard e-mail application. In most cases the user is sent over to the e-mail application and can then later return to your application.

If the `CommandParameter` is a plain string, then it is used as a comma delimited list of e-mail addresses that the e-mail should be sent to. Alternatively you can specify a `SendEmailParameters` object and supply the subject, the initial text to use as the e-mail body as well as the list of recipients. This object is described below. Finally, you can also omit the `CommandParameter` entirely and it will simply open up a new blank e-mail for the user to send.

| Property | Type | Description |
| :--- | :--- | :--- |
| Subject | string | The subject of the e-mail to be sent, this is optional and the user will be able to edit before it is sent. |
| Message | string | The body of the e-mail to be sent, this is optional and the user will be able to edit the content before it is sent. |
| Recipients | List\ | The e-mail addresses that will receive the e-mail. While this is a list of strings, it also accepts a comma delimited string to specify multiple e-mail addresses at once. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding SendEmail}"
        CommandParameter="ted@rocksolidchurchdemo.com" />
```

```markup
<Button Text="Tap"
        Command="{Binding SendEmail}">
    <Button.CommandParameter>
        <Rock:SendEmailParameters Subject="Welcome to Rock!"
                                  Message="Thanks for saying you are coming to church tonight!"
                                  Recipients="ted@rocksolidchurchdemo.com, cindy@fakeinbox.com" />
    </Button.CommandParameter>
</Button>
```

```markup
<Button Text="Tap"
        Command="{Binding SendEmail}"
        CommandParameter="{SendEmailParameters Subject=Welcome to Rock!, Recipients=ted@rocksolidchurchdemo.com}" />
```

### CallPhoneNumber

We're sure you can think of other uses, but wouldn't it be nice if your application had your churches phone number displayed somewhere and the user could just tap on it to call your church office? Yep. That would be nice. If only the user was on a phone or something like that.

The `CommandParameter` specifies the phone number to be dialed, if it's blank then nothing will happen.

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding CallPhoneNumber}"
        CommandParameter="15552138874" />
```

### ShareContent

Using the ShareContent command you can open up the standard OS share dialog that lets the user share a piece of text or a URL to other apps and services.

If the `CommandParameter` is a plain string then it will simply share the text string. Alternatively you can specify a `ShareContentParameters` object and supply additional options as seen below.

| Property | Type | Description |
| :--- | :--- | :--- |
| Title | string | The title of the share window \(not used on iOS\). |
| Text | string | A text string to be shared. |
| Uri | string | A URL string to be shared. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ShareContent}"
        CommandParameter="Rock ChMS has an app!" />
```

```markup
<Button Text="Tap"
        Command="{Binding ShareContent}">
    <Button.CommandParameter>
        <Rock:ShareContentParameters Title="Endorse Rock!"
                                     Text="Rock ChMS has an app!"
                                     Uri="https://www.rockrms.com/" />
    </Button.CommandParameter>
</Button>
```

### ShowPopup

Rock Mobile Shell supports the idea of small popup pages. These don't support navigation, but can be useful for a simple display of additional content without leaving the current page.

If the `CommandParameter` is a string then it will be interpreted as a page guid with optional query string parametres. This will display a full Rock page inside the popup view.

Alternatively, you can specify a view to use as the content for the popup. This is ideal for showing additional details of items or perhaps a list filter.

Finally, you can specify a `ShowPopupParameters` object and supply additional options as seen below.

| Property | Type | Description |
| :--- | :--- | :--- |
| Anchor | ShowPopupAnchor | Where to anchor the popup, possible values are `Center`, `Top`, and `Bottom`. _Defaults to **Center**._ |
| Content | View | The view to display inside the popup, if set this will override the PageGuid property. |
| PageGuid | Guid | The Rock page to be displayed inside the popup. |
| Parameters | List&lt;[Parameter](block-commands.md#Parameter)&gt; | A collection of query string parameters that will be passed to the Rock page. |
| Title | string | The title of the popup. |

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ShowPopup}"
        CommandParameter="e4d80e57-da60-4822-bc22-c071f02958e8?GroupId=18&amp;Mode=Edit" />
```

```markup
<Button Text="Tap"
        Command="{Binding ShowPopup}">
    <Button.CommandParameter>
        <Rock:ShowPopupParameters Title="Select Group"
                                  PageGuid="e4d80e57-da60-4822-bc22-c071f02958e8">
            <Rock:Parameter Name="ParentGroupId" Value="8293" />
        </Rock:ShowPopupParameters>
    </Button.CommandParameter>
</Button>
```

```markup
<Button Text="Tap"
        Command="{Binding ShowPopup}">
    <Button.CommandParameter>
        <Rock:ShowPopupParameters Title="Select Group"
                                  Anchor="Top">
            <Rock:ShowPopupParameters.Content>
                <StackLayout>
                    <Label Text="Hello Rock" />
                    <Button Text="Close"
                            Command="{Binding ClosePopup}" />
                </StackLayout>
            </Rock:ShowPopupParameters.Content>
        </Rock:ShowPopupParameters>
    </Button.CommandParameter>
</Button>
```

### ClosePopup

This command is the opposite of the [ShowPopup](block-commands.md#ShowPopup) command. If there is an open popup then it will be closed.

This command takes no parameters.

**Examples**

```markup
<Button Text="Tap"
        Command="{Binding ClosePopup}" />
```

