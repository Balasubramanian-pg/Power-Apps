# Controls in Power Apps: Detailed Study Notes

## What are Controls?

- Controls are UI elements that users interact with in a Power Apps app.
- They display data, collect user input, navigate, or trigger actions.
- Examples include TextInput, Button, Gallery, Form, Label, Dropdown, Slider, Toggle, and many more.
- Controls have numerous customizable properties like Color, Size, Font, Visibility, and OnSelect behavior.

## Common Control Types and Their Functions

### TextInput
- Allows users to enter or edit text data.
- Key properties:
  - Text: the content entered.
  - HintText: placeholder text shown when empty.
  - Mode: single line, multiline, or password.
  - Default: initial value.
- Used in forms, search bars, or any user input scenario.

### Button
- Clickable control that triggers actions.
- Key properties:
  - OnSelect: formula or function executed on click.
  - Text: label displayed on the button.
  - DisplayMode: enabled, disabled, or view-only.
- Supports different styles like Standard or Primary.

### Gallery
- Displays a list or collection of items (data rows).
- Supports vertical, horizontal, flexible height, and wrap layouts.
- Key properties:
  - Items: source of data.
  - TemplateSize: controls the size of each item.
  - Selected: currently selected item in the gallery.
- Used to build menus, product lists, or complex data views.

### Form (Edit Form and Display Form)
- Used to show or edit records from a data source.
- Edit Form allows input and data submission, Display Form shows read-only data.
- Key properties:
  - DataSource: linked data source.
  - Item: specific record to display or edit.
  - Mode: Edit, Display, or New.
- Supports validation, error handling, and submit/reset commands.

## Important Additional Controls

### Label
- Displays static or dynamic text.
- Commonly used for instructions, field labels, or feedback messages.
- Properties: Text, Color, Font, Align, Visible, etc.

### Dropdown
- Lets users select one value from a predefined list.
- Key properties: Items (choices), Selected, Default.

### Checkbox and Toggle
- Boolean input controls.
- Checkbox shows a box to check/uncheck; Toggle is a switch style.
- Properties: Checked (true/false), OnCheck, OnUncheck.

### Slider
- Enables selection of numeric values by dragging a handle.
- Properties: Min, Max, Value, Step.

### Date Picker
- Allows user to select dates from a calendar popup.
- Properties: SelectedDate, MinDate, MaxDate.

## Control Properties Categories

- **Appearance:** Color, Font, Border, Fill, Size, Alignment.
- **Behavior:** Visibility, DisplayMode, OnSelect, OnChange events.
- **Data:** Items (source), Default values, Selected items.
- **Layout:** X, Y position, Width, Height.
- **Accessibility:** Tooltip, Label, Focus.

## Control Events and Formulas

- Controls respond to user interaction through events like:
  - OnSelect (button click, gallery item select)
  - OnChange (input changed)
  - OnCheck/OnUncheck (checkbox/toggle)
- Formulas in properties define dynamic behavior, e.g., visibility, navigation, data updates.

## Fluent UI Controls

- Microsoft provides Fluent UI controls for Power Apps aligned with Microsoft Teams design.
- Examples include new versions of Button, Check box, Combo box, Date picker, Label, Radio group, Slider, Text box, and Toggle.
- Provide consistent look and feel with Teams and modern Microsoft interface.

## Best Practices for Controls

- Name controls descriptively (e.g., `txtName`, `btnSubmit`) for easier maintenance.
- Use consistent fonts, colors, and sizes for UI harmony.
- Group related controls inside containers or screens for clearer structure.
- Always configure accessibility properties like tooltips and labels.
- Test controls on different devices for responsiveness.
- Optimize performance by minimizing complex formulas in visible controls.

## Flashcards: Q&A

- Q: Which control is used for user text input?  
  A: TextInput.

- Q: How do you trigger an action on a button click?  
  A: Set the OnSelect property with the desired formula.

- Q: What control displays a list of items?  
  A: Gallery.

- Q: What is the difference between Edit Form and Display Form?  
  A: Edit Form allows editing and submitting data; Display Form shows read-only details.

- Q: How can you let users pick a date?  
  A: Use the Date Picker control.

- Q: Which control is used for boolean (true/false) input in a modern UI?  
  A: Toggle or Checkbox.

- Q: Name a property that defines what happens when a control is selected.  
  A: OnSelect.

## Summary

Controls are the fundamental building blocks of Power Apps user interfaces. Understanding the various types, their key properties, and how they interact via events enables building rich, responsive, user-friendly applications.
