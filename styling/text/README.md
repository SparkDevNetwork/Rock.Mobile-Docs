---
description: >-
  Below is information on how to style text in your application. Several utility
  classes are provided to help you easily style your text in a way that is
  consistent and easy to maintain.
---

# Text

## Custom Properties for Text

Rock Mobile adds some custom CSS properties that are not a part of Xamarin Forms.

### Text Shadow

If you would like to add shadows to text in Label elements. You can do so like this:

```text
.heading1 {
    -rock-text-shadow: 6,7, 2, #000000;
}
```

The values represent \(in order\):

1. Distance X
2. Distance Y
3. Blur Radius

