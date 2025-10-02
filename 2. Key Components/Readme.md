# Canvas Apps: Key Components Deep Dive

## Introduction

Canvas Apps in Power Apps are built from five fundamental components that work together to create powerful, interactive applications. Understanding these components—Screens, Controls, Data Sources, Formulas, and Variables—is essential for building efficient and maintainable applications. This comprehensive guide explores each component in depth, providing practical examples, best practices, and real-world scenarios to help you master Canvas App development.

## Screens

### What Are Screens?

Screens are the fundamental building blocks of Canvas Apps. Think of screens as individual pages or views within your application. Each screen is a container that holds controls, displays data, and manages a specific part of your application's functionality. When a user navigates through your app, they're essentially moving from one screen to another.

### Screen Structure and Organization

**Naming Conventions**: Proper screen naming is crucial for maintainability. Use descriptive, consistent names that indicate the screen's purpose:
- `HomeScreen` - Landing page or main menu
- `ListScreen` - Displays lists or galleries of data
- `DetailScreen` - Shows detailed information about a selected item
- `FormScreen` - Data entry or editing
- `SettingsScreen` - Configuration options
- `ConfirmationScreen` - Success messages or confirmations

> [!TIP]
> Always use PascalCase for screen names and include the word "Screen" as a suffix. This makes screens immediately identifiable in formulas and helps organize your app structure.

**Screen Properties**: Every screen has properties that control its behavior and appearance:

**OnVisible**: Executes when the screen becomes visible. Use this for:
- Loading data specific to the screen
- Resetting variables
- Refreshing data sources
- Setting default values
- Initializing screen state

```
// Example OnVisible formula
Set(varSelectedDepartment, "All");
ClearCollect(colEmployees, Filter(Employees, Status = "Active"));
Set(varSearchText, "");
```

**OnHidden**: Executes when navigating away from the screen. Use this for:
- Cleaning up temporary data
- Saving user preferences
- Logging analytics
- Resetting search filters

**Fill**: Sets the background color of the screen
```
RGBA(245, 245, 245, 1)  // Light gray background
```

**BackgroundImage**: Sets an image as the screen background
```
"background.jpg"
```

> [!NOTE]
> The OnVisible property runs every time the screen is displayed, even when navigating back to it. Use this behavior strategically to refresh data or reset screen state.

### Screen Navigation Patterns

**Basic Navigation**: Moving between screens using the `Navigate()` function:

```
// Simple navigation
Navigate(DetailScreen)

// Navigation with transition effect
Navigate(DetailScreen, ScreenTransition.Fade)

// Navigation with transition and context
Navigate(DetailScreen, ScreenTransition.Cover, {SelectedItem: Gallery1.Selected})
```

**Screen Transitions**: Available transition types:
- `ScreenTransition.None` - Instant transition
- `ScreenTransition.Cover` - New screen slides in from right
- `ScreenTransition.CoverRight` - New screen slides in from left
- `ScreenTransition.Fade` - Smooth fade between screens
- `ScreenTransition.UnCover` - Current screen slides away
- `ScreenTransition.UnCoverRight` - Current screen slides right

> [!TIP]
> Use consistent transition types throughout your app for a professional, cohesive user experience. Reserve special transitions like Fade for important actions like successful submissions.

**Back Navigation**: Return to previous screens:
```
// Navigate back to previous screen
Back()

// Navigate back with specific transition
Back(ScreenTransition.UnCover)
```

**Passing Data Between Screens**: Multiple approaches to share data:

**Method 1: Context Variables** (Recommended for simple data)
```
// From source screen
Navigate(DetailScreen, ScreenTransition.Cover, {
    SelectedEmployee: Gallery1.Selected,
    ViewMode: "ReadOnly"
})

// On DetailScreen, access with:
SelectedEmployee.EmployeeName
ViewMode
```

**Method 2: Global Variables**
```
// Set before navigation
Set(gblSelectedEmployee, Gallery1.Selected);
Navigate(DetailScreen)

// Access on any screen
gblSelectedEmployee.EmployeeName
```

**Method 3: Collections**
```
// Store in collection
ClearCollect(colSelectedItems, Gallery1.SelectedItems);
Navigate(ProcessScreen)

// Access on target screen
First(colSelectedItems).ItemName
```

> [!IMPORTANT]
> Context variables passed through Navigate() are scoped to that screen only. Global variables (Set) and collections (ClearCollect) are accessible throughout the entire app.

