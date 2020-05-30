# Field Stack

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

![](../../../.gitbook/assets/formfieldstack-1.png)

