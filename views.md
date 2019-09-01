---
description: >-
  Views are user-interface objects such as labels, buttons, and sliders that are
  commonly known as controls or widgets in other graphical programming
  environments.
---

# Views

## Xamarin Forms Base Views

Xamarin Forms provides a rich collection of base views \(controls\). You can find a documentation on these views on the [Xamarin Forms Documentation site](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/controls/views).

## Rock Mobile Custom Controls

 In addition to all the [Xamarin Views](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/controls/views) you have available, there are a number of Rock specific views that you can use to customize your user's experience.

### ActivityIndicator

 I_nherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Sometimes things take a moment to load. You don't want to just display a big white screen for the user to stare at, you want them to know we're working hard for them. That said, usually you are not going to be instantiating this view directly. But just in case you want to, here you go.

The ActivityIndicator will begin "working" when it becomes visible on screen. Initially, it will not show anything. Once a built-in timer elapses \(350ms\) then the actual indicator becomes visible. Your user will not care if it takes your page a few hundred milliseconds to load so there is no need to show them a spinning wheel right away.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| IsVisible | bool | If `true` then the activity indicator will be visible and show that work is being done. |

**Example**

```text
<Rock:ActivityIndicator />
```

![ActivityIndicator](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/ActivityIndicator-1.png)

### FollowingIcon

_Inherits from_ [_Xamarin.Forms.Label_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.label)

Wouldn't it be nice if your mobile users could follow a content channel item and then later view a list of all their followed items? The `FollowingIcon` is what you are looking for. This view displays one of two icons, depending on the current state. That initial state is determined by the `IsFollowed`property. When the user taps on the icon, the state is toggled and an API call is made to the server to update the followed state of the entity.