### Screen Layout Strategies

**Fixed Layout**: Controls positioned at absolute pixel coordinates. Simple but not responsive across different screen sizes.

**Responsive Layout**: Using containers and flexible sizing to adapt to different devices:
- Horizontal containers for side-by-side elements
- Vertical containers for stacked elements
- Flexible height and width properties
- Responsive formulas based on screen dimensions

```
// Responsive width formula
Parent.Width * 0.8  // 80% of parent width

// Responsive position formula
(Parent.Width - Self.Width) / 2  // Center horizontally
```

**Mobile vs Tablet vs Desktop**: Design considerations for each:
- **Mobile**: Single column layouts, larger touch targets, simplified navigation
- **Tablet**: Multi-column possible, balance between touch and precision
- **Desktop**: Full multi-column layouts, smaller controls acceptable, mouse interaction

> [!WARNING]
> Designing for mobile-first is recommended. It's easier to expand a mobile design for larger screens than to shrink a desktop design for mobile use.

### Screen Best Practices

**Screen Count**: Keep your app focused and manageable
- Optimal: 5-10 screens for most business apps
- Maximum recommended: 15-20 screens
- Beyond 20 screens: Consider breaking into multiple apps or using a Model-Driven App

**Screen Purpose**: Each screen should have a clear, single purpose
- List/Browse screen
- Detail/View screen
- Create/Edit screen
- Settings screen
- Help/About screen

**Loading Performance**: Optimize screen load times
- Minimize OnVisible formula complexity
- Load only necessary data
- Use collections for data used multiple times
- Consider lazy loading for large datasets

> [!CAUTION]
> Complex OnVisible formulas can cause noticeable delays when navigating to a screen. If your screen takes more than 1-2 seconds to appear, optimize your OnVisible logic.

**Error Handling on Screens**: Implement graceful error handling
```
// OnVisible with error handling
If(
    IsEmpty(Errors(Employees)),
    Set(varLoadSuccess, true),
    Set(varLoadSuccess, false);
    Set(varErrorMessage, "Failed to load employee data")
)
```

## Controls

### Understanding Controls

Controls are the UI elements that users interact with in your Canvas App. They range from simple text labels to complex galleries and forms. Each control has properties that define its appearance, behavior, and data.

### Essential Control Types

**Display Controls**: Show information to users

**Label**: Displays static or dynamic text
```
// Label properties
Text: "Welcome, " & User().FullName
Font: Font.'Segoe UI'
Size: 18
Color: RGBA(0, 0, 0, 1)
Align: Align.Center
```

**HTML Text**: Displays rich formatted text with HTML
```
// HTML Text control
HtmlText: "<h2>Welcome</h2><p>This is <strong>formatted</strong> text</p>"
```

**Image**: Displays pictures or icons
```
// Image control
Image: User().Image  // User's profile picture
ImagePosition: ImagePosition.Fit
```

**Icon**: Displays vector icons from the built-in icon library
```
// Icon control
Icon: Icon.AddDocument
Color: RGBA(0, 120, 212, 1)
```

> [!TIP]
> Use icons instead of images when possible. They're vector-based, scale perfectly, and don't consume storage space.

**Input Controls**: Collect data from users

**Text Input**: Single-line text entry
```
// Text Input properties
Default: ""  // Initial value
HintText: "Enter employee name"
OnChange: Set(varSearchText, Self.Text)
Mode: TextMode.SingleLine
MaxLength: 50
```

**Text Area**: Multi-line text entry
```
// Text Area for longer content
Default: ""
HintText: "Enter detailed description"
Mode: TextMode.MultiLine
MaxLength: 500
```

**Date Picker**: Select dates
```
// Date Picker properties
DefaultDate: Today()
StartYear: Year(Today()) - 5
EndYear: Year(Today()) + 5
Format: DateTimeFormat.ShortDate
OnSelect: Set(varSelectedDate, Self.SelectedDate)
```

**Dropdown**: Select from predefined options
```
// Dropdown control
Items: ["Option 1", "Option 2", "Option 3"]
// Or from a data source
Items: Distinct(Products, Category)
Default: "Select category"
OnChange: Set(varSelectedCategory, Self.Selected.Value)
```

