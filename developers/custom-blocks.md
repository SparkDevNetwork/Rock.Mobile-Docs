# Custom Blocks

Okay, maybe all of this still isn't quite enough for what you need. Or maybe it works, but you are building such complicated entity commands and SQL queries in Lava that it's just not performant enough. Maybe you are just far more familiar with C\# so you'd rather go that way. Possibly you want to build a plugin that provides a new block type with some settings for the admin to configure and then it magically does what it does.

Let's start with the most basic block configuration:

```csharp
[DisplayName( "My Custom Block" )]
[Category( "Rock Solid Church > Mobile" )]
[Description( "A custom block to do magical things." )]
[IconCssClass( "fa fa-square" )]
public class MyCustomBlock : RockMobileBlockType
{
    #region IRockMobileBlockType Implementation

    int IRockMobileBlockType.RequiredMobileAbiVersion => 1;

    string IRockMobileBlockType.MobileBlockType => "Rock.Mobile.Blocks.Content";

    object IRockMobileBlockType.GetMobileConfigurationValues()
    {
        string content = "<Label Text=\"Hello World\" />";

        return new Rock.Mobile.Common.Blocks.Content.Configuration
        {
            Content = content
        }
    }
    
    #endregion
}
```

So here is what we got. We are specifying that in order for this block to render, the mobile app needs to support ABI version 1. Next we are specifying the CLR class name on the mobile app that will handle the rendering, we are going to use the `Rock.Mobile.Blocks.Content` block so that we can update things. This block will render a simple text label that says `Hello World`.

But that is rather boring. Instead, lets have it render out something a bit more expressive.

```csharp
object IRockMobileBlockType.GetMobileConfigurationValues()
{
    string content = @"
<StackLayout>
    <Rock:FormGroup Title=""Search"">
        <Rock:FormField>
            <Rock:TextBox x:Name=""SearchTerm"" />
        </Rock:FormField>
    </Rock:FormGroup>

    <Button Text=""Search"" StyleClass=""btn, btn-primary"" Command=""{Binding Callback}"">
        <Button.CommandParameter>
            <Rock:CallbackParameters Name="":Search"">
                <Rock:Parameter Name=""Term"" Value=""{Binding Source={x:Reference SearchTerm}, Path=Text}"" />
            </Rock:CallbackParameters>
        </Button.CommandParameter>
    </Button>
</StackLayout>
";

    return new Rock.Mobile.Common.Blocks.Content.Configuration
    {
        Content = content
    }
}
```

If you look very carefully, you'll notice that we prefixed the action command `Search` with a `:`. Here is the difference. Without the `:` prefix, the mobile block will call a single method:

```csharp
[BlockAction]
public string GetCallbackContent( string command, Dictionary<string, object> parameters)
```

That is perfectly fine and you can do it that way if you want. However, with the `:` prefix then it calls a method named for the `Name` and passes the parameters as actual method parameters. So in our case, our search method would be:

```csharp
[BlockAction]
public string Search( string term )
```

Either will achieve the same result, but the latter form might be easier to read in your code - especially if you have multiple commands being handled. The return type of these methods is a simple string that contains the XAML code used to render the UI. Let's use the latter form and build our search results:

```csharp
[BlockAction]
public object Search( string term )
{
    var searchClient = IndexContainer.GetActiveComponent();
    var searchEntities = new List<int> { EntityTypeCache.Get( SystemGuid.EntityType.GROUP.AsGuid() ) };
    var results = searchClient.Search( term,
        SearchType.Wildcard,
        searchEntities,
        "GroupTypeName^Serving Team",
        5,
        0,
        out int totalResults );

    var sb = new StringBuilder();
    sb.AppendLine( "<StackLayout>" );
    foreach ( var result in results )
    {
        var button = $@"
    <Button Text=""{result.DocumentName}""
            StyleClass=""btn, btn-primary""
            Command=""{{Binding Callback}}"">
        <Button.CommandParameter>
            <Rock:CallbackParameters Name="":ShowGroup"">
                <Rock:Parameter Name=""GroupId"" Value=""{result.Id}"" />
            </Rock:CallbackParameters>
        </Button.CommandParameter>
    </Button>";
        sb.AppendLine( button );
    }
    sb.AppendLine( "</StackLayout>" );

    return new Rock.Mobile.Common.Blocks.Content.CallbackResponse
    {
        Content = sb.ToString()
    };
}
```

So we use the Universal Search component to get a list of all matching serving team groups. Then we loop over those results and build a stack layout of buttons. Each button will trigger the `:ShowGroup` command with a different `GroupId` value. A handler for such a command might look like:

```csharp
[BlockAction]
public object ShowGroup( int groupId )
{
    return new Rock.Mobile.Common.Blocks.Content.CallbackResponse
    {
        Content = "<Rock:NotificationBox NotificationType=""Error"" Text=""Not Implemented"" />"
    };
}
```

This is a simple handler that just displays a notification on the screen stating it hasn't been implemented yet. Obviously you will want to have to actually do something. For example, you might have it display some information about the group and then display two buttons: `Cancel` and `Join`.


## Dynamic Initial Content

What we described above will leave you with a block whose initial content is static, that is it never changes unless the admin deploys a new application bundle. That might work for you, but you probably want to change that content based on query string parameters and such. Let's modify our block so that it instructs the mobile application to be dynamic.

```csharp
[DisplayName( "My Custom Block" )]
[Category( "Rock Solid Church > Mobile" )]
[Description( "A custom block to do magical things." )]
[IconCssClass( "fa fa-square" )]
public class MyCustomBlock : RockMobileBlockType
{
    #region IRockMobileBlockType Implementation

    int IRockMobileBlockType.RequiredMobileAbiVersion => 1;

    string IRockMobileBlockType.MobileBlockType => "Rock.Mobile.Blocks.Content";

    object IRockMobileBlockType.GetMobileConfigurationValues()
    {
        return new Rock.Mobile.Common.Blocks.Content.Configuration
        {
            Content = null,
            DynamicContent = true
        }
    }
    
    #endregion
    
    [BlockAction]
    public object GetInitialContent()
    {
        return new Rock.Mobile.Common.Blocks.Content.CallbackResponse
        {
            Content = "<Label Text=\"Hello World\" />"
        };
    }
}
```