This of course assumes that the user is currently logged in. If they are not logged in then no action will be taken. In the future we may add a new property to indicate an error message to the user that you can use to encourage your users to register or login to full functionality.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| EntityId | int | The Id number of the entity to be followed. |
| EntityTypeId | int | The Id number of the entity's type. |
| IsFollowed | bool | The current followed state of the entity. _Defaults to **false**._ |
| FontSize | double | The size of the font to use. |
| FollowingIconClass | string | The icon class to display when the entity is being followed. _Defaults to **star**._ |
| FollowingIconFamily | [IconFamily](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#IconFamily) | The icon family to use when the entity is not being followed. _Defaults to **FontAwesomeSolid**._ |
| FollowingIconColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use for the icon when the entity is being followed. |
| NotFollowingIconClass | string | The icon class to display when the entity is not being followed. _Defaults to **star**._ |
| NotFollowingIconFamily | [IconFamily](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#IconFamily) | The icon family to use when the entity is not being followed. _Defaults to **FontAwesomeRegular**._ |
| NotFollowingIconColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use for the icon when the entity is not being followed. |
| NotLoggedInText | string | The message that is displayed to the user when they try to follow an item while not logged in. _Defaults to **You must be logged in to follow this item.**._ |

**Example**

The example below demonstrates how to display a group's name and a following icon to let the user follow that group. We are using the default icons. The first screenshot shows the not followed state and the second screenshot shows the followed state.

```text
<StackLayout Orientation="Horizontal">
    {% assign group = 41 | GroupById %}
    <Rock:FollowingIcon EntityTypeId="{{ group.TypeId }}"
                        EntityId="{{ group.Id }}"
                        FontSize="20"
                        IsFollowed="{{ group | IsFollowed }}"
                        FollowingIconColor="#ee7725"
                        NotFollowingIconColor="#ee7725" />
    <Label Text="{{ group.Name }}" />
</StackLayout>
```

![Not Followed](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/FollowingIcon-1.png)

![Followed](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/FollowingIcon-2.png)

### FormFieldStack

_Inherits from_ [_Xamarin.Forms.Layout_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.layout-1)

This view is not currently intended to be used directly. There is nothing preventing you from using it, but your mileage may vary. The FormFieldStack is used internally by the [FormGroup](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#FormGroup) to display multiple fields in a vertical layout.

When displaying a [FormField](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#FormField) this view is responsible for drawing the border around the group of fields as well as the lines between the fields.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| BorderColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to be used for the border as well as the inter-field lines. |
| BorderSize | double | The width, in pixels, to be used when drawing the borders. _Default value is 1._ |

**Example**

```text
<Rock:FormFieldStack>
    <Rock:FormField>
        <Rock:TextBox Label="Username" />
    </Rock:FormField>
    <Rock:FormField>
        <Rock:TextBox Label="Password" />
    </Rock:FormField>
</Rock:FormFieldStack>
```

![FormField](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/FormFieldStack-1.png)

### Html

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

This view allows you to render HTML into the page. HTML rendering is limited on mobile applications and not as performant as it could be. Therefore it is recommended you limit your HTML usage. You don't need to avoid it, just try not to render a few dozen paragraphs of text and expect it to be snappy.

A limited subset of HTML tags is supported. Any non-supported tags will be rendered as their plain text contents \(meaning, the HTML tags are stripped\).

* `H1` - `H6`
* `DIV`
* `P`
* `SPAN`
* `STRONG`
* `B`
* `EM`
* `I`
* `CODE`
* `PRE`
* `OL`
* `UL`
* `LI`

In addition, in-line styles are supported, but again only for a few styles are supported:

* `color`
* `background-color`

It should be noted that because you are defining your HTML content in an XML file you cannot simply do something like `Text="{{ Item.Html }}"` and expect it to work. You will need to escape the HTML text, or you can use the `CDATA` trick as shown in the example.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The text content that contains the HTML. |
| Lava | bool | If **true** then the Text will be processed for any Lava before final rendering happens. _Defaults to **false**._ |
| FontFamily | string | The font family to use by default. |
| FontSize | double | The size of the font to use by default. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color of the text to use by default. _Default value is **Black**._ |

**Example**

```text
<Rock:Html TextColor="Red">
    <![CDATA[
    <p>This is some <span style='color: blue'>sample</span> text.</p>
    <p>This is a second paragraph.</p>
    ]]>
</Rock:Html>
```

![Html](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/Html-1.png)

### Icon

_Inherits from_ [_Xamarin.Forms.Label_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.label)

We are all accustomed to displaying nice icons on our pages. Your mobile application should be no different. This view allows you to display an icon from the FontAwesome library on your page.

Unlike on the web, you must specific the icon family that you wish to display from. So for example, you cannot just set `IconClass="car"` and have it work, because the Car icon is not availabe in the Regular font \(which is the default icon font family\). So you would need to display with `IconClass="car" IconFamily="Solid"`.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| IconClass | string | The icon class name to use. This can be found on the FontAwesome website. If it specified `fas fa-car` then you would use `car`. |
| IconFamily | [IconFamily](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#IconFamily) | The icon font family to use to display the icon. _Default value is **FontAwesomeRegular**._ |
| FontSize | double | The size of the icon. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color of the icon to use by default. |

**Example**

```text
<StackLayout Orientation="Horizontal">
    <Rock:Icon IconClass="car" IconFamily="FontAwesomeSolid" FontSize="32" />
    <Rock:Icon IconClass="rockrms" IconFamily="FontAwesomeBrands" TextColor="#ee7725" FontSize="32" />
    <Rock:Icon IconClass="user" FontSize="32" />
</StackLayout>
```

![Icon](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/Icon-1.png)

### ItemsView

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

You have probably already thought that you could display a list of things with a Content block and some Lava entity commands and then just loop through them and render out the XAML to display each item. Sure you could do that, but that might not be the best route. The ItemsView allows you to use a template to design how you want each item displayed and also specify the layout \(list or grid\).

For example, you may just want a simple vertical list of rows with a small thumbnail image and the title. Or maybe you want a grid of just the thumbnails. Either is possible by changing some properties. This view copies a number of concepts from the [Xamarin.Forms.CollectionView](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.collectionview) and you will find that they are, for the most part, interchangeable when it comes to their base functionality. They both layout a number of items on the page.

You will find the CollectionView to be more performant with larger lists as it uses native controls in the OS to perform the layout. But you will also find it more limited in some other functionality. For example, you cannot have a header that initially displays above the items and then scrolls away as you scroll through the list. ItemsView does allow you to do this.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| ItemsSource | IEnumerable | The collection of items that will be used along with the ItemTemplate to render the content. |
| ItemTemplate | DataTemplate | The View structure template that will be used to build each individual item view. |
| ItemsLayout | ItemsLayout | Defines the type of layout to perform with the items. _Default is **ListItemsLayout** in **Vertical**orientation._ |
| IsScrollable | bool | If **true**, then a ScrollView will be used to wrap the content. This is required for dynamic loading, but also means the page must not have it's own ScrollView otherwise you will have issues. |
| HeaderView | View | The view structure to place above the first item. If IsScrollable is true then this view will scroll with the items. |
| FooterView | View | The view structure to place below the last item. If IsScrollable is true then this view will scroll with the items. |
| FixedHeaderView | View | The view structure to place above the items and HeaderView. This view will not scroll with the content. |
| FixedFooterView | View | The view structure to place below the items and FooterView. This view will not scroll with the content. |
| EmptyView | View | If ItemsSource is empty then this view will be displayed. |
| SelectedItem | object | The currently selected item from ItemsSource. |
| SelectionChangedCommand | ICommand | This command will be executed when an item is tapped on. This is accomplished by your ItemTemplate being wrapped in another View that has a tap handler installed on it. |
| SelectionChangedCommandParameter | object | The parameter to pass to the called command. |

In the following examples we are specifying an array of strings as the `ItemsSource`, this wouldn't be normal but it allows us to concisely provide an array of test items.

**Example**

```text
<Rock:ItemsView>
    <Rock:ItemsView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Car Show</x:String>
            <x:String>Easter 2019</x:String>
            <x:String>Rock Solid Finances</x:String>
        </x:Array>
    </Rock:ItemsView.ItemsSource>
    <Rock:ItemsView.ItemTemplate>
        <DataTemplate>
            <Frame BorderColor="#ee7725" HasShadow="false" Margin="0,0,0,10">
                <StackLayout Orientation="Horizontal">
                    <Rock:Icon IconClass="User" />
                    <Label Text="{Binding .}" />
                </StackLayout>
            </Frame>
        </DataTemplate>
    </Rock:ItemsView.ItemTemplate>
</Rock:ItemsView>
```

![ItemsView](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/ItemsView-1.png)

**Example**

```text
<Rock:ItemsView IsScrollable="true">
    <Rock:ItemsView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Car Show</x:String>
            <x:String>Easter 2019</x:String>
            <x:String>Rock Solid Finances</x:String>
        </x:Array>
    </Rock:ItemsView.ItemsSource>
    <Rock:ItemsView.ItemsLayout>
        <Rock:GridItemsLayout Orientation="Vertical" Span="2" HorizontalItemSpacing="10" VerticalItemSpacing="10" />
    </Rock:ItemsView.ItemsLayout>
    <Rock:ItemsView.FixedHeaderView>
        <ContentView BackgroundColor="#ee7725">
            <Label HorizontalTextAlignment="Center" Margin="15">Fixed Header</Label>
        </ContentView>
    </Rock:ItemsView.FixedHeaderView>
    <Rock:ItemsView.HeaderView>
        <ContentView BackgroundColor="#ae5715" Margin="0,0,0,10">
            <Label HorizontalTextAlignment="Center" Margin="15" TextColor="White">Header</Label>
        </ContentView>
    </Rock:ItemsView.HeaderView>
    <Rock:ItemsView.ItemTemplate>
        <DataTemplate>
            <Frame BorderColor="#ee7725" HasShadow="false">
                <StackLayout Orientation="Horizontal">
                    <Rock:Icon IconClass="User" />
                    <Label Text="{Binding .}" />
                </StackLayout>
            </Frame>
        </DataTemplate>
    </Rock:ItemsView.ItemTemplate>
    <Rock:ItemsView.FooterView>
        <ContentView BackgroundColor="#ae5715" Margin="0,10,0,0">
            <Label HorizontalTextAlignment="Center" Margin="15" TextColor="White">Footer</Label>
        </ContentView>
    </Rock:ItemsView.FooterView>
    <Rock:ItemsView.FixedFooterView>
        <ContentView BackgroundColor="#ee7725">
            <Label HorizontalTextAlignment="Center" Margin="15">Fixed Footer</Label>
        </ContentView>
    </Rock:ItemsView.FixedFooterView>
</Rock:ItemsView>
```

![ItemsView](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/ItemsView-2.png)

If we had enough items to fill the screen in this second example, you would see that initially only the **Fixed Header**, **Header**, a number of items and the **Fixed Footer** would be visible. The **Footer** would be off-screen because it would be inside the scroll view. If you were to scroll that long list of items up you would push the **Header** off screen and eventually the **Footer** would appear. Both the **Fixed Header** and **Fixed Footer** would always stay visible.

### LoginStatus

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Okay let's face it. You aren't going to be using this view on any old page. It's a one-off. But you are going to need to know how to customize it for the flyout.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| WelcomeMessage | string `Lava` | The welcome message that is displayed when a user is logged in. _Defaults to **Hello {{ CurrentPerson.FirstName }}!**_ |
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

![LoginStatus](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/LoginStatus-1.png)

### Markdown

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

Another way that information is often styled in Rock is with something called [Markdown](https://www.markdownguide.org/cheat-sheet). This syntax allows the user to _indicate_ that they want things formatted in a certain fashion, but it does not give them the ability to specify exactly how that formatted is done. For example, they can specify that they want a heading, but they don't get to pick exactly how that heading is formatted.

Not everything in the Markdown syntax is supported, for example tables and footnotes are not supported. But most of your basic syntax will be supported. As with the [Html](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#Html) view, you will probably want to wrap your content in a `CDATA` tag.

When converting the markdown to XAML the Markdown control will add StyleClasses for you. Headings will get `.heading1-.heading6`, paragraphs will be assigned the `.text` class and links will be assigned the `.link` CSS class.

_Note:_ Links are only clickable at a leaf block level \(Xamarin.Forms formatted strings doesn't support span user interactions\) : if a leaf block contains more than one link, the user is prompted. This is almost a feature since text may be too small to be enough precise! ;\)

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The markdown text to be displayed. |
| Lava | bool | If **true** then the Text will be processed for any Lava before final rendering happens. _Defaults to **false**._ |
| FontFamily | string | The font family to use when rendering the markdown content. |
| FontSize | double | The size of the font to use as the base-size when rendering. |
| TextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The text color to use for plain text when rendering the content. _Defaults to **Black**._ |
| HeadingTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering markdown headings. |
| CodeTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering inline code snippets and full code blocks. |
| CodeBackgroundColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `BackgroundColor` when rendering inline code snippets and full code blocks. |
| QuoteTextColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | If set, overrides the `TextColor` when rendering quoted text. |
| QuoteBackgroundColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | Sets the background color used when rendering quoted text. _Defaults to **Gray at 20% alpha**._ |
| BarColor | [Color](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.color) | The color to use when any lines and bars need to be rendered. |

**Example**

```text
<Rock:Markdown HeadingTextColor="#3bab3b">
    <![CDATA[
# Heading

This is some **bold** text.
    ]]>
</Rock:Markdown>
```

![Markdown](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/Markdown-1.png)

### NotificationBox

_Inherits from_ [_Xamarin.Forms.Frame_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.frame)

It happens. Something goes wrong and you need to display an error message to the user. Or perhaps something goes right and you want to be sure the user has feedback that everything is taken care of. Either way, you need a nice way to display a message on the page that has some nice colorful visual indicators.

The NotificationBox allows you to display a color-coded notification on the page. The notification will be colored to match the type of notification and contain the text you specify. You can also include an optional header text to stand out even more.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Text | string | The text to be displayed as the message of the notification. |
| HeaderText | string | An optional bit of text that can be used to give the user context about the message. |
| NotificationType | [NotificationType](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference#NotificationType) | The type of notification to display. _Defaults to **Information**._ |

**Example**

```text
<Rock:NotificationBox NotificationType="Information"
                      HeaderText="Information Needed"
                      Text="Please update your information below to keep our records current." />
```

![NotificationBox](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/NotificationBox-1.png)

### VideoPlayer



_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

While there are block commands that will allow you to start playback of an audio or video file in full-screen mode, sometimes you want that video embedded on the page. For example, on your message library pages you might want to display the sermon title, then the video in-line, and then the description of that sermon.

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Source | string | The URL to be loaded into the video player. Supports MP4 and HLS. |
| AutoPlay | bool | If `true` then the video will start playing as soon as it has loaded into the player. \_Defaults to `false`. |
| InitalAspectRatio | string | The initial aspect ratio to use until the video has loaded and we know the actual aspect ratio. Can be specified as either a `width:height` ratio \(example `16:9`\) or as a decimal number \(example `1.77777`\). _Defaults to **16:9**._ |

**Example**

```text
<Rock:VideoPlayer Source="https://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"
                  AutoPlay="false" />
```

![Video Player](https://github.com/SparkDevNetwork/Rock.Mobile/wiki/Developer-Reference-Images/VideoPlayer-1.png)

> Note: Currently on Android it will just show a black box with the play button if AutoPlay is false. The above screenshot is how it will look on iOS devices. If we can figure out how to get Android to display a thumbnail image as well then we will.



