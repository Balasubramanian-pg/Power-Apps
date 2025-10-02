# Power Apps Formulas: Detailed Study Notes

> [!NOTE]  
> Power Apps formulas use Power Fx, a language similar to Excel formulas, to create dynamic app behavior and data manipulation.

## What Are Formulas?

- Formulas are expressions that calculate values, respond to user inputs, and control app logic.
- Used to:
  - Calculate or transform data (e.g., `Sum()`, `Len()`).
  - Filter or search data collections (e.g., `Filter()`, `Search()`).
  - Update or patch data sources (e.g., `Patch()`).
  - Control UI elements' visibility, color, or navigation.
- No need to prefix formulas with `=` like Excel; writing a formula directly is enough.

## Common Formula Functions and Examples

### Filter()

- Returns a subset of data matching specific criteria.
- Example: `Filter(Employees, Department = "Sales")`
- Filters records in a data source where Department equals "Sales".

### Search()

- Searches a table for records containing a specific text string.
- Example: `Search(Products, "Laptop", "ProductName")`
- Finds all products with "Laptop" in the ProductName column.

### Patch()

- Updates or creates a record in a data source.
- Example:  