**Combo Box**: Multi-select dropdown with search
```
// Combo Box properties
Items: Employees
DefaultSelectedItems: []
SelectMultiple: true
IsSearchable: true
SearchFields: ["EmployeeName", "Department"]
OnChange: Set(varSelectedEmployees, Self.SelectedItems)
```

> [!NOTE]
> Combo Box is more powerful than Dropdown. It supports multi-select, search, and better performance with large datasets through delegation.

**Toggle**: On/off switch
```
// Toggle control
Default: false
TrueText: "Active"
FalseText: "Inactive"
OnCheck: Set(varIsActive, true)
OnUncheck: Set(varIsActive, false)
```

**Slider**: Select numeric value by dragging
```
// Slider control
Min: 0
Max: 100
Default: 50
OnChange: Set(varPriority, Self.Value)
```

**Rating**: Star-based rating input
```
// Rating control
Max: 5
Default: 0
OnChange: Patch(Reviews, LookUp(Reviews, ID = varReviewID), {Rating: Self.Value})
```

**Action Controls**: Trigger actions and navigation

**Button**: Execute actions when clicked
```
// Button properties
Text: "Submit"
OnSelect: Patch(Employees, Defaults(Employees), {
    EmployeeName: TextInput1.Text,
    Department: Dropdown1.Selected.Value,
    HireDate: DatePicker1.SelectedDate
})
DisplayMode: If(IsBlank(TextInput1.Text), DisplayMode.Disabled, DisplayMode.Edit)
```

**Icon (as button)**: Compact button alternative
```
// Icon as action button
Icon: Icon.Save
OnSelect: SubmitForm(Form1)
Tooltip: "Save changes"
```

> [!IMPORTANT]
> Always provide visual feedback when buttons are clicked. Use DisplayMode.Disabled during processing or UpdateContext to show loading states.

**Layout Controls**: Organize other controls

**Container**: Groups controls for layout and responsive design
```
// Container properties
LayoutDirection: LayoutDirection.Vertical
LayoutGap: 10
LayoutAlignItems: LayoutAlignItems.Stretch
FlexibleWidth: true
```

**Gallery**: Displays repeating data from a collection or data source
```
// Gallery properties
Items: Filter(Employees, StartsWith(EmployeeName, SearchBox.Text))
TemplateSize: 100
OnSelect: Navigate(DetailScreen, ScreenTransition.Cover, {
    SelectedEmployee: ThisItem
})
```

**Vertical Gallery**: Standard top-to-bottom gallery
**Horizontal Gallery**: Left-to-right gallery for carousels
**Flexible Height Gallery**: Adjusts row height based on content

> [!TIP]
> Use Flexible Height Gallery when your data has varying content lengths, like descriptions or comments. It provides a better user experience than fixed-height galleries.

**Form**: Complete data entry solution with automatic field generation
```
// Form properties
DataSource: Employees
Item: Gallery1.Selected  // For editing
DefaultMode: FormMode.New  // For creating
OnSuccess: Navigate(HomeScreen); Notify("Employee created successfully", NotificationType.Success)
OnFailure: Notify("Failed to save: " & Form1.Error, NotificationType.Error)
```

**Advanced Controls**: Specialized functionality

**Camera**: Capture photos
```
// Camera control
OnSelect: Set(varPhoto, Self.Photo)
// Then save to data source
Patch(Inspections, Defaults(Inspections), {
    InspectionPhoto: varPhoto
})
```

**Barcode Scanner**: Read barcodes and QR codes
```
// Barcode scanner
OnScan: Set(varScannedCode, Self.Value);
        LookUp(Products, ProductCode = varScannedCode)
BarcodeType: BarcodeType.Code128
```

**PDF Viewer**: Display PDF documents
```
// PDF Viewer
Document: "https://example.com/document.pdf"
// Or from data source
Document: Gallery1.Selected.ContractPDF
```

**Power BI Tile**: Embed Power BI reports
```
// Power BI Tile properties
Workspace: "workspace-guid"
Report: "report-guid"
Tile: "tile-guid"
```

**Microphone**: Record audio
```
// Microphone control
OnStop: Set(varAudioRecording, Self.Audio);
        Patch(VoiceNotes, Defaults(VoiceNotes), {
            AudioFile: varAudioRecording
        })
```

### Control Properties Deep Dive

**Common Properties**: Every control has these base properties

**X, Y**: Position on the screen (pixels from top-left)
```
X: 20
Y: 100
```

