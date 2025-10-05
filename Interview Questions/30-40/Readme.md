### **31. How do you create a custom component in Power Apps?**

*   **What it is:** A **custom component** is a reusable set of controls with their own properties and logic, similar to a function in programming. Create once, use many times.
*   **Creation Steps:**
    *   **Step 1: Insert Component:** Go to the **"Insert"** tab, click the **"Component"** dropdown, and select **"New component"**.
    *   **Step 2: Design the UI:** A new component canvas opens. **Add controls** (text inputs, labels, icons, etc.) just like you would on a screen.
    *   **Step 3: Define Custom Properties:** This is the key to making components dynamic.
        *   In the **"Properties"** pane on the right, click **"New custom property"**.
        *   Give it a name (e.g., `DisplayText`), choose a type (**Input**, **Output**, or **Data**), and a data type (Text, Number, etc.).
        *   **Input Properties** pass data *into* the component.
        *   **Output Properties** pass data *out of* the component (e.g., when it's clicked).
    *   **Step 4: Bind Properties:** Connect your component's internal controls to the custom properties.
        *   Example: Set a **Label's** `Text` property to `Component.DisplayText` to show whatever text is passed into the component.
    *   **Step 5: Save and Use:** Click the checkmark to save. The component now appears in the "Custom components" library. Drag and drop it onto any screen.

### **32. What is a form control, and how do you use EditForm() and DisplayForm()?**

*   **What it is:** The **Form** control is a container specifically designed for creating, viewing, and editing records from a data source. It automatically generates fields based on your data source's schema.
*   **EditForm():**
    *   **Purpose:** Used for **creating new records** or **editing existing ones**.
    *   **Setup:**
        1.  Insert an **Edit Form** control.
        2.  Set its **`DataSource`** property (e.g., `SharePointList`).
        3.  Use the **"Edit fields"** pane to add and arrange the data cards (fields) you want users to edit.
        4.  Set the form's **`Mode`** property:
            *   **`FormMode.New`**: For creating a record.
            *   **`FormMode.Edit`**: For editing an existing record. You must set the **`Item`** property to the record to edit (e.g., `Gallery1.Selected`).
        5.  Use **`SubmitForm(FormName)`** in a button's `OnSelect` to save changes.
*   **DisplayForm():**
    *   **Purpose:** Used for **read-only viewing** of a single record. Users cannot edit the data.
    *   **Setup:**
        1.  Insert a **Display Form** control.
        2.  Set its **`DataSource`** and **`Item`** properties (e.g., `Gallery1.Selected`).
        3.  Use the **"Edit fields"** pane to choose which fields to display.
    *   **Key Difference:** There is no `SubmitForm()` for a Display Form, as no data is saved.

### **33. How do you implement a dropdown menu with dynamic values?**

*   **Goal:** Create a dropdown where the choices come from a data source (like a SharePoint list) rather than being hard-coded.
*   **Steps:**
    *   **Step 1:** Add a **Dropdown** control from the **"Input"** menu.
    *   **Step 2:** Set the **`Items`** property to your data source.
        *   Example: `DepartmentsList` (a SharePoint list with a "Title" column).
    *   **Step 3: Configure Value and Display Fields:**
        *   **`Value`:** This is the unique identifier that gets saved when a user makes a selection (e.g., `ID`).
        *   **`DisplayFields`:** This is a table of the friendly text the user sees (e.g., `["Title"]`).
*   **Advanced Dynamic Filtering:** You can make one dropdown filter another.
    *   **First Dropdown (`ddCategory`):** `Items: Categories`
    *   **Second Dropdown (`ddSubCategory`):** `Items: Filter( SubCategories, CategoryID = ddCategory.Selected.ID )`

### **34. What is the Pen Input control, and how do you save signatures?**

*   **What it is:** The **Pen Input** control allows users to draw or write directly on the screen, making it perfect for capturing signatures, sketches, or annotations.
*   **Implementation:**
    *   **Step 1:** Insert a **Pen Input** control from the **"Input"** menu.
    *   **Step 2: Configure Properties:**
        *   **`InkColor`**: Set the drawing color.
        *   **`Thickness`**: Set the line thickness.
    *   **Step 3: Saving the Signature:**
        *   The drawn image is stored in the control's **`Image`** property.
        *   To save it, you must **`Patch`** this image data to a data source column of type **"Image"** or **"Multiple lines of text"**.
        *   **Example Patch Formula:**
            ```powerfx
            Patch( SignaturesList,
                Defaults(SignaturesList),
                {
                    Customer: varCurrentCustomer,
                    Signature: PenInput1.Image,
                    SignDate: Now()
                }
            )
            ```
    *   **Step 4: Displaying a Saved Signature:** Use an **Image** control and set its **`Image`** property to the URL or data from your data source.

### **35. How do you create a responsive layout for mobile and tablet?**

*   **Core Principle:** Use containers and relative positioning instead of fixed X/Y coordinates.
*   **Key Techniques:**
    *   **Use Container Controls:** **Vertical and Horizontal Containers** are the foundation of responsive design. They automatically arrange child controls.
    *   **Set Sizing to "Fit to contents" or "Fill remaining space":** This allows controls to grow and shrink based on screen size and available space.
    *   **Use `App.Width` and `App.Height`:**
        *   Set a container's **`Width`** to `App.Width` and **`Height`** to `App.Height` to make it fill the screen.
        *   Use formulas for dynamic sizing: `App.Width * 0.8` for 80% of the screen width.
    *   **Use `Min`/`Max` Values:** Set a control's **`MinWidth`**, **`MaxWidth`**, **`MinHeight`**, and **`MaxHeight`** to prevent it from becoming too small or too large.
    *   **Reorder Content:** Use the **`Layout`** property on containers to change from horizontal to vertical arrangement on smaller screens.
    *   **Show/Hide Controls:** Use the **`Visible`** property with screen size checks (e.g., `If(App.Width < 600, false, true)`) to show simplified UI on mobile.

### **36. What is the Timer control, and how do you use it for delays?**

*   **What it is:** The **Timer** control runs in the background and can trigger actions at set intervals or after a delay.
*   **Key Properties:**
    *   **`Duration`**: How long the timer runs for (in milliseconds). E.g., `5000` for 5 seconds.
    *   **`AutoStart`**: If `true`, the timer starts as soon as the screen loads.
    *   **`Repeat`**: If `true`, the timer restarts automatically after it finishes.
    *   **`OnTimerEnd`**: The formula that runs when the timer completes its `Duration`.
*   **Use Case: Creating a Delay/Splash Screen:**
    1.  On a "Splash" screen, add a Timer.
    2.  Set `Duration` to `3000` (3 seconds) and `AutoStart` to `true`.
    3.  Set the `OnTimerEnd` property to `Navigate(HomeScreen)`.
    4.  The app will automatically navigate to the home screen after 3 seconds.

### **37. How do you implement dark mode in a canvas app?**

*   **Core Concept:** Use **global variables** to store color schemes and reference them throughout the app.
*   **Implementation Steps:**
    *   **Step 1: Create Theme Variables:** In the `App.OnStart` property, define color palettes.
        ```powerfx
        Set( LightTheme,
            {
                Background: White,
                Text: Black,
                Accent: ColorValue("#0078D4")
            }
        );
        Set( DarkTheme,
            {
                Background: Black,
                Text: White,
                Accent: ColorValue("#0078D4")
            }
        );
        Set( AppTheme, LightTheme ) // Set default theme
        ```
    *   **Step 2: Apply Theme to Controls:** Instead of hard-coding colors, set control properties to the variables.
        *   **Screen's `Fill`**: `AppTheme.Background`
        *   **Label's `Color`**: `AppTheme.Text`
        *   **Button's `Fill`**: `AppTheme.Accent`
    *   **Step 3: Create a Toggle Switch:** Add a Toggle control.
    *   **Step 4: Switch Themes:** Set the Toggle's `OnCheck` and `OnUncheck` properties.
        *   `OnCheck`: `Set( AppTheme, DarkTheme )`
        *   `OnUncheck`: `Set( AppTheme, LightTheme )`

### **38. What is the PDF Viewer control, and how do you display PDFs?**

*   **What it is:** A control that allows you to **display and interact with PDF files** directly within your Power App.
*   **Implementation:**
    *   **Step 1:** Insert a **PDF Viewer** control from the **"Display"** menu.
    *   **Step 2: Set the PDF Source (`Document` property):** You can reference a PDF from several locations:
        *   **From a Data Source:** If the PDF is stored in SharePoint or OneDrive as a hyperlink, use: `{Document: ThisItem.PDFFile.Url}`
        *   **From a Variable:** `{Document: varPDFURL}`
        *   **From a Text Input:** `{Document: TextInputPDFURL.Text}`
    *   **Features:** The control automatically provides a toolbar for zooming, printing, and downloading the PDF. The user can scroll through the pages.

### **39. How do you use icons and images dynamically in a gallery?**

*   **Core Concept:** Use the **`If()`** or **`Switch()`** function to change an icon or image based on the data in each gallery record.
*   **For Icons:**
    *   Add an **Icon** control inside your gallery template.
    *   Set the **`Icon`** property to a formula that evaluates the record's data.
        *   **Example (Status Indicator):**
            ```powerfx
            Switch( ThisItem.Status.Value,
                "New", Icon.Circle,
                "In Progress", Icon.Clock,
                "Complete", Icon.Checkmark
            )
            ```
    *   Set the **`Color`** property similarly: `If( ThisItem.Status.Value = "Complete", Green, Red )`
*   **For Images:**
    *   Add an **Image** control.
    *   Set the **`Image`** property to the URL of an image stored in your data source.
        *   **Example:** `ThisItem.ProductImage.Url` (if `ProductImage` is a SharePoint image column).
    *   You can also use `If()` logic to show different static images: `If( ThisItem.Category = "Hardware", HardwareIcon, SoftwareIcon )`

### **40. What is the Barcode Scanner control, and how do you integrate it?**

*   **What it is:** A control that uses the device's camera to **scan barcodes and QR codes**, returning the scanned data as text.
*   **Important Note:** This control only works on **mobile devices** running the Power Apps mobile app.
*   **Integration Steps:**
    *   **Step 1:** Insert a **Barcode Scanner** control from the **"Input"** menu.
    *   **Step 2: Configure the `OnScan` Property:** This formula runs after a successful scan. The scanned data is available in `BarcodeScanner.Value`.
    *   **Common `OnScan` Logic:**
        *   **Look Up a Product:** `Set( varScannedItem, LookUp( Products, BarcodeID = BarcodeScanner1.Value ) )`
        *   **Navigate to a Detail Screen:** `If( Not IsBlank(varScannedItem), Navigate(DetailScreen, ScreenTransition.None) )`
        *   **Add to a List:** `Collect( ScannedItems, { Barcode: BarcodeScanner1.Value, Time: Now() } )`
    *   **Step 3: Add a Trigger:** Typically, you add a button labeled "Scan" that sets the scanner's `Visible` property to `true` and starts the camera. The `OnScan` property automatically fires when a code is detected.
