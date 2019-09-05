# Custom Blocks

Okay, maybe all of this still isn't quite enough for what you need. Or maybe it works, but you are building such complicated entity commands and SQL queries in Lava that it's just not performant enough. Maybe you are just far more familiar with C\# so you'd rather go that way. Possibly you want to build a plugin that provides a new block type with some settings for the admin to configure and then it magically does what it does.

Let's start with the most basic block configuration:

```text
[DisplayName( "My Custom Block" )]
[Category( "Rock Solid Church > Mobile" )]
[Description( "A custom block to do magical things." )]
[IconCssClass( "fa fa-square" )]
public class MyCustomBlock : RockBlockType, IRockMobileBlockType
{
    #region IRockMobileBlockType Implementation
    int IRockMobileBlockType.RequiredMobileAbiVersion => 1;
    string IRockMobileBlockType.MobileBlockType => "Rock.Mobile.Blocks.DynamicContent";

    object IRockMobileBlockType.GetMobileConfigurationValues()
    {
        string content = "<Label Text=\"Hello World\" />";
        return new Rock.Mobile.Common.Blocks.DynamicContent.Configuration
        {
            InitialContent = content;
        }
    }
    #endregion
}
```

So here is what we got. We are specifying that in order for this block to render, the mobile app needs to support ABI version 1. Next we are specifying the CLR class name on the mobile app that will handle the rendering, we are going to use the `Rock.Mobile.Blocks.DynamicContent` block so that we can update things. This block will render a simple text label that says `Hello World`.

But that is rather boring. Instead, lets have it render out the exact same thing we did above for the universal search.

```text
    object IRockMobileBlockType.GetMobileConfigurationValues()
    {
        string content = @"
    <StackLayout>
        <Rock:FormGroup Title=""Search"">
            <Rock:FormField>
                <Rock:TextBox x:Name=""SearchTerm"" />
            </Rock:FormField>
        </Rock:FormGroup>

        <Button Text=""Search"" StyleClass=""btn, btn-primary"" Command=""{Binding UpdateContent}"">
            <Button.CommandParameter>
                <Rock:Action Command="":Search"">
                    <Rock:ActionParameter Name=""Term"" Value=""{Binding Source={x:Reference SearchTerm}, Path=Text}"" />
                </Rock:Action>
            </Button.CommandParameter>
        </Button>
    </StackLayout>
";
        return new Rock.Mobile.Common.Blocks.DynamicContent.Configuration
        {
            InitialContent = content;
        }
    }
```

If you look very carefully, you'll notice that we changed the action command from `Search` to `:Search`. Here is the difference. Without the `:` prefix, the mobile block will call a single method:

```text
[BlockAction]
public string GetDynamicContent( string command, Dictionary<string, object> parameters)
```

That is perfectly fine and you can do it that way if you want. However, with the `:`: prefix then it calls a method named for the `Command` and passes the parameters as actual method parameters. So in our case, our search method would be:

```text
[BlockAction]
public string Search( string term )
```

Either will achieve the same result, but the latter form might be easier to read in your code - especially if you have multiple commands being handled. The return type of these methods is a simple string that contains the XAML code used to render the UI. Let's use the latter form and build our search results:

```text
[BlockAction]
public string Search( string term )
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
        var button = $"
    <Button Text=""{ result.DocumentName }""
            StyleClass=""btn, btn-primary""
            Command=""{{ Binding UpdateContent }}"">
        <Button.CommandParameter>
            <Rock:Action Command="":ShowGroup"">
                <Rock:ActionParameter Name=""GroupId"" Value=""{ result.Id }"" />
            </Rock:Action>
        </Button.CommandParameter>
    </Button>";
        sb.AppendLine( button );
    }
    sb.AppendLine( "</StackLayout>" );

    return sb.ToString();
}
```

So we use the Universal Search component to get a list of all matching serving team groups. Then we loop over those results and build a stack layout of buttons. Each button will trigger the `:ShowGroup` command with a different `GroupId` value. A handler for such a command might look like:

```text
[BlockAction]
public string ShowGroup( int groupId )
{
    return "<Rock:NotificationBox NotificationType=""Error"" Text=""Not Implemented"" />";
}
```

This is a simple handler that just displays a notification on the screen stating it hasn't been implemented yet. Obviously you will want to have to actually do something. For example, you might have it display some information about the group and then display two buttons: `Cancel` and `Join`.

