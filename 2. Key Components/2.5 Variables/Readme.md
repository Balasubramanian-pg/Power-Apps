# Variables in Power Apps: Detailed Study Notes

> [!NOTE]  
> Variables store temporary data in Power Apps and are essential for managing app state, user inputs, and UI behavior.

## Types of Variables in Power Apps

### 1. Global Variables
- Accessible across the entire app, all screens.
- Created and updated using the `Set()` function.
- Can hold types like number, text, boolean, record, table.
- Example: `Set(varUserName, "John Doe")`
- Used to share data, states, or configuration across multiple screens.
- Global variables do not persist after the app is closed.

### 2. Context Variables (Local Variables)
- Scoped to a single screen only.
- Created and updated using `UpdateContext()` function.
- Used to control data or UI states local to one screen.
- Example: `UpdateContext({varIsVisible: true})`
- Useful for managing pop-ups, toggles, or screen-specific states.
- Cannot be accessed outside the screen where defined.

### 3. Collections (Table Variables)
- Hold tables (sets of records) and are usable app-wide.
- Created using `Collect()` or `ClearCollect()` functions.
- Use when working with multiple rows of data or caching data locally.
- Example: `Collect(colEmployees, {Name:"John", Age:30})`
- Can be saved to local device for offline use.
- Collections function like in-memory databases during app runtime.

## Creating and Using Variables

- Variables are created implicitly when you use `Set()`, `UpdateContext()`, `Collect()`, or `ClearCollect()`.
- Example:  
  - Global: `Set(varCount, 0)`  
  - Context: `UpdateContext({varCount: 0})`  
  - Collection: `ClearCollect(colOrders, OrdersTable)`
- Variable type is defined by the value assigned. Once a type is set, it must remain consistent.
- Variables are referenced by their name in formulas and control properties.

## Comparison: Global vs Context Variables

| Aspect           | Global Variables                  | Context Variables                  |
|------------------|---------------------------------|----------------------------------|
| Scope            | Entire app                      | Single screen                    |
| Creation         | `Set(varName, value)`            | `UpdateContext({varName: value})`|
| Use Case         | Cross-screen data sharing       | Screen-specific states           |
| Persistence      | Exists until app is closed       | Exists only on the screen        |
| Accessibility    | Anywhere in the app              | Only within its screen           |

## One-Time Variables: `With()` Function

- Temporary variables used inside formulas for calculations without storing state.
- Scope limited to the formula block.
- Example:  
  ```
  With({tempVar: 10}, tempVar * 2)
  ```
- Useful for simplifying complex expressions and improving readability.

## Practical Examples

- Set a global variable on button click:  
  `Set(varUserName, TextInput1.Text)`
- Show/hide a popup on a screen using context variable:  
  `UpdateContext({showPopup: true})`  
  Control Visible property: `showPopup`
- Load a collection with data from a SharePoint list:  
  `ClearCollect(colProjects, SharePointList)`
- Increment a global counter:  
  `Set(varCount, varCount + 1)`

## Best Practices

- Use global variables sparingly as they consume more memory.
- Prefer context variables for UI-related, screen-specific data for better performance.
- Maintain consistent variable naming conventions, e.g., prefix with `gbl` for global, `ctx` for context.
- Reset variables when no longer needed to free resources.
- Use collections when working with tabular data rather than ad-hoc global variables.

## Flashcards: Q&A

- Q: How do you create a global variable named `varName` with value 10?  
  A: `Set(varName, 10)`

- Q: Which function creates a context variable?  
  A: `UpdateContext()`

- Q: What type of variable holds tables?  
  A: Collections (created with `Collect()` or `ClearCollect()`)

- Q: Can a context variable be accessed across multiple screens?  
  A: No, context variables are limited to one screen.

- Q: How can you declare a temporary variable inside a formula?  
  A: Using the `With()` function.


These notes provide a thorough overview of how to create, use, and manage variables in Power Apps for building dynamic apps.

[1](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/working-with-variables)
[2](https://www.powerapps911.com/post/the-different-types-of-power-apps-variables)
[3](https://www.youtube.com/watch?v=AfuG4mLPaJw)
[4](https://www.c-sharpcorner.com/article/local-and-global-variables-in-power-apps/)
[5](https://mindmajix.com/powerapps-variables)
[6](https://www.linkedin.com/pulse/power-platform-beginners-guide-using-collections-essandoh-crudf)
[7](https://www.matthewdevaney.com/power-apps-coding-standards-for-canvas-apps/power-apps-standards-variable-types/)
[8](https://www.c-sharpcorner.com/article/variables-in-powerapps/)
[9](https://learn.microsoft.com/en-us/power-platform/power-fx/data-types)
