# Advanced: Dynamic Content

As you've seen you can do a lot with a `Content` block. While that block does allow for some dynamic ability via Lava processing, it's essentially a "one-shot" block. Whatever it renders is what you are stuck with, so to speak. Think of it like the HTML Content block. It's certainly dynamic in that you can use Lava to customize how it looks, but it doesn't allow the user to interact with it and change the content.

Or does it?

## Callbacks

The content block also supports what is called `Callbacks`. If you have an ASP.Net background, you can think of this as a partial postback. Otherwise, just know that this allows you to change the content displayed in the Content block in response to a user action, such as tapping on a button. To do this, it adds a new Command that is available for you to use: `Callback`. This command allows you to send the command and parameters you define back to a special lava template defined in the Advanced Settings tab of the block.

In a previous article, we built a simple search page that linked the user out to google to perform the search. Linking out to Google is great an all, but what if we want to use the built-in universal search? Let's look at what that might look like. First let's define the primary content of the block.

```xml
<StackLayout>
    <Rock:FormGroup Title="Search">
        <Rock:FormField>
            <Rock:TextBox x:Name="SearchTerm" />
        </Rock:FormField>
    </Rock:FormGroup>

    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding Callback}">
        <Button.CommandParameter>
            <Rock:CallbackParameters Name="Search">
                <Rock:Parameter Name="Term"
                                Value="{Binding Source={x:Reference SearchTerm}, Path=Text}" />
            </Rock:Action>
        </Button.CommandParameter>
    </Button>
</StackLayout>
```

This still gives us a pretty Search text field for the user to enter a search term into, as well as a button to press. The button now calls the `Callback` command and we have defined our custom Action object to have a sub-command of `Search`. We also defined a parameter called `Term` that will take it's value from the textbox.

Next, let's define what will happen when the user clicks the `Search` button. This is what we are going to put into our Callback Event in the block's settings.

```xml
<StackLayout>
    <Rock:FormGroup Title="Search">
        <Rock:FormField>
            <Rock:TextBox x:Name="SearchTerm" Text="{{ Paremeters.Term | Escape }}" />
        </Rock:FormField>
    </Rock:FormGroup>

    <Button Text="Search"
            StyleClass="btn, btn-primary"
            Command="{Binding Callback}">
        <Button.CommandParameter>
            <Rock:CallbackParameters Name="Search">
                <Rock:Parameter Name="Term"
                                Value="{Binding Source={x:Reference SearchTerm}, Path=Text}" />
            </Rock:Action>
        </Button.CommandParameter>
    </Button>

    {% if Command == "Search" %}
        <StackLayout Margin="0,20,0,0">
            {% search query:'{{ Parameters.Term }}' entities:'group' fieldcriteria:'GroupTypeName^Serving Team' %}
                {% for result in results %}
                    <Button Text="{{ result.DocumentName | Escape }}"
                            StyleClass="btn, btn-info" />
                {% endfor %}
            {% endsearch %}
        </StackLayout>
    {% endif %}
</StackLayout>
```

Notice that we duplicated our original content. That is because we want the page to still have the textbox and Search button so that the user can change their search term and try again. Notice we also added the default Text property to the textbox. This will keep whatever the user entered into the textbox in place when the content is reloaded.

After all that, comes our new code. We check if the Command variable is the word `Search` which is what we are expecting. If found, we then use the search lava command to search our serving teams and look for any groups that match the search term the user entered \(`{{ Parameters.Term }}`\). For each result we get back, we render a button with the group name on it. If this was a full working example we would make that button do something, like go to a Rock page and pass the group Id.

So we implemented an internal Rock search system for our mobile application even though there is no "search" block available. And we did it without C\# code. Now, I mentioned above that if this were a real search block we would need the "group button" to do something. As I said above, that might be going to another page and passing the Group Id in. But we could also build another `<Rock:Action />` for each button that performs another `Callback` action and updates the UI based on the group they have selected.

## Page Events

