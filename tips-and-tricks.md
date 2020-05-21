# Advanced Topics

## **Static Resources - Dynamic JSON**

When working with different DataViews you have the ability to create a ItemSource directly within the page your DataView lives on. In order to create a consumable object, you’ll want to create a JSON object from your dynamic content, such as a Content Channel. Since you are creating the JSON object structure directly, there is not need to use the \| TOJSON command on the item within the Rock:Command. You will however still need the \| XamlWrap command.

```text
{% contentchannelitem where:'ContentChannelId == 21 && StartDateTime <= "{{ 'Now' | Date }}"' sort:'StartDateTime desc' securityenabled:'false' %}
 
{% assign closeCurly = '}' -%}
{%- capture seriesJSON -%}
    [
        {%- for item in contentchannelitemItems -%}
        {
            "Id": {{ item.Id | ToJSON }}, 
            "Title": {{ item.Title | ToJSON }}, 
            "Summary": {{ item | Attribute:'Summary' | ToJSON }}, 
            "ImageId": {{ item | Attribute:'SeriesImage','RawValue' | ToJSON }}, 
            "MessageCount": {{ item.ChildItems | Size | ToJSON }} 
        {% if forloop.last %}{{ closeCurly -}}{% else %}{{ closeCurly | Append:', ' -}}{%- endif -%}
        {%- endfor -%}
    ]
{%- endcapture -%} 

{% endcontentchannelitem %}
<StackLayout.Resources>
    <Rock:FromJson x:Key="Series">
        {}{{ seriesJSON | XamlWrap }}
    </Rock:FromJson>
</StackLayout.Resources>
```

This must be placed on the page ABOVE the View where you need to consume it. Then you’ll be able to access it as a StaticResource.

```text
<CarouselView ItemsSource="{StaticResource Series}">
```

From here you would just simply Bind the Key in the item where you would like the data to show.

```text
<Label
    StyleClass="heading1"   
    VerticalTextAlignment="Center"
    Text="{Binding Title}" />  
```

## **Static Resources - DataTemplate**

In addition to Dynamic JSON data, it is possible to create a DataTemplate that can be consumed in multiple areas of your layout by defining it as a Static Resource somewhere on the page ABOVE the area where you need to use it.  


```text
<StackLayout.Resources>
<DataTemplate x:Key="CarouselTemplate"
    <StackLayout WidthRequest="180">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="2*"/>
                <ColumnDefinition Width="1*"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <Rock:StyledView
                CornerRadius="15"
                HasShadow="True"
                Elevation="1"
                Padding="0"
                Grid.Row="0"
                Grid.Column="0"
                Grid.ColumnSpan="2"
                IsClippedToBounds="True">
                <Rock:Image MarginBottom="-64" HeightRequest="220" Source="{Binding ImageId, StringFormat='{{ 'Global' | Attribute:'PublicApplicationRoot' }}/GetImage.ashx?Guid={0}'}" Aspect="AspectFill" VerticalOptions="CenterAndExpand"/>
            </Rock:StyledView>
            <Label Text="{Binding Title}" Grid.Row="1" Grid.Column="0" StyleClass="heading4, leading-none, text-bold"/>
            <Label Text="Jan '20" MarginTop="3" HorizontalTextAlignment="Right" Grid.Row="1" Grid.Column="1" StyleClass="heading4, leading-none"/>
        </Grid>
    </StackLayout>
</DataTemplate>
</StackLayout.Resources>
```

Once this has been defined, it can then be consumed as an ItemTemplate by a View, or multiple Views. Again, this must be declared higher up the page than any Views which will be using it on page.

```text
<CarouselView 
    ItemsSource="{StaticResource Series}" 
    ItemTemplate="{StaticResource CarouselTemplate}"
    HeightRequest="250"
    PeekAreaInsets="20,0,40,0">   
```

  
  