**Width, Height**: Size of the control
```
Width: Parent.Width - 40  // Responsive width
Height: 50
```

**Visible**: Show or hide the control
```
Visible: varUserRole = "Admin"  // Show only to admins
Visible: !IsBlank(TextInput1.Text)  // Show when input has value
```

**DisplayMode**: Control whether users can interact
- `DisplayMode.Edit` - Fully interactive
- `DisplayMode.View` - Visible but not interactive
- `DisplayMode.Disabled` - Grayed out and not interactive

```
DisplayMode: If(Form1.Mode = FormMode.View, DisplayMode.View, DisplayMode.Edit)
```

> [!WARNING]
> DisplayMode.Disabled and Visible = false are different. Disabled controls are visible but grayed out, while Visible = false completely hides the control.

**Color and Style Properties**: Visual appearance

**Fill**: Background color
**Color**: Text or foreground color
**BorderColor**: Border color
**BorderThickness**: Border width in pixels
**BorderStyle**: Solid, dashed, dotted, or none

```
// Professional styling
Fill: RGBA(255, 255, 255, 1)
Color: RGBA(51, 51, 51, 1)
BorderColor: RGBA(204, 204, 204, 1)
BorderThickness: 1
BorderStyle: BorderStyle.Solid
```

**Theme Colors**: Using color variables for consistency
```
// Define in App OnStart
Set(varPrimaryColor, RGBA(0, 120, 212, 1));
Set(varSecondaryColor, RGBA(51, 51, 51, 1));
Set(varSuccessColor, RGBA(16, 124, 16, 1));
Set(varErrorColor, RGBA(168, 0, 0, 1));

// Use throughout app
Button.Fill: varPrimaryColor
Label.Color: varSecondaryColor
```

> [!TIP]
> Define your color scheme in App.OnStart as variables. This makes it easy to maintain consistent branding and quickly change colors app-wide.

**Behavior Properties**: Control interaction

**OnSelect**: Triggers when the control is clicked/tapped
**OnChange**: Triggers when the value changes
**OnCheck/OnUncheck**: Toggle-specific events
**OnScan**: Barcode scanner event
**OnStream**: Camera stream event

**TabIndex**: Controls keyboard navigation order
```
TabIndex: 1  // First control to receive focus
TabIndex: 0  // Excluded from tab navigation
```

**Accessibility Properties**: Making apps inclusive

**AccessibleLabel**: Screen reader description
**Live**: Announce changes to screen readers
**Role**: Semantic role for accessibility

```
Button.AccessibleLabel: "Submit employee information form"
Label.Live: Live.Polite  // Announce content changes
Icon.Role: AccessibilityRole.Button
```

> [!IMPORTANT]
> Always set AccessibleLabel for interactive controls. Screen reader users need descriptive labels to understand what each control does.

### Control Best Practices

**Naming Controls**: Use consistent, descriptive names
- **Bad**: `Button1`, `TextInput3`, `Label5`
- **Good**: `btnSubmit`, `txtEmployeeName`, `lblWelcome`

**Prefixes to use**:
- `btn` - Button
- `txt` - Text Input
- `lbl` - Label
- `drp` - Dropdown
- `gal` - Gallery
- `frm` - Form
- `ico` - Icon
- `con` - Container

```
// Well-named controls are self-documenting
btnSaveEmployee.OnSelect: ...
txtEmployeeName.Default: ...
galEmployeeList.Items: ...
```

> [!TIP]
> Adopt a naming convention on day one of your project. Renaming controls later becomes tedious and error-prone as your app grows.

**Control Organization**: Keep your control tree organized
- Group related controls in containers
- Use descriptive group names
- Limit nesting depth (3-4 levels maximum)
- Order controls logically in the control tree

**Control Reusability**: Create components for repeated patterns
```
// Instead of duplicating buttons throughout app
// Create a StandardButton component with:
- Consistent styling
- Standard size
- Common behavior
- Reusable across screens
```

**Performance Considerations**: Control count matters
- **Optimal**: Under 200 controls per screen
- **Maximum**: 500 controls per screen
- Beyond 500: Performance degrades significantly

> [!CAUTION]
> Gallery templates count toward your control limit. A gallery with 100 visible items and 5 controls per template = 500 controls. Use Virtualization to improve performance.

