# Tips and Tricks

## **Spacing**

Be aware that many layouts have default spacing parameters. If things don't line up correctly you may have to remove this default spacing. 

For example, StackLayouts have a default Spacing property set to 10. This means each control within the layout will have a 10 unit space between it and it's neighbor.  For a StackLayout the spacing will be relative to it's Orientation property. When set to Horizontal it will be horizontal spacing, Vertical would be vertical spacing. 

Other layout controls also have this same behavior. Below is a list of others \(this is not meant to be an inclusive list\).

* Grids - Have RowSpacing and ColumnSpacing properties
* ResponsiveLayouts - Have a ColumnSpacing property

## **Device Type Customization**

As you assign values to properties you may want to have different values for a phone vs a tablet. This is often the case when working with the sizes of images and cards \(via the Ratio properties\). Providing different properties is possible using the syntax below.

```text
<Label 
    Text="{Rock:OnDeviceType Phone='I am a phone!', Tablet='I am a tablet!'}" 
    Margin="{Rock:OnDeviceType Phone='20, 20, 20, 20', Tablet='0, 0, 0, 0' }" />
    
<Image Source="https://yourserver.com/photo.jpg" 
    Ratio="{Rock:OnDeviceType Phone=4:3, Tablet=4:2}" />
```

If you'd like to change more than just a property, and instead provide different markup, you can use the controls below.

```text
 <Rock:OnDeviceType>
    <Rock:OnDeviceType.Phone>
        <BoxView Color="Red" HeightRequest="10" />
    </Rock:OnDeviceType.Phone>
    <Rock:OnDeviceType.Tablet>
        <BoxView Color="Blue" HeightRequest="10" />
    </Rock:OnDeviceType.Tablet>
</Rock:OnDeviceType>
```

## **Device Platform Customization**

As you assign values to properties you may want to have different values by platform \(iOS or Android\).  Providing different properties is possible using the syntax below.

```text
<Label Text="{Rock:OnDevicePlatform Android='Droid', iOS='Apple'}" />
```

If you'd like to change more than just a property, and instead provide different markup, you can use the controls below.

```text
 <Rock:OnDeviceType>
    <Rock:OnDeviceType.Phone>
        <BoxView Color="Red" HeightRequest="10" />
    </Rock:OnDeviceType.Phone>
    <Rock:OnDeviceType.Tablet>
        <BoxView Color="Blue" HeightRequest="10" />
    </Rock:OnDeviceType.Tablet>
</Rock:OnDeviceType>
```

## **StringFormat Data Binding**

From time to time, you’ll find yourself needing to Bind data as part of a string. You may be tempted to just throw your Binding in the middle of the string as you would in Lava, but you’ll be disappointed as nothing will show up. This is where StringFormat comes into play.

```text
<Label Text="{Binding Name, StringFormat='Hey there {0}!'}" />

```

In this case the Key we are Binding to is Name and we want it to output “Hey there Ted!”. We use {0} to insert our Binding Value into the string where we want it to show up.