Let's set the stage because this is going to give some of you heartburn. Lava is designed and meant to be used as a formatting engine. Said another way, it's not meant to generate any content but just format content that already exists. This you probably know. Now, throw that idea out the window. We are going to use Lava in a way it was never meant to be used. You might ask why. In the simplest terms, we needed a scripting engine that you would be familiar with. You are already familiar with doing `if` checks and other logic with Lava, so we decided to just use what you already know. If this makes you nervous, take an antacid pill because we're diving in!

When you edit a mobile page in Rock, there is a section called `Event Handler`. This is Lava that will be executed when certain events happen on the page. You might be thinking, _so what? I can only format data with Lava_. Oh contraire. Well, normally you would be correct, but we have added a few custom bits to Lava in the mobile application. Let's start with a really simple example. First put a Content block on your page and set the content to be:

```markup
<Label Text="{Binding PageValues.Message}" />
```

Now edit your page settings and put this in for the Event Handler:

```text
{% if Event == 'Load' or Event == 'Resize' %}
    {% if Device.Orientation == "Landscape" %}
        {% setpagevalue 'Message', 'You are looking at this sideways!' %}
    {% else %}
        {% setpagevalue 'Message', 'Good to see you upright.' %}
    {% endif %}
{% endif %}
```

Once you deploy this you will see the proper text appear depending on which way you are holding your device. So what is going on here? Well, we setup our label to bind the Text value to `PageValues.Message`. The `PageValues` object exists on each page and is essentially a dictionary. You can store whatever values you want in it and access them. Because this object exists at the page level, it is shared between blocks.

Next, in our Event Handler we are checking the device's orientation. Depending on the value we set the `Text` value of the `UsersValue` dictionary to the string it should have.

Okay great, your probably thinking, so I can make text appear when they rotate their phone. What good is that? Okay, how about a more useful example? Let's take a Content Channel Item List block. Suppose you wanted to display the results with a thumbnail image. This might look good on a phone in portrait orientation, but what about if the phone is rotated or if they are on a tablet? Now you have these massive thumbnails. Using the Lava Events we will adjust the number of thumbnails displayed per row. Let's setup the template \(note: we are leaving out the xmlns declarations as they waste a lot of space\):

```markup
<Rock:ItemsView ItemsSource="{Binding Items}">
    <Rock:ItemsView.ItemTemplate>
        <DataTemplate>
            <Rock:Image ImageUrl="{Binding Image}" />
        </DataTemplate>
    </Rock:ItemsView.ItemTemplate>
    <Rock:ItemsView.ItemsLayout>
        <Rock:GridItemsLayout Span="{Binding PageValues.Columns}" />
    </Rock:ItemsView.ItemsLayout>
</Rock:ItemsView>
```

Above, we are setting up a custom `ItemsLayout` and have specified that it should be a grid, which by default is vertical. The `Span` parameter specifies how many columns there will be. Since we are binding that value to `PageValues.Columns` we will need to set that in our Event Handler:

```text
{% if Event == 'Load' or Event == 'Resize' %}
    {% if Device.Width > 900 %}
        {% assign columns = 4 %}
    {% elsif Device.Width > 600 %}
        {% assign columns = 3 %}
    {% else %}
        {% assign columns = 2 %}
    {% endif %}

    {% setpagevalue 'Columns', columns %}
{% endif %}
```

What this will give us is a grid that is 2 columns wide on really narrow screens, 3 columns on medium width screens and 4 columns on really wide screens. And, this happens each time you rotate the device, this isn't just a set once and hope it's correct until they leave the page.

So how do you know why your Event Handler is being executed? As you saw, there is an `Event` variable defined which will tell you the event that initiated the lava. Currently, there are two pre-defined events:

* `Load`: This event is initiated when the page is loaded, but before any blocks have been instantiated.
* `Resize`: This event is initiated when the available screen space has changed, such as a rotation.

We just mentioned above "pre-defined" events. You can initiate your own events with the `LavaEvent`command. The name of the event you wish to trigger is specified by the `CommandParameter`. This means you can setup a button on screen that will trigger a custom Lava event and then you can update your PageValues any way you wish.