**Responsive Design with Controls**: Adapt to screen sizes
```
// Responsive button width
Width: If(App.Width < 768, Parent.Width - 40, 300)

// Responsive font size
Size: If(App.Width < 768, 14, 18)

// Responsive layout
Container.LayoutDirection: If(App.Width < 768, 
    LayoutDirection.Vertical, 
    LayoutDirection.Horizontal
)
```

## Data Sources

### Understanding Data Sources

Data sources are external systems that store your application's data. Canvas Apps can connect to hundreds of different data sources, from Microsoft services to third-party platforms to custom APIs. Understanding how to work with data sources effectively is crucial for building robust applications.

### Connection Types

**Standard Connectors**: Included with Microsoft 365 licenses
- SharePoint
- Microsoft 365 (Mail, Calendar, Users)
- OneDrive for Business
- Teams
- Planner
- Outlook

**Premium Connectors**: Require Power Apps premium licenses
- Microsoft Dataverse
- SQL Server
- Oracle
- Azure services
- Salesforce
- SAP
- Custom connectors

> [!IMPORTANT]
> Users need appropriate licenses to run apps that use premium connectors. Check connector requirements before distributing your app widely.

### Microsoft Dataverse

**Why Dataverse**: The recommended data source for Canvas Apps

**Advantages**:
- Native integration with Power Platform
- Relational data with strong typing
- Built-in security (row-level, field-level)
- Audit logging
- Business rules and calculated fields
- Automatic delegation
- Rich metadata
- Scalability for enterprise use

**Connecting to Dataverse**:
```
// Add data source
1. Add data → Dataverse
2. Select tables (e.g., Accounts, Contacts)
3. Tables appear in data panel
4. Reference in formulas: Accounts, Contacts
```

**Working with Dataverse Tables**:
```
// Retrieve all records
ClearCollect(colAccounts, Accounts)

// Filter records
Filter(Accounts, 'Account Type' = "Customer")

// Look up single record
LookUp(Contacts, 'Contact ID' = varContactID)

// Create record
Patch(Accounts, Defaults(Accounts), {
    'Account Name': txtAccountName.Text,
    'Primary Contact': LookUp(Contacts, 'Contact ID' = drpContact.Selected.ID)
})

// Update record
Patch(Accounts, LookUp(Accounts, 'Account ID' = varAccountID), {
    'Account Status': "Active"
})

// Delete record
Remove(Accounts, LookUp(Accounts, 'Account ID' = varAccountID))
```

> [!NOTE]
> Dataverse table and column names with spaces must be enclosed in single quotes. Use 'Account Name' not AccountName.

**Relationships in Dataverse**:
```
// Navigate one-to-many relationship
Gallery1.Selected.Contacts  // All contacts for selected account

// Filter related records
Filter(Accounts, 'Primary Contact'.'Contact Name' = "John Smith")

// Access related record fields
LookUp(Orders, 'Order ID' = varOrderID).'Customer Account'.'Account Name'
```

### SharePoint

**Common Use Cases**:
- Document libraries
- Simple lists
- Lightweight data storage
- When SharePoint is already in use

**Connecting to SharePoint**:
```
// Add SharePoint connection
1. Add data → SharePoint
2. Enter site URL
3. Select lists/libraries
4. Lists appear in data panel
```

**SharePoint List Operations**:
```
// Read items
Filter('Task List', Status = "In Progress")

// Create item
Patch('Task List', Defaults('Task List'), {
    Title: txtTaskName.Text,
    'Assigned To': {
        '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedUser",
        Claims: "i:0#.f|membership|" & User().Email,
        DisplayName: User().FullName,
        Email: User().Email
    }
})

// Update item
Patch('Task List', LookUp('Task List', ID = varTaskID), {
    Status: "Completed",
    'Completion Date': Today()
})
```

> [!WARNING]
> SharePoint has a delegation limit for complex queries. Operations beyond 2,000 items may not return complete results unless you use delegation-friendly functions.

**SharePoint Person/Group Columns**:
```
// Set person column
{
    '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedUser",
    Claims: "i:0#.f|membership|user@domain.com",
    DisplayName: "User Name",
    Email: "user@domain.com"
}

// Read person column
Gallery1.Selected.'Assigned To'.DisplayName
Gallery1.Selected.'Assigned To'.Email
```

