Of course. Here are the comprehensive, detailed study notes on the Power Fx collection functions, following all specified formatting, content guidelines, and the extensive word count requirement.

# Ultimate Guide to Power Fx Collections: `ClearCollect`, `Collect`, and `Clear`

## Part 1: The Foundational Concepts of Collections

### Introduction: What is a Collection and Why is it Essential?

*   In Power Fx, a **collection** is an in-memory, temporary table of data that exists only while the app is running. It is one of the most powerful and versatile tools available to a Power Apps developer.
*   Think of a collection as a private, temporary spreadsheet or a scratchpad for your application. You can create it on the fly, add rows, modify data, remove rows, and use it as a data source for galleries, forms, and charts, all without permanently affecting any external database like SharePoint or Dataverse.
*   Mastering collections is the line between building simple, static apps and creating dynamic, high-performance, and user-friendly applications that can handle complex data manipulations, offline scenarios, and sophisticated user interfaces.
*   These functions are **client-side**, meaning all operations happen on the user's device (phone, tablet, browser). This makes them incredibly fast for UI interactions but also means they are subject to device memory limits and are not shared between users.

### The Anatomy of a Collection

*   A collection is fundamentally a **table**. Like any table, it has two key components:
    *   **Records:** These are the rows of the table. Each record represents a single item or entity (e.g., a customer, a product, a shopping cart item).
    *   **Columns:** These are the columns of the table, also known as fields. Each column defines a specific piece of information about each record (e.g., `FirstName`, `ProductID`, `Quantity`, `Price`).
*   **Schema:** The structure of a collection—its set of columns and their corresponding data types (Text, Number, Date, etc.)—is called its schema. A collection's schema is typically defined the first time data is placed into it using `Collect()` or `ClearCollect()`.
    *   Example: The collection `colShoppingCart` might have the schema:
        *   `ItemID` (Number)
        *   `ItemName` (Text)
        *   `UnitPrice` (Number)
        *   `Quantity` (Number)

> [!IMPORTANT]
> The data in a collection is temporary and **is not saved automatically**. When the user closes the app, all collections are cleared from memory and their data is lost. To persist collection data between sessions, you must explicitly use the `SaveData()` and `LoadData()` functions.

### Core Use Cases: Why Collections are Indispensable

*   Collections are not just a feature; they are a solution to numerous common app development challenges. Understanding these use cases is key to knowing *when* and *why* to use them.

*   **1. Caching Data for Performance:**
    *   **Problem:** Continuously querying a large external data source (like a SQL server or a large SharePoint list) for data that doesn't change often is slow and inefficient. Every filter or sort operation might require a new server round-trip, leading to a laggy user experience.
    *   **Solution:** On app or screen load, use `ClearCollect` to pull a relevant subset of data from the external source into a local collection. Then, point your galleries and controls to this local collection. All subsequent filtering, sorting, and searching operations are performed on the in-memory collection, which is lightning-fast because it requires no network communication.

*   **2. Staging and Shaping Data:**
    *   **Problem:** Data from an external source is often not in the perfect shape for your app's UI. You might need to combine columns, perform calculations, rename fields for clarity, or join information from multiple sources.
    *   **Solution:** Use `ClearCollect` combined with functions like `AddColumns`, `RenameColumns`, `ShowColumns`, and `LookUp` to pull data and transform it into a perfectly shaped collection that is optimized for your app's display and logic.

*   **3. Gathering Data from the User (Temporary "Shopping Cart"):**
    *   **Problem:** An app needs to allow a user to select multiple items from a list, specify quantities, and then submit them all in a single transaction (e.g., a shopping cart, an expense report, a timesheet).
    *   **Solution:** Create an empty collection, `colCart`. As the user clicks an "Add" button next to items, use `Collect` to add each selected item as a record to `colCart`. The user can view, edit (`Patch`), and remove (`Remove`) items from this collection. When they are ready, a "Submit" button can then loop through `colCart` and save all the items to the permanent data source in one go using `ForAll` and `Patch`.

