---
description: 'We all have to start somewhere, so why not start at the beginning.'
---

# Fundamentals

## Important Note For Developers

Adding a new block type into the mobile application itself is not supported at this time. It has been discussed but there are various issues that come up with making changes to the mobile application, even for the core team. Therefore at the moment pull requests will not be supported to the mobile application itself. Though if you have ideas bring them up, it's not that we don't want to add new features, they are just more complex to implement as it requires synchronizing the rollout between your users and your server. That said there are various ways to add new functionality to the mobile application without having to modify the application itself.

First, you can re-use existing block types and simply provide different configuration options. While some block types that wouldn't do much, for example a Workflow Entry block, others allow you great flexibility depending on what configuration you send them. Two blocks that excel at customization are the `Content` and `Dynamic Content` block types. Before we get to those, lets talk about some commands that are available to all blocks.

## Pages and Blocks

First, the content in the app is provided via pages and blocks from your Rock server. Each Rock page represents a page in your mobile application and the blocks on that Rock page define the content that is displayed on the page. A page can contain either a single block or multiple blocks. For example, you might have a block that provides a banner image at the top and then a second block that shows a scrolling list of content channel items.

Because a block that provides information for a mobile application does not have a web presence, a new block type was created. Part of the reason for this was performance. It takes a long time to instantiate a regular `RockBlock` because we have to initialize the entire page and ASP.NET stack to do it properly. For these reasons, a new base block type was created: `IRockBlockType`. This interface provides the minimal required bits and pieces in order to have a block exist in the system. There is also a base class that provides a a number of helper methods for your blocks: `RockBlockType`.

The next piece of the block puzzle is a new interface that defines that an `IRockBlockType` can be used for mobile applications: `IRockMobileBlockType`. This defines a few properties and methods that must exist for a block to be able to provide the required information to a mobile application:

* RequiredMobileAbiVersion - This defines the minimum "Abi" \(no that's not a typo\) version that the mobile application must support in order for the block to function correctly. For example, if we add a new supported block type in the mobile app, this number would increase.
* MobileBlockType - This instructs the mobile application which class to instantiate to handle displaying and interacting with the block data.
* GetMobileConfigurationValues\(\) - Returns a customized object that is JSON encoded and passed to an instance of the MobileBlockType above. This allows you to return a custom set of configuration values based on the block's settings. For example, you might have a block setting where the user selects a Defined Value. The value is stored as a Guid which isn't too helpful for display purposes, so you might use this method to send over the display value of the Defined Value instead.

### Block Actions

Another aspect of these new block types is that they support their own API actions. This allows your block to respond to API requests without having to build a new API endpoint. A block's action has a name and set of parameters. The action name does not have to be unique in the system.

In the context of a mobile application, you might have a block that displays a large set of data but you only want to load 5 items at a time. Instead of creating a new API endpoint to deal with loading those next pages of items, you simply define an action on your block type that takes a parameter of "page number" and returns the set of 5 items for that page.

A block type can have any number of actions, each taking their own set of parameters.

### Custom Settings

Most blocks have a pretty simple configuration UI and can just use the standard Attribute Values configuration view. A few blocks might need a more custom UI to handle dynamically showing and hiding elements based on the user's selection. Don't worry, we've got you covered. There is a way to build a custom UI that is displayed in the Block Settings popup in either pure C\# code or with an .ascx file.

A custom setting UI is declared by creating a subclass of `RockCustomSettingsProvider` or `RockCustomSettingsUserControlProvider`. The former allows you to build the UI in pure C\# while the latter uses an `ascx` user control to build the UI. Whichever class you inherit from, you would then add a `[TargetType( typeof( nnn ) )]` decorator on your subclass. This `TargetType` identifies the class type that it targets. In other words, the block type class you are building a custom settings UI for.

You can define more than one custom settings UI for your block by creating multiple sub-classes. Each implementation specifies the title used on the tab the UI is housed inside. This means if you have a rather complex UI that would be easier in multiple tabs, you can do just that \(though they cannot interact with each other\).

As a bonus, if **all** of your settings are custom settings, meaning the `Basic Settings` tab would normally be empty, you can define the title of your custom settings tab to be `Basic Settings` and it will put placed in the `Basic Settings` tab rather than a new tab.

