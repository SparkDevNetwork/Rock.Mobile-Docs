---
description: Working with a design system is better that working against it.
---

# Helper Classes

Below are some helper classes to make styling your application quicker and more consistent.

## Misc Helpers

### Separators

Often you'll want to add separators between elements \(think and `<hr>` tag in HTML\). To do this use a `BoxView` with a `StyleClass` of `separator`.

```text
<BoxView StyleClass="separator" />
```

Not thick enough? There are two additional thicker options available. These are used in conjunction with the above class \(e.g. `.StyleClass="separator, separator-thick"`\). 

| Selector | Result |
| :--- | :--- |
| .separator-thick | Line is height of 2. |
| .separator-thickest | Line is a height of 4. |