*   **4. Enabling Offline Capabilities:**
    *   **Problem:** Users need to be able to use the app in areas with poor or no internet connectivity.
    *   **Solution:** Collections are the core of offline functionality. The pattern is:
        1.  While online, `ClearCollect` data into a collection.
        2.  Use `SaveData()` to save the collection to a local file on the device.
        3.  On app start, use `LoadData()` to load this saved data back into the collection if the device is offline.
        4.  The user interacts entirely with the local collection.
        5.  Changes made offline are stored in another "sync" collection.
        6.  When connectivity is restored, the app can then patch the changes from the sync collection back to the master data source.

*   **5. Simplifying Complex Forms and Logic:**
    *   **Problem:** Building forms with repeating sections (e.g., adding multiple contacts or line items) is difficult with standard form controls.
    *   **Solution:** Use a collection as the data source for a gallery. Each item in the gallery can contain input controls (text inputs, dropdowns). As the user fills out these controls, their data is patched directly into the collection. Adding a "New Item" button simply uses `Collect` to add a new blank record to the collection, which dynamically adds a new row to the gallery form.

### Collections vs. Global/Context Variables

*   It's important to understand the difference between collections and variables, as they both store data in memory.

| Feature | **Collection** | **Variable (Global or Context)** |
| :--- | :--- | :--- |
| **Structure** | Tabular (Rows and Columns) | Scalar (single value like Text, Number, Boolean) or a single Record/Table |
| **Data Type** | Always a `Table` | Can be any data type: `Text`, `Number`, `Boolean`, `Record`, `Table` |
| **Primary Use** | Storing lists of similar items, tabular data, temporary databases. | Storing state information, single values, user preferences, flags. |
| **Creation** | `Collect()`, `ClearCollect()` | `Set()` (for global), `UpdateContext()` (for screen-context) |
| **Analogy** | A temporary spreadsheet with multiple rows. | A single sticky note with a piece of information on it. |
| **Example** | `colUsers` storing 100 user records. | `varCurrentUser` storing the record for the logged-in user. `varIsAdmin` flag. |

> [!TIP]
> A common "level-up" moment for Power Apps developers is realizing when they are using multiple, related variables (e.g., `varItemName1`, `varItemPrice1`, `varItemName2`, `varItemPrice2`) and should instead be using a single collection with columns for `ItemName` and `ItemPrice`. This makes code cleaner, more scalable, and easier to manage.

---

## Part 2: Deep Dive into `ClearCollect()`

### Core Concept: The "Reset and Load" Powerhouse

*   `ClearCollect()` is arguably the most frequently used collection function. Its name perfectly describes its two-step action:
    1.  **Clear:** It first finds the specified collection and completely **deletes all existing records** from it. If the collection does not exist yet, this step does nothing.
    2.  **Collect:** It then **adds a new set of records** to that now-empty collection.
*   This makes it the ideal function for initializing or resetting a collection with a fresh set of data from a source.

### Syntax Variations

*   There are two primary ways to use `ClearCollect`:

1.  **Creating from a Data Source (Most Common):**
    *   `ClearCollect(CollectionName, DataSource)`
    *   This syntax takes an existing table (like a SharePoint List, a Dataverse Table, or even another collection) and creates a complete copy of it in `CollectionName`.

2.  **Creating from Manually Defined Records:**
    *   `ClearCollect(CollectionName, Record1, [Record2, ...])`
    *   This syntax allows you to create a collection from scratch by explicitly defining each record.

### Parameters Explained in Detail

*   **`CollectionName` (Required)**
    *   The name you want to give your collection.
    *   Collection names should follow good naming conventions, typically starting with a prefix like `col` or `local` for clarity (e.g., `colUsers`, `colProducts`).
    *   If a collection with this name already exists, it will be cleared. If it doesn't exist, it will be created.
