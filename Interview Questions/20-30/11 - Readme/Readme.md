### **21. What is the difference between If() and Switch()?**

*   **`If()` Function:**
    *   **Purpose:** Used to test **one or more sequential conditions** and return different results based on which condition is true.
    *   **Structure:** It follows an `If-Then-Else If-Else` logic.
    *   **Syntax:** `If( Condition, ResultIfTrue, [ResultIfFalse], [Condition2, ResultIfTrue2, ...] )`
    *   **Best For:** Scenarios with **1-3 logical branches** or when conditions are completely different from one another.
    *   **Example:**
        ```powerfx
        If( Rating >= 4.5,
            "Excellent",
            Rating >= 3.5,
            "Good",
            Rating >= 2.5,
            "Average",
            "Poor"
        )
        ```

*   **`Switch()` Function:**
    *   **Purpose:** Used to test a **single expression or value** against a list of possible matches. It's cleaner and more efficient for multiple, discrete comparisons.
    *   **Structure:** It follows a "match this value to one of these cases" logic.
    *   **Syntax:** `Switch( Expression, Match1, Result1, Match2, Result2, ..., [DefaultResult] )`
    *   **Best For:** Scenarios where you are checking the **same field or expression for multiple specific values**.
    *   **Example:**
        ```powerfx
        Switch( Status.Value,
            "New", "🟡 Pending",
            "In Progress", "🔵 Active",
            "Closed", "🟢 Complete",
            "❓ Unknown"
        )
        ```

### **22. How do you use IsBlank() to check for empty values?**

