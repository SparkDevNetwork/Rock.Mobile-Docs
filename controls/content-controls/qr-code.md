# QR Code

Ever wish you could display a QR Code in your application based on something generated on the server? Okay maybe this isn't a common thing just yet, but we think QR Codes will become a great way to pass information without having to make direct contact.

QR Codes allow you to store lots of information. But the more information you store, the more complex they become. The more complex they become the more difficult they are to scan. That isn't to say you can't scan them, but think of it like the old Where's Waldo books. Imagine you had two Waldo pages. One page had Waldo standing all by himself in an open field. It only takes a quick glance to know where he is. Now imagine your standard "complex" Waldo page. It's not hard to find him because he is obscured and barely visible, it's just because there is a lot of clutter that looks similar around him.

The same is true of QR Code complexity. A simple code can be scanned without trouble. A complex code might take a bit more finesse on the scanner to get things lined up properly. So the moral of the Waldo story is test your codes. It isn't just the complexity, but the size and color choices also play a role. So test your code at the "most complex" it could be and verify it will scan easily. Otherwise you might need to adjust size and color \(assuming you can't make it less complex\) so it's easier to scan.

## Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| BackgroundColor | Color | The background color behind the QR Code. By default this is transparent so whatever background color your page is will be used. |
| Color | Color | The color of the foreground blocks in the QR Code. _Defaults to Black._ |
| Level | [ECCLevel](qr-code.md#ecclevel) | The ECC Level to be applied to the generated QR code. _Defaults to M_. |
| Content | string | The actual content to be encoded into the QR Code. |

### ECCLevel

ECC Level or Error Correcting Code Level, defines how much of the QR Code can be damaged before it can no longer be scanned. A higher ECC Level means a more complex image, so you shouldn't plan on just using the highest ECC Level and calling it a day. Since we are displaying on a screen, the chance of damage is near zero \(unless your finger is in the way\).

| Value | Description |
| :--- | :--- |
| L | Allows for up to 7% damage. |
| M | Allows for up to 15% damage. |
| Q | Allows for up to 25% damage. |
| H | Allows for up to 30% damage. |

## Example

```markup
<Rock:QRCode Content="{{ CurrentPerson.Guid }}" />
```

In this example, we are assuming the user is logged in and that we are processing Lava on the server. This would generate a QR Code on screen whose scanned content would contain the person's unique identifier. Later, you could use this with another application to scan the QR Code and know who the person is. Another option would be to encode something like the Guid value of an Event Registration Registrant. Then when scanned you could display the details of their registration.

