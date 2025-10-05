### **11. How do you connect a canvas app to SharePoint?**

*   **Step 1: Add a Data Source:**
    *   In Power Apps Studio, click on **"View"** in the top menu and then select **"Data sources"**. This opens the Data panel.
*   **Step 2: Choose SharePoint:**
    *   In the Data panel, click **"Add data source"**.
    *   From the list of connectors, select **"SharePoint"**.
    *   If prompted, sign in with your Microsoft 365 credentials.
*   **Step 3: Select Site and List:**
    *   Choose the **SharePoint site** that contains your list or library. You can type the site address or pick from "Recent sites" or "Favorites".
    *   After selecting the site, you will see all the lists and libraries on that site. **Select the specific list or library** you want to connect to.
    *   Click **"Connect"**.
*   **Result:** The SharePoint list now appears in your Data sources panel. You can now use its fields in galleries, forms, and other controls throughout your app.

### **12. What is a collection, and how do you create one using ClearCollect()?**

*   **What it is:** A **collection** is a temporary, in-memory table of data that is stored locally on the user's device. It is extremely versatile and used for storing data you don't want to query repeatedly, manipulating data, holding user selections, or working offline.
*   **How to Create with `ClearCollect()`:**
    *   The **`ClearCollect`** function is the most common way to create and populate a collection. It performs two actions:
        1.  **`Clear`**: Deletes all the records from a collection if it already exists.
        2.  **`Collect`**: Creates the collection (if it doesn't exist) and adds the specified records to it.
    *   **Syntax:** `ClearCollect( CollectionName, DataSource )`
    *   **Example:** `ClearCollect( colEmployees, SharePointEmployeesList )`
        *   This clears `colEmployees` (or creates it) and then populates it with all the data from the `SharePointEmployeesList`.
    *   You can use this function in the **`OnStart`** property of the app or the **`OnSelect`** property of a button.

### **13. How do you filter data in a gallery using the Filter() function?**

*   **The Principle:** You don't filter the gallery itself. Instead, you set the gallery's **`Items`** property to a filtered version of your data source.
*   **Using the `Filter()` Function:**
    *   Select your **Gallery** control.
    *   In the property dropdown (just below the ribbon), select **`Items`**.
    *   In the formula bar, you start with a `Filter()` formula that references your data source.
    *   **Syntax:** `Filter( DataSource, LogicalTest )`
    *   **Examples:**
        *   **By a Text Field:** `Filter( SharePointEmployeesList, Department = "Marketing" )`
        *   **By a Choice Field:** `Filter( SharePointEmployeesList, Status.Value = "Active" )`
        *   **By a Number Field:** `Filter( SharePointEmployeesList, 'Vacation Days' > 5 )`
        *   **Using Search:** `Filter( SharePointEmployeesList, StartsWith( Title, TextSearchBox.Text ) )`
*   The gallery will now only display records that meet the filter criteria.

### **14. What is delegation, and why is it important in Power Apps?**

*   **What it is:** **Delegation** is the process where the data processing workload is **"delegated" to the data source** (e.g., SharePoint, SQL Server) instead of being pulled into the app and processed locally on your device.
*   **Why it is Critically Important:**
    *   **Performance:** Data sources are powerful servers designed to handle large datasets. Sending a filtered query to the server is vastly faster than downloading 100,000 records to your phone and then filtering them locally.
    *   **Data Limits:** Many data sources have a **delegation limit** (often 500, 2000, or 2000 records). If a function cannot be delegated, Power Apps will only retrieve the *first* set of records up to that limit, and then filter them locally. **This means you could miss data.**
*   **Key Takeaway:** You must use **delegable functions** (like `=`, `<`, `>`, `StartsWith`, `Filter`) with data source columns that support delegation to ensure you work with your complete dataset. Non-delegable functions (like `Search`, `in`, `Contains`) can cause performance issues and data loss with large datasets.

### **15. How do you sort data in a gallery using Sort() or SortByColumns()?**

*   **Using `Sort()`:**
    *   This function sorts a table based on a single column in **ascending** order.
    *   **Syntax:** `Sort( DataSource, ColumnName )`
    *   **Example:** `Sort( SharePointEmployeesList, 'Last Name' )`
    *   To sort in **descending** order, wrap it in `Sort( ..., ColumnName, Descending )`.
*   **Using `SortByColumns()`:**
    *   This function is more flexible. It can sort by multiple columns and explicitly defines the sort order for each.
    *   **Syntax:** `SortByColumns( DataSource, ColumnName, SortOrder [, ColumnName2, SortOrder2, ... ] )`
    *   **Examples:**
        *   Single Column: `SortByColumns( SharePointEmployeesList, "Department", Ascending )`
        *   Multiple Columns: `SortByColumns( SharePointEmployeesList, "Department", Ascending, "Last Name", Ascending )`
*   **Application:** You set the gallery's **`Items`** property to the sorted data source: `SortByColumns( SharePointEmployeesList, "Department", Ascending )`.

### **16. What is the difference between Patch() and Update()?**

*   **`Patch()` Function:**
    *   **Purpose:** The primary function for **creating, updating, or modifying records** in a data source. It is highly versatile.
    *   **Syntax:** `Patch( DataSource, BaseRecord, Changes )`
        *   **To Update:** `Patch( Employees, LookUp(Employees, ID=1), {Status: "Inactive"} )`
        *   **To Create:** `Patch( Employees, Defaults(Employees), {Title: "Jane Doe", Department: "Sales"} )`
    *   **Use Case:** Use `Patch()` for most data write operations, especially when you need to update specific fields without affecting others.

*   **`Update()` Function:**
    *   **Purpose:** **Only updates an existing record** if it can be found based on all **primary key** columns. It cannot create new records.
    *   **Syntax:** `Update( DataSource, Record, ChangeRecord )` - The `Record` must match the primary key.
    *   **Use Case:** Less common than `Patch()`. It's useful when you have a complete record and want to replace it, but `Patch()` is generally preferred for its flexibility and ability to create records.

### **17. How do you use LookUp() to retrieve a single record?**

*   **Purpose:** The **`LookUp()`** function searches a data source and returns the **first record** that matches a specified condition.
*   **Syntax:** `LookUp( DataSource, LogicalTest, [ResultIfFound] )`
*   **Common Use Cases:**
    *   **Populating a Detail Screen:** When a user clicks an item in a gallery, `LookUp` fetches the full record to display on a new screen.
        *   **Example:** `LookUp( SharePointEmployeesList, ID = Gallery1.Selected.ID )`
    *   **Getting a Specific Field:** You can also return just a single column from the found record.
        *   **Example:** `LookUp( SharePointEmployeesList, ID = 5, "Email Address" )` would return just the email string for the employee with ID 5.
    *   **Use with Variables:** `Set( varCurrentEmployee, LookUp( SharePointEmployeesList, ID = Gallery1.Selected.ID ) )`

### **18. What is a data table, and how do you display one in Power Apps?**

*   **What it is:** The **Data table** control is a structured, tabular control designed specifically for displaying a table of data. Unlike a Gallery, it presents data in a traditional row-and-column format, similar to an Excel table.
*   **How to Display One:**
    *   **Step 1:** Insert a Data table control from the **"Insert"** menu under **"Display"**.
    *   **Step 2:** Set its **`Data source`** property to your data source (e.g., `SharePointEmployeesList`).
    *   **Step 3 (Configure Fields):**
        *   Select the data table.
        *   In the right-hand properties pane, click **"Edit fields"**.
        *   Click **"Add field"** and select the columns from your data source you want to display.
    *   You can reorder columns by dragging the field headers and change their display name by double-clicking the header in the data table.

### **19. How do you connect to Excel Online as a data source?**

*   **Prerequisite:** The Excel file must be stored in **OneDrive for Business** or a **SharePoint Document Library**, and the data must be in a ** formatted Excel Table** (not just a range of cells).
*   **Steps:**
    *   **Step 1:** In the **Data sources** panel, click **"Add data source"**.
    *   **Step 2:** Select the **"OneDrive for Business"** or **"SharePoint"** connector (depending on where your file is stored).
    *   **Step 3:** Navigate to and select your **Excel file**.
    *   **Step 4:** After selecting the file, Power Apps will show a list of the **Tables** within that workbook. Select the table you want to use.
    *   **Step 5:** Click **"Connect"**. The Excel table now appears as a data source, and you can use it just like a SharePoint list.

### **20. What is the Search() function, and how do you implement search in a gallery?**

*   **What it is:** The **`Search()`** function is a simple way to search for text across multiple columns in a data source. **Important Note:** It has significant delegation limitations and should only be used on small datasets (< 500 records).
*   **How to Implement a Search Box:**
    *   **Step 1:** Add a **Text input** control to your screen. This will be your search box. Rename it to `txtSearch`.
    *   **Step 2:** Select your **Gallery** control.
    *   **Step 3:** Set the gallery's **`Items`** property to the `Search()` function.
    *   **Syntax:** `Search( DataSource, SearchString, Column1, Column2, ... )`
    *   **Example:** `Search( SharePointEmployeesList, txtSearch.Text, "Title", "Department", "Email" )`
    *   This will filter the gallery to show only records where the text in `txtSearch.Text` appears in the "Title", "Department", or "Email" columns.
*   **Delegation Warning:** For large datasets, it's better to use the delegable **`Filter()`** function with **`StartsWith()`** on a single column, e.g., `Filter( DataSource, StartsWith( Title, txtSearch.Text ) )`.
