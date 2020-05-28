# NotificationBox

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

![](../../.gitbook/assets/notificationbox-1.png)

### 