**Document Libraries**:
```
// Get files
Filter('Shared Documents', 'Folder Path' = "/sites/site/Shared Documents/Folder")

// Upload file
'SharePointConnection'.CreateFile(
    "Shared Documents",
    "filename.pdf",
    Camera1.Photo
)

// Download file
'SharePointConnection'.GetFileContent(Gallery1.Selected.ID)
```

### SQL Server

**Use Cases**:
- Existing corporate databases
- Large datasets requiring complex queries
- Integration with legacy systems
- Advanced data processing needs

**Connecting to SQL Server**:
```
// Add SQL Server connection
1. Add data → SQL Server
2. Enter server and database
3. Authentication (Windows or SQL)
4. Select tables
```

> [!IMPORTANT]
> SQL Server connections require an on-premises data gateway if the server isn't cloud-accessible. Plan gateway deployment and maintenance accordingly.

**SQL Server Operations**:
```
// Read data
Filter(Employees, Department = "Sales")

// Stored procedure
SQLConnection.ExecuteProcedure("sp_GetEmployeesByDepartment", {
    DepartmentID: 5
})

// Create record
Patch(Employees, Defaults(Employees), {
    EmployeeName: txtName.Text,
    DepartmentID: drpDepartment.Selected.ID,
    HireDate: dpHireDate.SelectedDate
})
```

**SQL Views**: Use database views for complex queries
```
// Instead of complex filters in Canvas App
Filter(Orders, Year(OrderDate) = 2024 && Status = "Shipped")

// Create SQL view and use directly
vw_2024ShippedOrders  // Cleaner and potentially faster
```

### Excel

**Use Cases**:
- Quick prototypes
- Small reference data
- When data is already in Excel
- User-maintained datasets

**Connecting to Excel**:
```
// Requirements:
1. Excel file in OneDrive or SharePoint
2. Data formatted as a table (Insert → Table)
3. Table must have headers

// Add connection
1. Add data → OneDrive for Business
2. Select Excel file
3. Choose table(s)
```

**Excel Operations**:
```
// Read data
Filter(Products, Category = "Electronics")

// Add row
Patch(Products, Defaults(Products), {
    ProductName: txtProduct.Text,
    Price: Value(txtPrice.Text),
    Category: drpCategory.Selected.Value
})

// Update row
Patch(Products, LookUp(Products, ProductID = varID), {
    Price: Value(txtNewPrice.Text)
})
```

> [!CAUTION]
> Excel tables have limitations: maximum 2,000 rows for delegation, slower performance than databases, and potential concurrency issues with multiple users.

### Microsoft 365 Connectors

**Office 365 Users**: Access user directory
```
// Get user information
Office365Users.MyProfile()
Office365Users.UserProfile(varUserEmail)
Office365Users.SearchUser({searchTerm: txtSearch.Text})

// User properties
Office365Users.MyProfile().DisplayName
Office365Users.MyProfile().Mail
Office365Users.MyProfile().JobTitle
Office365Users.MyProfile().Department
```

**Office 365 Outlook**: Email integration
```
// Send email
Office365Outlook.SendEmailV2(
    txtRecipient.Text,
    "Subject Line",
    "Email body content",
    {
        Importance: "High",
        Attachments: [
            {
                Name: "report.pdf",
                ContentBytes: PDF1.Document
            }
        ]
    }
)

// Read emails
Office365Outlook.GetEmails({
    folderPath: "Inbox",
    top: 50,
    searchQuery: "subject:urgent"
})
```

**SharePoint**: Beyond lists, site operations
```
// Get site users
SharePoint.GetUsers("https://company.sharepoint.com/sites/site")

// Create folder
SharePoint.CreateFolder(
    "Shared Documents",
    "NewFolder"
)
```

### Custom Connectors

**When to Use Custom Connectors**:
- Integrate with proprietary systems
- Connect to REST APIs not in connector library
- Extend existing connectors with custom operations
- Create reusable API integrations

**Creating Custom Connectors**:
```
// Process:
1. Define API endpoint and authentication
2. Create connector in Power Platform
3. Test operations
4. Share with app makers

// Example: Custom CRM API
CustomCRM.GetCustomers()
CustomCRM.CreateOrder({
    CustomerID: varCustomerID,
    OrderDate: Today(),
    Items: colCartItems
})
```

> [!TIP]
> Custom connectors can be certified and shared across your organization. Invest time in creating well-documented, reusable connectors for your most common integrations.

### Data Source Best Practices

