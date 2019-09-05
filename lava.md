---
description: >-
  This section will provide a reference to the lava functionalities available to
  you and in what context they are available.
---

# Lava

## Lava Filters

The [Community](https://community.rockrms.com/lava) page has a list of all the filters that are available. If a filter is available on the Mobile application it will be marked as such.

## Lava Variables

The following variables and properties are available in all Lava contexts:

* CurrentPerson
  * PersonAliasId
  * PersonId
  * FirstName
  * LastName
  * Gender
  * Email
  * BirthDate
  * PhotoUrl
  * MobilePhone
  * HomePhone
  * CampusGuid
  * AttributeValues
* Device
  * DeviceType - Phone, Tablet, Watch, Unknown
  * Manufacturer
  * Model
  * Name
  * VersionString
  * DevicePlatform - iOS, Android, Unknown
  * Orientation - Unknown, Portrait, Landscape
  * Width - Width of screen \(in relation to current orientation\) in pixels
  * Height - Height of screen \(in relation to current orientation\) in pixels.

When the Lava is executing in the context of a page, the following is also available:

* UserValues

When the Lava is executing in the context of a block, the following is also available in addition to those on the page:

* AttributeValues - Mobile Local Settings block settings defined on the block.

## Lava Commands

The mobile application provides a few additional Lava commands that you can use in different circumstances.

#### setuservalue

This command allows you to set a specific value in the UserValues dictionary. The syntax is `{% setuservalue key, value %}`. Both `key` and `value` can either be specified as literal values or as variable references, so both of the following are functionally the same:

```text
{% assign name = 'MaxLength' %}
{% assign max = 100 %}
{% setuservalue name, max %}
```

```text
{% setuservalue 'MaxLength', 100 %}
```

UserValues are a completely customizable dictionary whose contents is entirely up to you. There is no predefined meaning to any specific keys, so it is up to you to decide how to use them. You can access a current UserValue by simply accessing it like any other Lava object, for example:

```text
{% assign oldMax = UserValues.MaxLength %}
```

This allows you to track information between Event Handler runs.