*   **`DataSource` or `Items` (Required)**
    *   This is the source of the data for the new collection.
    *   It can be a direct reference to a data source (`SharePointList`), another collection (`colAllUsers`), or the result of a function that returns a table (e.g., `Filter(...)`, `AddColumns(...)`, `ShowColumns(...)`).
    *   When using manually defined records, each record must be enclosed in curly braces `{}` with the column name and value separated by a colon (e.g., `{Name: "Alice", Age: 30}`).

### Schema Definition with `ClearCollect`

*   The `ClearCollect` function plays a critical role in defining the **schema** (the columns and their data types) of your collection.
*   The schema is inferred from the `DataSource` or the first record provided.
*   **Example 1: Schema from a Data Source**
    *   If you run `ClearCollect(colUsers, 'Your User List')`, the `colUsers` collection will automatically have the exact same columns and data types as your `'Your User List'` SharePoint list.
*   **Example 2: Schema from a Manual Record**
    *   Formula: `ClearCollect(colEmployees, {FullName: "Bob Smith", EmployeeID: 101, IsActive: true})`
    *   Resulting `colEmployees` Schema:
        *   `FullName` (Text)
        *   `EmployeeID` (Number)
        *   `IsActive` (Boolean)

> [!WARNING]
> A collection's schema is rigid once defined. If you later try to use `Collect()` to add a record with a different data type (e.g., adding text to the `EmployeeID` column), your app will show a formula error. `ClearCollect` is powerful because it can *redefine* the schema entirely with each call if the source schema is different from the collection's previous one.

### Realistic and Practical `ClearCollect` Examples

*   **Use Case 1: Caching a SharePoint List on App Start**
    *   **Context:** Improve app performance by loading a "Products" list locally.
    *   **Location:** `App.OnStart` property.
    *   **Formula:** `ClearCollect(colProducts, 'Products List')`
    *   **Result:** A local, in-memory copy of the entire 'Products List' is created. Galleries throughout the app can now use `colProducts` as their `Items` source, making sorting and filtering instantaneous.

*   **Use Case 2: Populating a Dropdown with Static Choices**
    *   **Context:** You need a dropdown for task priorities that are not stored in a database.
    *   **Location:** `Screen.OnVisible` property.
    *   **Formula:**
        ```powerapps
        ClearCollect(
            colPriorities,
            {ID: 1, Value: "High"},
            {ID: 2, Value: "Medium"},
            {ID: 3, Value: "Low"}
        )
        ```
    *   **Dropdown Control Setup:**
        *   `Items`: `colPriorities`
        *   `Value`: `Value` (Telling the dropdown which column to display)

*   **Use Case 3: Shaping and Transforming Data**
    *   **Context:** You have a `Sales Data` list but only need three columns for a chart, and you need to calculate a `Total` for each record.
    *   **Formula:**
        ```powerapps
        ClearCollect(
            colChartData,
            AddColumns(
                ShowColumns(
                    'Sales Data',
                    "Product",
                    "UnitsSold",
                    "UnitPrice"
                ),
                "TotalSale", UnitsSold * UnitPrice
            )
        )
        ```
    *   **Explanation:** This is a powerful, nested pattern.
        1.  `ShowColumns` grabs only the three columns we need from the data source.
        2.  `AddColumns` takes that smaller table and adds a new calculated column called `TotalSale`.
        3.  `ClearCollect` takes the final, perfectly shaped result and stores it in `colChartData`.

