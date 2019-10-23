---
description: >-
  This section will provide a reference to the lava functionalities available to
  you and in what context they are available.
---

# Lava

## Lava Filters

The [Community](https://community.rockrms.com/lava) page has a list of all the filters that are available. If a filter is available on the Mobile application it will be marked as such.

## Shell Lava Variables

The following variables and properties are available in all Lava contexts when executing Lava on the mobile application shell.

* PageParameter
* CurrentPerson
* Device
* PageValues

### CurrentPerson

The `CurrentPerson` object that is available on the mobile shell is a stripped down version of what you are used to using on the Server side. It does not support all the same properties, but most of the ones you are most likely to use should be available.

| Property | Description |
| :--- | :--- |
| PersonAliasId | The primary person alias identifier associated with this person. |
| PersonId | The person identifier that matches up with the `Person.Id` you would use on the server. |
| FirstName | The given name of the person. |
| NickName | The name the person usually is addresses by. |
| LastName | The family name of the person. |
| Gender | The person's gender, if known. |
| Email | The primary e-mail address of the person. |
| BirthDate | The date the person was born, if known. |
| PhotoUrl | A url that can be used to display the person's profile image. If no image is available then this will be an empty string. |
| MobilePhone | The mobile phone number associated with this person. |
| HomePhone | The home phone number associated with this person. |
| HomeAddress | The physical address associated with this person. |
| CampusGuid | The GUID that identifies this person's primary campus. |

### Device

The Device variable gives you access to the type of device in use by the user. At present the following fields are available.

| Property | Description |
| :--- | :--- |
| DeviceType | The type of device the shell is running on: `Phone`, `Tablet`, `Watch`, `Unknown`. |
| Manufacturer | The manufacturer of the device \(e.g. `Apple`\). |
| Model | The model of the device \(e.g. `iPhone 7`\). |
| Name | The name of the device. Note: this is often not a real device name as that is considered confidential information by Apple. |
| VersionString | The application version number of the Shell. |
| DevicePlatform | The platform the shell is running on: `iOS`, `Android`, `Unknown`. |
| Orientation | The current orientation of the device: `Unknown`, `Portrait`, `Landscape`. |
| Width | The width of screen \(in relation to current orientation\) in pixels. |
| Height | The height of screen \(in relation to current orientation\) in pixels. |
| Density | The pixel density of the screen \(e.g. on an iPhone 7 this would be `2`\). |

### PageValues

Page values are used along with Page Events to allow you to customize your user experience. The `PageValues` object is just a dictionary of string/object pairs that you can use any way you want. There is no defined structure to them. You can read more about their use in the [Page Events](advanced-dynamic-content.md#Page-Events) section.

## Server Lava Variables

When you are using Lava to customize the experience from the server side, for example in a Content block set to server-side Lava rendering, the following variables are available to you.

* PageParameter
* CurrentPerson
* [Device](lava.md#Device)

In addition, the Content block defines these additional variables when processing a callback event. You can check out the [Advanced: Dynamic Content](advanced-dynamic-content.md) page for more details on their contents.

* Command
* Parameters

## Lava Commands

The mobile application provides a few additional Lava commands that you can use in different circumstances.

#### setpagevalue

This command allows you to set a specific value in the [PageValues](lava.md#PageValues) dictionary.

```text
{% setpagevalue key, value %}
```

Both `key` and `value` can either be specified as literal values or as variable references, so both of the following are functionally the same.

```text
{% assign name = 'MaxLength' %}
{% assign max = 100 %}
{% setpagevalue name, max %}
```

```text
{% setpagevalue 'MaxLength', 100 %}
```

You can then access your PageValue by simply accessing it like any other Lava object, for example:

```text
{% assign oldMax = UserValues.MaxLength %}
```