**Choosing the Right Data Source**:

**Use Dataverse when**:
- Building enterprise applications
- Need strong relational data model
- Require row-level security
- Want audit trails
- Need to integrate with Dynamics 365
- Building Model-Driven Apps alongside Canvas Apps

**Use SharePoint when**:
- Simple lists (< 2,000 items)
- Already using SharePoint extensively
- Need document storage with metadata
- Quick departmental solutions
- List data that business users maintain

**Use SQL Server when**:
- Have existing corporate database
- Complex reporting requirements
- Large datasets (millions of records)
- Need SQL Server features (triggers, views, stored procedures)
- Integration with other enterprise systems

**Use Excel when**:
- Prototyping
- Reference data (lookup lists)
- Small datasets (< 500 rows)
- User-maintained data
- Temporary solutions

> [!WARNING]
> Don't use Excel for production apps with multiple concurrent users. It's not designed for high-concurrency scenarios and can lead to data corruption.

**Connection Management**:

**Sharing Apps with Connections**: When sharing your app, you must also share connections
```
// Process:
1. Share app with users/groups
2. Share underlying connections
3. Users may need to create their own connections for certain connectors
4. Document required permissions
```

**Connection Security**: Principle of least privilege
- Only connect to necessary data sources
- Use service accounts for SQL/database connections
- Don't embed credentials in formulas
- Use Azure Key Vault for sensitive connection strings

**Multiple Environments**: Manage connections across Dev/Test/Prod
```
// Use environment variables or manual update
// Development: Connect to Dev database
// Test: Connect to Test database  
// Production: Connect to Prod database
```

> [!IMPORTANT]
> Always test connection changes in development before promoting to production. Broken connections can make your app completely unusable.

**Data Refresh and Caching**:

**Explicit Refresh**: Force data source refresh
```
// Refresh specific data source
Refresh(Employees)

// Refresh multiple sources
Refresh(Employees); Refresh(Departments); Refresh(Locations)
```

**Automatic Refresh**: Data refreshes automatically in certain scenarios
- When screen becomes visible
- After Patch, Remove operations
- When filter conditions change

**Caching with Collections**: Store data in memory
```
// Load data once
ClearCollect(colEmployees, Employees)

// Use collection instead of direct data source
Filter(colEmployees, Department = "IT")  // Fast, in-memory

// Refresh when needed
ClearCollect(colEmployees, Employees)
```

> [!TIP]
> Use collections for data that doesn't change frequently during a session. This reduces API calls, improves performance, and works offline.

**Delegation Considerations**:

**What is Delegation**: Power Apps sends query to data source, which processes and returns results. This allows working with millions of records.

**Delegation-Friendly Functions**:
- Filter
- Search
- LookUp
- Sort
- SortByColumns

**Non-Delegable Functions**:
- CountRows (use CountIf instead when delegable)
- Sum, Average, Min, Max on entire table
- First, FirstN beyond delegation limit
- Complex nested filters

```
// Delegable - processed by data source
Filter(Employees, Department = "Sales")  // ✓

// Non-delegable - processed locally
Filter(Employees, Len(EmployeeName) > 10)  // ✗

// Delegable alternative
Filter(Employees, EmployeeName <> "" && Len(EmployeeName) > 10)  // Still ✗
// Better: Add a calculated column in data source
```

> [!WARNING]
> The blue underline warning in formulas indicates delegation issues. Your formula will only process the first 500 (or 2,000 with setting) records from the data source.

**Delegation Limit Setting**:
```
// App Settings → Advanced Settings → Data row limit
// Default: 500
// Maximum: 2,000
// Set to maximum for production apps
```

## Formulas

### Understanding Power Fx

Power Fx is the formula language used in Canvas Apps. It's based on Excel formulas, making it familiar to business users while being powerful enough for complex logic. Formulas are the heart of your app, controlling everything from simple calculations to complex business logic.

### Formula Fundamentals

**Formula Syntax**: Basic structure
```
FunctionName(Parameter1, Parameter2, ...)
```

**Operators**: Mathematical and logical operations
```
// Mathematical
5 + 3  // Addition: 8
10 - 4  // Subtraction: 6
6 * 7  // Multiplication: 42
20 / 5  // Division: 4
2 ^ 3  // Exponentiation: 8

// Comparison
x = 5  // Equals
x <> 5  // Not equals
x >
