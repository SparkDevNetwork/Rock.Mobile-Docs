# Advanced: Dynamic Content

As you've seen you can do a lot with a `Content` block. While that block does allow for some dynamic ability via Lava processing, it's essentially a "one-shot" block. Whatever it renders is what you are stuck with, so to speak. Think of it like the HTML Content block. It's certainly dynamic in that you can use Lava to customize how it looks, but it doesn't allow the user to interact with it and change the content.

That is where the `Dynamic Content` block steps in. It behaves much the same way, in that you use Lava to generate the XAML which defines the interface. But it adds a new Command that is available for you to use: `UpdateContent`. This command allows you to send the action and parameters you define back to the original Lava template.

Said another way, your Lava template runs to render the original XAML content. During an `UpdateContent` command, it runs a second time to render the updated XAML content. This `UpdateContent` step can be performed as many times as you wish. So, for example, let's continue with our custom Search block. Linking out to Google is great an all, but what if we want to use the built-in universal search? Let's look at what that might look like:

```text
{% if Command == empty %}
    <StackLayout>
        <Rock:FormGroup Title="Search">
            <Rock:FormField>
                <Rock:TextBox x:Name="SearchTerm" />
            </Rock:FormField>
        </Rock:FormGroup>

        <Button Text="Search" StyleClass="btn, btn-primary" Command="{Binding UpdateContent}">
            <Button.CommandParameter>
                <Rock:Action Command="Search">
                    <Rock:ActionParameter Name="Term" Value="{Binding Source={x:Reference SearchTerm}, Path=Text}" />
                </Rock:Action>
            </Button.CommandParameter>
        </Button>
    </StackLayout>
{% endif %}
```

So we made a small change to the original XAML. We changed the action to have a `Command` of `Search`, and the main Command is now `{Binding UpdateContent}` and the parameter got changed from `q` to `Term`. We also wrapped everything in an `if` statement. The `Command` lava variable will either be an empty string \(on initial load\) or a string that correlates to the `Command` of the `<Rock:Action />`. In this case, we are checking if it is blank so this is what will render on initial page load.

Next, let's define what happens when the user clicks the `Search` button.

```text
{% elseif Command == "Search" %}
    <StackLayout>
        {% search query:'{{ Parameters.Term }}' entities:'group'  fieldcriteria:'GroupTypeName^Serving Team' %}
            {% for result in results %}
                <Button Text="{{ result.DocumentName }}" StyleClass="btn, btn-primary" />
            {% endfor %}
        {% endsearch %}
    </StackLayout>
{% endif %}
```

We are using the Search lava command to search our serving teams and look for any groups that match the search term the user entered \(`{{ Parameters.Term }}`\). For each result we get back, we render a button with the group name on it. If this was a full working example we would make that button do something, like go to a Rock page and pass the group Id.

So we implemented an internal Rock search system for our mobile application even though there is no "search" block available. And we did it without C\# code. Now, I mentioned above that if this were a real search block we would need the "group button" to do something. As I said above, that might be going to another page and passing the Group Id in. But we could also build another `<Rock:Action />` for each button that performs another `UpdateContent` action and updates the UI based on the group they have selected:

```text
{% elseif Command == "Search" %}
    <StackLayout>
        {% search query:'{{ Parameters.Term }}' entities:'group'  fieldcriteria:'GroupTypeName^Serving Team' %}
            {% for result in results %}
                <Button Text="{{ result.DocumentName }}" StyleClass="btn, btn-primary" Command="{Binding UpdateContent}">
                    <Rock:Action Command="DisplayGroup">
                        <Rock:ActionParameter Name="GroupId" Value="{{ result.Id }}" />
                    </Rock:Action>
                </Button>
            {% endfor %}
        {% endsearch %}
    </StackLayout>
{% elseif Command == "DisplayGroup" %}
    <!-- Your homework is to fill this in -->
{% endif %}
```

### Lava Events

Let's set the stage because this is going to give some of you heartburn. Lava is designed and meant to be used as a formatting engine. Said another way, it's not meant to generate any content but just format content that already exists. This you probably know. Now, throw that idea out the window. We are going to use Lava in a way it was never meant to be used. You might ask why. In the simplest terms, we needed a scripting engine that you would be familiar with. You are already familiar with doing `{% if %}` checks and other logic with Lava, so we decided to just use what you already know. If this makes you nervous, take an antacid pill.

When you edit a mobile page in Rock, there is a section called `Event Handler`. This is Lava that will be executed when certain events happen on the page. You might be thinking, _so what? I can only format data with Lava_. Oh contraire. Well, normally you would be correct, but we have added a few custom bits to Lava in the mobile application. Let's start with a really simple example. First put a Content block on your page and set the content to be:

```text
<Label Text="{Binding PageValues.Text}" />
```

Now edit your page settings and put this in for the Event Handler:

```text
{% if Device.Orientation == "Landscape" %}
    {% setpagevalue 'Text', 'You are looking at this sideways!' %}
{% else %}
    {% setpagevalue 'Text', 'Good to see you upright.' %}
{% endif %}
```

Once you deploy this you will see the proper text appear depending on which way you are holding your device. So what is going on here? Well, we setup our label to bind the Text value to `PageValues.Text`. The `PageValues` object exists on each page and is essentially a dictionary. You can store whatever values you want in it and access them. Because this object exists at the page level, it is shared between blocks.

Next, in our Event Handler we are checking the device's orientation. Depending on the value we set the `Text` value of the `UsersValue` dictionary to the string it should have.

Okay great, your probably thinking, so I can make text appear when they rotate their phone. What good is that? Okay, how about a more useful example? Let's take a Content Channel Item List block. Suppose you wanted to display the results with a thumbnail image. This might look good on a phone in portrait orientation, but what about if the phone is rotated or if they are on a tablet? Now you have these massive thumbnails. Using the Lava Events we will adjust the number of thumbnails displayed per row. Let's setup the template \(note: we are leaving out the xmlns declarations as they waste a lot of space\):

```text
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
{% if Device.Width > 900 %}
    {% assign columns = 4 %}
{% elsif Device.Width > 600 %}
    {% assign columns = 3 %}
{% else %}
    {% assign columns = 2 %}
{% endif %}
{% setpagevalue 'Columns', columns %}
```

What this will give us is a grid that is 2 columns wide on really narrow screens, 3 columns on medium width screens and 4 columns on really wide screens. And, this happens each time you rotate the device, this isn't just a set once and hope it's correct until they leave the page.

So how do you know why your Event Handler is being executed? There is an `Event` variable defined which will tell you the event that initiated the lava. Currently, there are two pre-defined events:

* `Load`: This event is initiated when the page is loaded, but before any blocks have been instantiated.
* `Resize`: This event is initiated when the available screen space has changed, such as a rotation.

We mentioned above "pre-defined" events. You can initiate your own events with the `LavaEvent`command. The name of the event you wish to trigger is specified by the `CommandParameter`. This means you can setup a button on screen that will trigger a custom Lava event.