*   **Use Case 4: Creating a Searchable Cache (Delegation Workaround)**
    *   **Context:** You need to search on a column in SharePoint that is not delegable (e.g., a `Choice` or `Person` column's display name). Direct filtering `Filter('My List', 'MyChoiceColumn'.Value = "Some Value")` will throw a delegation warning.
    *   **Formula (in `Screen.OnVisible`):** `ClearCollect(colLocalCache, 'My List')`
    *   **Gallery `Items` Property:**
        ```powerapps
        Filter(
            colLocalCache,
            StartsWith(SearchInput.Text, Title) &&
            'MyChoiceColumn'.Value = StatusDropdown.Selected.Value
        )
        ```
    *   **Benefit:** Since `colLocalCache` is in memory, the delegation limits do not apply. You can perform complex filtering and searching across all its columns without any warnings, as long as the entire list fits within the non-delegable query limit (typically up to 2000 records).

### Performance Considerations and Delegation

*   When the source for `ClearCollect` is an external data source, **delegation** is the most critical concept to understand.
*   **Delegable Source:** If your `ClearCollect` formula is simple, Power Apps "delegates" the work of fetching the data to the server. For example, `ClearCollect(col, 'My List')` tells the server "give me everything". `ClearCollect(col, Filter('My List', Status = "Active"))` tells the server "just give me the active records". The server does the hard work, which is highly efficient.
*   **Non-delegable Source:** If the formula contains functions the server doesn't understand (like complex `Search` or operations on certain column types), Power Apps first pulls a limited number of records from the server to the app (the delegation limit, default 500, max 2000), and *then* performs the operation locally.
> [!CAUTION]
> If your `ClearCollect` contains a non-delegable query against a list of 10,000 items, it will only process the first 500-2000 records. Your collection will be incomplete, leading to missing data and incorrect calculations in your app. Always look for the blue underline and delegation warnings in the formula bar.

### Flashcard Q&A: `ClearCollect()`

*   **Q:** What are the two distinct actions performed by `ClearCollect` in order?
*   **A:** First, it `Clear`s (removes all records from) the target collection. Second, it `Collect`s (adds new records to) the collection.

*   **Q:** How is the schema (columns and data types) of a new collection determined when using `ClearCollect`?
*   **A:** The schema is inferred from the source data provided, whether it's another data source table or the first manually defined record in the arguments.

*   **Q:** You have an app with 10,000 products. Why might `ClearCollect(colProducts, Filter('Products List', "Blue" in Color.Value))` result in an incomplete collection?
*   **A:** The `in` operator on a choice field (`Color.Value`) is often non-delegable. The formula would only filter the first 500-2000 records brought back from the server, potentially missing "Blue" products that exist beyond that limit.

---

## Part 3: Deep Dive into `Collect()`

### Core Concept: The "Append" Function

*   The `Collect()` function is used to **add one or more records to an existing collection**.
*   Unlike `ClearCollect`, it is **additive**. It does not delete any existing data; it simply appends new records to the end of the collection.
*   This makes it the perfect function for incrementally building a collection based on user actions, such as adding items to a shopping cart, logging events, or capturing form inputs in a loop.

### Syntax

*   `Collect(CollectionName, ItemOrRecordToAdd)`
*   You can also add multiple items at once, although this is less common than with `ClearCollect`: `Collect(CollectionName, Record1, Record2, ...)`

### Parameters Explained in Detail

*   **`CollectionName` (Required)**
    *   The name of the collection to which you want to add records.
    *   **Crucial Point:** If the collection does not exist when `Collect` is first called, Power Fx will automatically create it and infer its schema from the first item being added.
*   **`ItemOrRecordToAdd` (Required)**
    *   This is the data to be added. It can be a single `Record`, a `LookUp` result that returns a record, or `ThisItem` from within a gallery.

### Schema Matching: The Critical Hurdle

*   The biggest challenge when using `Collect` is ensuring the schema of the record you are adding **matches the schema of the existing collection**.

*   **Perfect Match (The Happy Path):**
    *   The collection `colUsers` has columns `FirstName` (Text) and `UserID` (Number).
    *   `Collect(colUsers, {FirstName: "Jane", UserID: 102})` -> Works perfectly. The new record is appended.

*   **Mismatched Column Names:**
    *   `Collect(colUsers, {FullName: "John Doe", ID: 103})`
    *   **Behavior:** This will likely cause a schema mismatch. Power Fx may try to be "helpful" by creating a collection that can hold multiple record types, which can become very confusing. It is best practice to *always match the schema exactly*. Use the `Patch` function or reshape your data before collecting if schemas differ.

*   **Mismatched Data Types:**
    *   `Collect(colUsers, {FirstName: "Kevin", UserID: "104"})`
    *   **Behavior:** This will cause a formula error. The `UserID` column in `colUsers` expects a `Number`, but you are providing `Text` ("104"). You would need to convert it using `Value()`: `Collect(colUsers, {FirstName: "Kevin", UserID: Value("104")})`.

*   **Adding an Incomplete Record:**
    *   `Collect(colUsers, {FirstName: "Maria"})`
    *   **Behavior:** This works! The `UserID` column for Maria's record will be `Blank()`. This is valid and often useful for creating partially filled records that will be completed later.

> [!IMPORTANT]
> The very first call to `Collect` on a non-existent collection sets its schema. Every subsequent `Collect` call must provide records that are compatible with that initial schema. `ClearCollect` can reset this schema, but `Collect` cannot change it once established.

### Realistic and Practical `Collect` Examples

*   **Use Case 1: The quintessential "Shopping Cart"**
    *   **Context:** In a gallery displaying products (`GalleryProducts`), an "Add to Cart" button adds the selected item to a `colCart` collection.
    *   **Button `OnSelect` Property:**
        ```powerapps
        Collect(
            colCart,
            {
                CartItemID: Self.UniqueId, // A way to get a unique ID for the cart item
                ProductID: ThisItem.ID,
                ProductName: ThisItem.Title,
                UnitPrice: ThisItem.Price,
                Quantity: 1 // Default to adding one item
            }
        )
        ```
    *   **Result:** Each time a button is clicked, a new row representing that product is added to `colCart`. A separate gallery can then display the contents of `colCart`.

*   **Use Case 2: Capturing Multiple Selections using `ForAll`**
    *   **Context:** A gallery of tasks has checkboxes, and you want to collect all checked tasks into a separate collection for batch processing.
    *   **Gallery:** `GalleryTasks` with `Checkbox1` inside.
    *   **"Archive Selected" Button `OnSelect` Property:**
        ```powerapps
        // First, ensure the collection is empty before starting
        Clear(colSelectedTasks);
        
        ForAll(
            Filter(GalleryTasks.AllItems, Checkbox1.Value = true),
            Collect(colSelectedTasks, ThisRecord)
        )
        ```
    *   **Explanation:**
        1.  `Clear` empties the collection to avoid duplicates from previous clicks.
        2.  `Filter(...)` creates a temporary table of only the gallery items where the checkbox is ticked.
        3.  `ForAll` iterates through each record in that filtered table.
        4.  `Collect(colSelectedTasks, ThisRecord)` adds the entire record (`ThisRecord`) for each selected task into the target collection.

*   **Use Case 3: Creating a Log of User Actions**
    *   **Context:** For auditing or debugging, you want to keep a running log of important actions the user takes during their session.
    *   **Location (on various button `OnSelect` properties):**
        *   On "Save" button: `Collect(colAuditLog, {Timestamp: Now(), Action: "Record Saved", RecordID: MyForm.LastSubmit.ID})`
        *   On "Delete" button: `Collect(colAuditLog, {Timestamp: Now(), Action: "Record Deleted", RecordID: Gallery.Selected.ID})`
    *   **Result:** The `colAuditLog` collection builds up over time, providing a history of the user's session that can be inspected or even saved upon exit.

### Flashcard Q&A: `Collect()`

*   **Q:** Does `Collect()` ever remove data from a collection?
*   **A:** No. `Collect()` is purely additive; it only appends new records.

*   **Q:** What happens if you try to `Collect` a record into a collection, but the data type for one of the columns does not match the collection's schema?
*   **A:** Power Fx will generate a formula error, preventing the app from running, because the data types are incompatible.

*   **Q:** If the `colOrders` collection does not exist, what does the formula `Collect(colOrders, {OrderID: 1})` do?
*   **A:** It creates the `colOrders` collection, defines its schema to have a single `Number` column named `OrderID`, and adds the first record `{OrderID: 1}` to it.

---

## Part 4: Deep Dive into `Clear()`

### Core Concept: The "Empty and Preserve" Function

*   The `Clear()` function is the simplest of the three. Its sole purpose is to **remove all records** from a specified collection.
*   The most important nuance of `Clear()` is that it **does not delete the collection itself or its schema**. After calling `Clear()`, the collection still exists—it is simply empty. Its columns and their data types remain defined, ready to receive new data.

### Syntax

*   `Clear(CollectionName)`

### Parameters Explained

*   **`CollectionName` (Required)**
    *   The name of the collection you want to empty.

### Detailed Behavior and Nuances

*   **Empties Records, Preserves Schema:** This is the key takeaway.
    *   `ClearCollect(colTest, {A: 1, B: "Hello"})` -> Creates `colTest` with columns A (Number) and B (Text) and one record.
    *   `Clear(colTest)` -> Removes the record `{A: 1, B: "Hello"}`. The collection `colTest` still exists and still knows it should have a Number column 'A' and a Text column 'B'.
    *   `Collect(colTest, {A: 2, B: "World"})` -> This will work perfectly, because the schema was preserved.

*   **Difference from `ClearCollect(Collection, {})`:**
    *   At first glance, `Clear(MyCollection)` and `ClearCollect(MyCollection, {})` might seem similar (the latter provides an empty record, effectively adding nothing).
    *   **`Clear(MyCollection)`:** Empties the collection, leaving its existing schema intact.
    *   **`ClearCollect(MyCollection, {})` or `ClearCollect(MyCollection, Blank())`:** This is more drastic. It clears the collection and then re-defines its schema to be empty or based on the empty record. This can effectively erase the schema, which is rarely what you want. `Clear()` is the safe and correct way to just empty the records.

> [!NOTE]
> `Clear()` is a highly efficient operation. It's much faster than, for example, using `RemoveIf(MyCollection, true)` to remove all records. When you want to empty a collection, `Clear()` is always the right tool.

### Realistic and Practical `Clear` Examples

*   **Use Case 1: Resetting a Form or Shopping Cart**
    *   **Context:** After a user submits an order from their shopping cart, the cart needs to be emptied for their next purchase.
    *   **"Submit Order" Button `OnSuccess` or `OnSelect` Property:**
        ```powerapps
        ForAll(colCart, Patch(...)); // Save the data
        Clear(colCart); // Clear the local cart
        Navigate(ConfirmationScreen); // Move to the next screen
        ```
    *   **Result:** The user's cart is empty and ready for a new session.

*   **Use Case 2: Preparing for a Fresh `ForAll` Loop**
    *   **Context:** You are collecting items in a loop, as seen in the `ForAll` example for `Collect`. You must `Clear` the target collection *before* the loop starts to ensure you're not just adding to items from a previous operation.
    *   **"Process" Button `OnSelect`:**
        ```powerapps
        Clear(colItemsToProcess); // CRITICAL reset step
        ForAll(
            Filter(GallerySource.AllItems, IsSelected),
            Collect(colItemsToProcess, ThisRecord)
        )
        // ... now do something with the freshly collected items
        ```

*   **Use Case 3: Freeing Up Device Memory**
    *   **Context:** Your app has a screen with a large collection that is only used on that screen. To be a good citizen of the device's memory, you can clear the collection when the user leaves the screen.
    *   **Screen `OnHidden` Property:**
        ```powerapps
        Clear(colLargeLocalCache)
        ```
    *   **Result:** This helps ensure your app remains performant, especially on lower-end devices or in long-running sessions, by releasing memory that is no longer needed.

### Flashcard Q&A: `Clear()`

*   **Q:** What is the primary difference in outcome between `Clear(col)` and a user closing and reopening the app?
*   **A:** `Clear(col)` removes all records but preserves the collection's existence and schema. Closing the app destroys the collection entirely (both records and schema) from memory.

*   **Q:** You have a collection named `colA`. What is the difference between `Clear(colA)` and `Remove(colA, First(colA))`?
*   **A:** `Clear(colA)` removes *all* records. `Remove(colA, First(colA))` removes only the very *first* record in the collection.

*   **Q:** Is `Clear()` a delegable function?
*   **A:** The concept of delegation does not apply to `Clear()` because it operates exclusively on an in-memory collection and does not interact with an external data source. All its operations are local.

---

## Part 5: Advanced Collection Management and Architectural Patterns

### Manipulating Records Within Collections: The Missing Pieces

*   `ClearCollect`, `Collect`, and `Clear` are for adding and removing *all* records. But you also need to manage individual records.

*   **Updating a Record: The `Patch()` Function**
    *   `Patch` is your tool for modifying an existing record *in place*.
    *   **Syntax:** `Patch(CollectionName, RecordToUpdate, {ChangesToMake})`
    *   **Example: Updating Quantity in a Shopping Cart**
        *   Inside the cart gallery, an "Increase Qty" button `OnSelect` could have:
        ```powerapps
        Patch(
            colCart,
            ThisItem,
            { Quantity: ThisItem.Quantity + 1 }
        )
        ```
        *   **Explanation:**
            1.  `colCart` is the collection to modify.
            2.  `ThisItem` identifies the specific record (the row in the gallery) to update.
            3.  `{Quantity: ...}` provides the new value for the `Quantity` column.

*   **Removing Individual Records: `Remove()` and `RemoveIf()`**
    *   **`Remove(Collection, RecordToRemove)`:** Removes a specific record that you have a reference to.
        *   **Example: A "Delete" icon in a gallery:**
        *   `OnSelect`: `Remove(colCart, ThisItem)`
    *   **`RemoveIf(Collection, Condition)`:** Removes all records that match a logical condition.
        *   **Example: Removing all out-of-stock items:**
        *   `OnSelect`: `RemoveIf(colCart, Quantity = 0)`

### Comprehensive Workflow: A "Multi-Step Form" Pattern

*   This pattern demonstrates how all the functions work together to create a sophisticated user experience.
*   **Goal:** Create a wizard-style form where the user enters data on multiple screens, with the ability to go back and edit, before a final submission.

1.  **Screen 1: `OnVisible`**
    *   `// If this is the start of a new entry, create an empty collection`
    *   `// The ID of -1 is a temporary marker for a new record`
    *   `ClearCollect(colWizardData, {ID: -1, Step1Info: "", Step2Info: "", Step3Status: ""})`

2.  **Screen 1: "Next" Button `OnSelect`**
    *   `// Patch the data from the first screen into our collection`
    *   `Patch(colWizardData, First(colWizardData), {Step1Info: txtStep1Input.Text});`
    *   `Navigate(Screen2)`

3.  **Screen 2: "Next" Button `OnSelect`**
    *   `// Patch the data from the second screen`
    *   `Patch(colWizardData, First(colWizardData), {Step2Info: txtStep2Input.Text});`
    *   `Navigate(Screen3)`

4.  **Screen 3: "Submit" Button `OnSelect`**
    *   `// Patch the final data`
    *   `Patch(colWizardData, First(colWizardData), {Step3Status: ddStatus.Selected.Value});`
    *   `// Now, patch the complete record from the collection to the main data source`
    *   `Patch('Main DataSource', Defaults('Main DataSource'), First(colWizardData));`
    *   `// Clean up the temporary collection`
    *   `Clear(colWizardData);`
    *   `Navigate(SuccessScreen)`

> [!TIP]
> This "staging collection" pattern is incredibly powerful. It allows for complex, multi-part data entry without saving incomplete data to your master data source. It also provides a perfect structure for "Save as Draft" functionality. All you need to do is `Collect` the staging record into a `colDrafts` collection.
