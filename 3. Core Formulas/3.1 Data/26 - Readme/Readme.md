# Power Apps Data Functions: Detailed Study Notes

> [!NOTE]  
> Power Apps provides a rich set of data manipulation functions for working directly with tables, records, and data sources like Dataverse, SharePoint, SQL, and others. Key functions include `Filter()`, `Search()`, `Sort()`, and `LookUp()`.

## Filter()

- Purpose: Returns a new table containing only records that meet one or more specified conditions.
- Syntax: `Filter(Table, Condition1 [, Condition2, ...])`
- Example: `Filter(Employees, Department = "HR")`
  - Returns only employees in the "HR" department.
- Supports multiple conditions combined with `&&` (AND) or `||` (OR).
- Delegable function, meaning the filtering happens server-side with supported data sources, allowing efficient queries on large data sets.
- Conditions can include comparisons, logical expressions, and the use of columns and variables.

## Search()

- Purpose: Returns a new table comprising records where a specific column contains a provided search string.
- Syntax: `Search(Table, "searchText", "Column1" [, "Column2", ...])`
- Example: `Search(Products, "Laptop", "ProductName")`
  - Returns products with "Laptop" in the ProductName column.
- Useful for implementing search bars where users can type partial text to filter visible records.
- Supports multiple columns for broader search contexts.
- Not fully delegable on all data sources; apply with caution on large lists.

## Sort()

- Purpose: Orders records in a table based on a specific column or expression.
- Syntax: `Sort(Table, ColumnName, SortOrder)`
- SortOrder can be `Ascending` or `Descending`.
- Example: `Sort(Employees, LastName, Ascending)`
  - Displays employees sorted alphabetically by last name.
- Works with both single and multiple columns through nesting or combining with `SortByColumns()`.
- Delegable where supported by the data source.

## LookUp()

- Purpose: Returns the first record in a table that matches one or more criteria.
- Syntax: `LookUp(Table, Condition [, Result])`
- Example: `LookUp(Employees, EmployeeID = 123)`
  - Finds the employee with ID 123.
- Can return the entire record or a specific field by adding the optional `Result` argument.
- Non-delegable in some contexts; best with smaller data sets or when used with indexed columns for performance.

---

## Relationship and Use Cases

| Function  | Use Case Example                          | Returns               |
|-----------|-------------------------------------------|----------------------|
| Filter()  | Show employees in HR department            | Table of records     |
| Search()  | Search products by partial text            | Table of records     |
| Sort()    | Display customers alphabetically           | Sorted table         |
| LookUp()  | Get specific employee details by ID       | Single record or field|

---

## Function Combinations and Advanced Usage

- Combine `Filter()` and `Sort()`  
  Example: `Sort(Filter(Employees, Department = "HR"), LastName, Ascending)`  
  Filters HR employees and sorts by last name.

- Combine `Search()` inside `Filter()` for complex search and filtering  
  Example:  