*   **Purpose:** The **`IsBlank()`** function checks if a value is **`blank`** (i.e., null, empty, or undefined). This is the primary way to test for empty controls or optional data source fields.
*   **Returns:** **`true`** if the value is blank, otherwise **`false`**.
*   **Common Use Cases:**
    *   **Form Validation:** `If( IsBlank(TextInput1.Text), Notify("Please enter a name", Error), SubmitForm(Form1) )` - Prevents submission if a field is empty.
    *   **Setting Default Values:** `If( IsBlank(LocationField), "Not Specified", LocationField )` - Displays a friendly default if a field is empty.
    *   **Conditional Formatting:** Set the `Color` property of a label to `If( IsBlank(ThisItem.DueDate), Green, Red )` to highlight overdue items (where a blank due date means it's not due).
    *   **Checking for New Records:** In a form, `IsBlank(Form1.LastSubmit)` can be used to check if a record is new (has never been submitted).

### **23. What does the CountRows() function do?**

*   **Purpose:** The **`CountRows()`** function counts the number of **records (rows)** in a table (data source, collection, or filtered set).
*   **Syntax:** `CountRows( Table )`
*   **Common Use Cases:**
    *   **Displaying Record Count:** Set a **Label's** `Text` property to `"Total Items: " & CountRows(MyGallery.Items)` to show how many items are in a filtered gallery.
    *   **Checking if a Collection has Data:** `If( CountRows(myCollection) > 0, Navigate(NextScreen), Notify("No data") )` - Navigates only if the collection is populated.
    *   **Counting Related Items:** `CountRows( Filter( Issues, Issues.AssignedTo = ThisUser.Email ) )` - Counts how many issues are assigned to the current user.

### **24. How do you concatenate strings using Concatenate() or &?**

*   **Using the `&` Operator:**
    *   This is the **most common and concise method** for string concatenation.
    *   It seamlessly joins text strings with values from controls, variables, and data fields.
    *   **Examples:**
        *   `"Hello, " & User().FullName & "!"`
        *   `"Name: " & TextInput1.Text & " - Department: " & Dropdown1.Selected.Value`
        *   `ThisItem.FirstName & " " & ThisItem.LastName`

*   **Using the `Concatenate()` Function:**
    *   This function takes a **single table** of values to concatenate. It is less flexible for mixing static text and dynamic values in a simple formula.
    *   **Syntax:** `Concatenate( TableOfStrings, [Delimiter] )`
    *   **Primary Use Case:** It's most useful when you have a **column of data in a table** that you want to combine.
    *   **Example:** `Concatenate( Gallery1.AllItems, FullName, ", " )` would take all the `FullName` values from the gallery and join them into a single string, separated by commas.

*   **Conclusion:** For most day-to-day concatenation needs (building sentences, labels, etc.), **the `&` operator is the preferred and simpler choice.**

### **25. What is the purpose of the Set() function?**

*   **Purpose:** The **`Set()`** function is used to create or update a **global variable**. Global variables are accessible from anywhere within the app (across all screens).
*   **Syntax:** `Set( VariableName, Value )`
*   **Key Characteristics:**
    *   **Scope:** **Global** (app-level).
    *   **Persistence:** The value persists while the app is running but is **lost when the app is closed**.
    *   **Common Use Cases:**
        *   **Storing User Selections:** `Set( varSelectedCustomer, Gallery1.Selected )` - Store a selected record for use on another screen.
        *   **Holding Calculated Values:** `Set( varTotalCost, Slider1.Value * 1.1 )` - Calculate and store a value for later use.
        *   **Tracking App State:** `Set( varIsEditing, true )` - Use a variable to show/hide controls or change modes.
    *   **How to Use:** Typically called from the `OnSelect` property of a button or the `OnStart` property of the app.

### **26. How do you use ForAll() to iterate over a collection?**

*   **Purpose:** The **`ForAll()`** function evaluates a formula for **all the records in a table**. It is used to perform an action on each record individually.
*   **Syntax:** `ForAll( Table, Formula )`
*   **Important Behavior:** It does not return a table that can be directly displayed in a gallery. It's used for its *side effects*, like patching data or collecting results.
*   **Common Use Cases:**
    *   **Updating Multiple Records:**
        ```powerfx
        ForAll( CheckedItemsGallery.AllItems,
            If( ThisItem.IsChecked,
                Patch( SharePointList, ThisItem, { Status: "Approved" } )
            )
        )
        ```
    *   **Creating a Custom Collection:**
        ```powerfx
        ClearCollect( EmailList,
            ForAll( EmployeeGallery.AllItems,
                { To: ThisItem.Email, Subject: "Welcome!" }
            )
        )
        ```
    *   **Performing Calculations:** `Set( varTotal, Sum( ForAll( InvoiceLines, ThisItem.Quantity * ThisItem.Price ), Value ) )`

### **27. What is the difference between Navigate() and Back()?**

*   **`Navigate()` Function:**
    *   **Purpose:** Used to move **forward** to a specified screen. It gives you full control over the destination.
    *   **Syntax:** `Navigate( ScreenName, [ScreenTransition], [UpdateContextRecord] )`
    *   **You specify:**
        *   **Which screen** to go to.
        *   The **transition animation** (e.g., `ScreenTransition.Cover`, `ScreenTransition.Fade`).
        *   An optional **context variable** to pass to the new screen (using the third argument).

*   **`Back()` Function:**
    *   **Purpose:** Used to move **backward** to the **previous screen** in the navigation history. It's like a browser's "Back" button.
    *   **Syntax:** `Back()`
    *   **It is automatic:** You do not specify a screen. It simply reverses the last `Navigate()` action.
    *   **Use Case:** Perfect for "Cancel" buttons or a universal "Back" button in the header of a detail screen.

### **28. How do you validate user input in a form?**

*   **1. Using Data Source Validation:** The simplest method. Define required fields and data types (e.g., email, number) directly in your data source (like SharePoint or Dataverse). The form will automatically show an error upon submission if these rules are violated.
*   **2. Using Form Control Properties:**
    *   **`Required` Property:** Set a form control's `Required` property to `true` to force the user to enter a value.
    *   **`Valid` Property:** Use this property to run a custom validation rule.
        *   **Example (Email Format):** The `Valid` property of an email input could be: `IsMatch( EmailInput.Text, Match.Email )`
*   **3. Using the `If()` and `IsBlank()` Functions:**
    *   **On a Button's `OnSelect`:**
        ```powerfx
        If( IsBlank( TextInput_Name.Text ),
            Notify( "Name is required", Error ),
            IsBlank( TextInput_Email.Text ),
            Notify( "Email is required", Error ),
            // If all validations pass, submit the form
            SubmitForm( Form1 )
        )
        ```
*   **4. Using the `Error` and `ErrorKind` Properties:** After a form submission fails, you can check `Form1.Error` and `Form1.ErrorKind` to display a custom error message to the user.

### **29. What is the DateDiff() function, and how do you use it?**

*   **Purpose:** The **`DateDiff()`** function calculates the **difference between two dates** in a specified unit (e.g., days, months, years).
*   **Syntax:** `DateDiff( StartDateTime, EndDateTime, Unit )`
*   **Units:** `Years`, `Months`, `Days`, `Hours`, `Minutes`, `Seconds`.
*   **Common Use Cases:**
    *   **Calculating Age:** `DateDiff( ThisItem.BirthDate, Today(), Days ) / 365` (approximate age in years).
    *   **Calculating Days Until Due:** `DateDiff( Today(), ThisItem.DueDate, Days )` - A positive number means it's in the future, a negative number means it's past due.
    *   **Tracking Response Time:** `DateDiff( ThisItem.CreatedDate, ThisItem.ClosedDate, Hours )`
    *   **Conditional Formatting:** Set a label's `Color` property to `If( DateDiff(Today(), ThisItem.DueDate, Days) < 0, Red, Black )` to turn overdue items red.

### **30. How do you use IsEmpty() to check if a collection is empty?**

*   **Purpose:** The **`IsEmpty()`** function checks if a **table (data source, collection, or filtered set) contains zero records**.
*   **Syntax:** `IsEmpty( Table )`
*   **Returns:** **`true`** if the table is empty, **`false`** if it contains one or more records.
*   **Common Use Cases:**
    *   **Showing a "No Data" Message:** Add a **Label** control to your screen with the text "No records found". Set its `Visible` property to `IsEmpty( MyGallery.Items )`. This label will only appear when the gallery is empty.
    *   **Conditionally Hiding/Showing Controls:**
        ```powerfx
        // Show a "Refresh" button only if the collection is empty
        If( IsEmpty( myCollection ),
            true, // Show the button
            false // Hide the button
        )
        ```
    *   **Preventing Actions:** `If( IsEmpty( colSelectedItems ), Notify("Please select an item first"), Navigate(NextScreen) )`
