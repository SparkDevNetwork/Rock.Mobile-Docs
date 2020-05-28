# XAML Styling

Below are various styling rules we recommend when writing XAML.

1. When using a StyleClass place it as the first parameter when defining Labels, or when the CSS class name helps define the intent of the control.   Good: `<Label StyleClass="h1" Text="My Title" />` Bad: `<Label Text="My Title" StyleClass="h1" />` 
2. When using a control \(like a Label\) that does not need a body do not provide an end tag.  Good: `<Label Text="My Label" />` Bad: `<Label Text="My Label"></Label>` 
3. ...

