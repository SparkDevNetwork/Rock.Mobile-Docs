---
description: Below is tips for styling the mobile shell.
---

# Shell

## Status Bar

The Status Bar is part of the mobile operation that shows the time and status of things like the battery, cellular signal and battery. Your application doesn't have much control of this area of the display, but you can set the color of foreground color using the CSS below.

![](../.gitbook/assets/image.png)

```text
NavigationPage {
  -rock-status-bar-text: light;
}
```

_\(Valid values: light,dark\)_

## Navigation Bar

The Navigation Bar is the top of header of your application. It typically holds the header image and any content you want on the right side.

![](../.gitbook/assets/image%20%288%29.png)

You can style this bar with the following CSS.

```text
.navigation-bar {
    -xf-bar-background-color: #9acced;
    -xf-bar-text-color: #ffffff;
}
```

## Tab Bar

The Tab Bar only displays when the shell is set to Tab mode. 

![](../.gitbook/assets/image%20%281%29.png)

You can style this bar with the following CSS.

```text
.tab-bar {
    -xf-bar-background-color: #9acced;
    -xf-bar-text-color: #ffffff;
}
```

