# Tags

Tags are amazing as they come, but you can style them with just a bit of CSS.

## Tag Selectors

Tags can be selected in CSS using the `.tag` class. There are several additional hooks available to limit your selections.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Selector</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">.tag-sm</td>
      <td style="text-align:left">Selects only small tags.</td>
    </tr>
    <tr>
      <td style="text-align:left">.tag-md</td>
      <td style="text-align:left">Selects the default sized tags.</td>
    </tr>
    <tr>
      <td style="text-align:left">.tag-lg</td>
      <td style="text-align:left">Selects only large tags.</td>
    </tr>
    <tr>
      <td style="text-align:left">.tag-[type]</td>
      <td style="text-align:left">
        <p>Selects tags by their type. Options include:</p>
        <p>.tag-primary</p>
        <p>.tag-secondary</p>
        <p>.tag-success</p>
        <p>.tag-info</p>
        <p>.tag-warning</p>
        <p>.tag-danger</p>
      </td>
    </tr>
  </tbody>
</table>### Example

```text
.tag.tag-sm.tag-info {
    /* would select small tags of type info */
}
```

### Styling the Tag's Text

To style the text of the tag use the `.tag-text` child selector.

```text
.tag .tag-text {
    color: #ee7625;
}
```

