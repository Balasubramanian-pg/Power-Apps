### **81. What is a site map, and how do you customize it?**

*   **What it is:** The **site map** is the navigation menu (the left-hand pane) in a model-driven app. It defines the structure and grouping of all the areas, sub-areas, and components that users can access.
*   **How to Customize It:**
    1.  **Access the Sitemap Designer:** In the model-driven app designer, select the **"Navigation"** component in the tree view on the left.
    2.  **Understand the Hierarchy:** The sitemap is structured as:
        *   **Area:** The top-level navigation group (e.g., "Sales," "Service," "Marketing"). These appear as the main tabs at the top level of the left pane.
        *   **Sub-area:** Groups within an Area (e.g., "Accounts," "Contacts" within the "Sales" area). These appear as expandable sections.
        *   **Group:** A visual container inside a Sub-area to further organize links.
        *   **URL/Entity/Dashboard/Page:** The actual links that users click on. These can point to a table (entity) view, a dashboard, a custom page, or an external URL.
    3.  **Drag-and-Drop Editing:** You can add, remove, and reorder these elements using a visual designer. To add a new table, you simply drag the "Entity" component from the toolbox into the desired Sub-area.
    4.  **Publish Changes:** After customizing, you must **"Save"** and **"Publish"** the sitemap for the changes to become visible to users.

### **82. How do you create a custom entity in Dataverse?**

*   **What it is:** A **custom entity** (now commonly referred to as a **custom table**) is a container you create to store a specific type of data that isn't covered by the standard tables (like Account or Contact).
*   **Creation Process:**
    1.  **Navigate to Tables:** In [make.powerapps.com](https://make.powerapps.com), go to **"Dataverse" > "Tables"**.
    2.  **Create New Table:** Click **"+ New table"**.
    3.  **Define Table Properties:**
        *   **Display name:** The user-friendly name (e.g., "Project").
        *   **Plural name:** The plural display name (e.g., "Projects").
        *   **Primary Name Column:** The default column used to identify a record (e.g., "Project Name"). This is like the "Title" column in SharePoint.
    4.  **Configure Ownership (Crucial for Security):**
        *   **User:** Records are owned by individual users. Enables row-level security where users can only see their own records.
        *   **Organization:** Records are owned by the organization and are visible to all users with appropriate privileges.
    5.  **Click "Create":** The table is created with the primary name column and system columns (like `Created On`). You can then add custom columns to it.

### **83. What is a business rule, and how do you implement it?**

*   **What it is:** A **business rule** provides a no-code way to apply custom logic and validation to a Dataverse table's forms. It allows you to automate processes without writing code.
*   **Capabilities:**
    *   **Set field values** (e.g., automatically calculate a total).
    *   **Show or hide fields.**
    *   **Set field requirement levels** (e.g., make a field required only if another field has a specific value).
    *   **Validate data** and show error messages.
    *   **Enable or disable fields.**
*   **Implementation Steps:**
    1.  **Navigate to Business Rules:** Open your table in the Power Apps portal, go to the **"Business rules"** tab.
    2.  **Create New Rule:** Click **"New business rule"**.
    3.  **Design the Logic:** Use the visual designer to drag and configure components:
        *   **Condition:** The "IF" part of your rule (e.g., "If Status equals 'Active'").
        *   **Action:** The "THEN" part (e.g., "Then show the 'Reason' field," "Then set the 'Expiry Date' field to 30 days from today").
    4.  **Scope and Activate:**
        *   Set the scope to **"All Forms"** or a specific form.
        *   **Activate** the business rule. It will now run automatically whenever a form is loaded or data is changed, based on your conditions.

### **84. How do you create a custom workflow in Power Automate for model-driven apps?**

*   **Concept:** While "classic workflows" exist in Dataverse, the modern, powerful way to create server-side automation is using **Power Automate cloud flows** triggered from model-driven apps.
*   **Process:**
    1.  **Create a Cloud Flow:** In Power Automate, create a new flow.
    2.  **Choose the Trigger:** Use the **"When a row is added, modified, or deleted"** trigger from the Dataverse connector. This is the most common trigger for background automation.
        *   Specify the **Table name** (e.g., "Projects").
        *   Specify the **Scope** (e.g., "Organization" to run on any record, or "User" to run only on records the triggering user owns).
    3.  **Add Actions:** Build your workflow logic. This can include:
        *   **Creating/Updating** other rows in Dataverse.
        *   **Sending emails** or **Teams notifications**.
        *   **Calling external APIs** via the HTTP action.
        *   **Running an Office Script** on an Excel file.
    4.  **Test and Activate:** Test the flow with sample data and turn it on. It will now run automatically in the background based on the Dataverse trigger you configured.

### **85. What is a dashboard, and how do you build one?**

*   **What it is:** A **dashboard** in a model-driven app is a single screen that displays multiple visualizations (charts and lists) from various tables, providing an at-a-glance view of key business metrics.
*   **Types of Dashboards:**
    *   **Interactive Dashboards:** Allow users to filter and drill down into the data.
    *   **Classic Dashboards:** A fixed view of data.
*   **How to Build One:**
    1.  **Navigate to Dashboards:** In [make.powerapps.com](https://make.powerapps.com), go to **"Dataverse" > "Dashboards"**.
    2.  **Create New Dashboard:** Click **"New dashboard"** and choose a layout (e.g., 2-column, 3-column).
    3.  **Add Components:** In the designer, click **"Add Component"** for each cell in the layout. You can add:
        *   **Chart:** Select an existing system chart or a custom one you've created for a table.
        *   **List:** Embeds a view from a table, showing a list of relevant records.
        *   **iFrame:** Embeds a web resource or external website.
        *   **Web Resource:** Embeds a custom HTML/JavaScript file.
    4.  **Configure Components:** For each chart or list, you select the source table and the specific chart or view.
    5.  **Assign to App:** Once saved, the dashboard will be available to add to your model-driven app's sitemap.

### **86. How do you implement role-based security in model-driven apps?**

*   **Concept:** **Role-based security** in model-driven apps is enforced by the underlying **Dataverse**. The app itself merely reflects the data permissions defined by the user's security roles.
*   **Implementation Process:**
    1.  **Define Security Roles:** In the Power Platform Admin Center, go to your environment and navigate to **"Settings" > "Security" > "Security Roles"**.
    2.  **Create or Modify a Role:** Create a new role (e.g., "Regional Sales Manager") or edit an existing one.
    3.  **Set Table-Level Permissions:** For each relevant table (e.g., Account, Opportunity, your custom "Project" table), set the **CRUD** permissions:
        *   **Create, Read, Write, Delete, Append, Append To, Assign, Share.**
        *   The critical permission is **"Read"**, which can be set at different levels:
            *   **Organization:** See all records.
            *   **Parent: Child Business Units:** See records in your BU and subordinate BUs.
            *   **Business Unit:** See records only in your BU.
            *   **User:** See only records you own.
    4.  **Assign Roles to Users/Groups:** Assign the security roles to individual users or, more efficiently, to **Azure Active Directory Groups**. Users inherit the permissions of all roles assigned to them.

### **87. What is Duet AI, and how do you use it in model-driven apps?**

*   **What it is:** **Duet AI** (now often referred to in the context of Microsoft Copilot for Microsoft 365) is an AI assistant that integrates across the Microsoft Cloud, including the Power Platform. In model-driven apps, it can help users interact with data using natural language.
*   **How to Use It (Example Scenarios):**
    *   **Natural Language Search & Filtering:** Instead of building complex advanced find views, users can type queries like **"Show me all opportunities in the Western region with a value over $50,000 that are closing this month"** directly into a search bar powered by AI.
    *   **Summarizing Record Data:** While viewing a long case or opportunity record, an AI copilot could summarize the key details, recent activities, and next steps.
    *   **Generating Content:** Assist sales reps by drafting email content to a client based on the data within an opportunity record.
*   **Implementation:** This functionality is typically enabled at the platform level by administrators. App makers then expose these AI capabilities within their apps by adding the relevant Copilot controls or enabling the features in views and forms.

### **88. How do you customize forms and views in model-driven apps?**

*   **Customizing Forms (The UI for a single record):**
    1.  Open your table in the Power Apps portal.
    2.  Go to the **"Forms"** tab and select a form (e.g., the "Main" form).
    3.  The **Form Designer** opens. Here you can:
        *   **Add, remove, and rearrange fields** into different sections and tabs.
        *   **Configure field properties:** Make them read-only, required, or set their visibility using logic.
        *   **Add custom components:** Such as PCF controls or web resources.
        *   **Add a header** or a timeline control.
    4.  **Save and Publish** the form.
*   **Customizing Views (The lists of records):**
    1.  From your table, go to the **"Views"** tab.
    2.  Select a view (e.g., "Active Accounts").
    3.  The **View Designer** allows you to:
        *   **Add or remove columns.**
        *   **Reorder columns** by dragging them.
        *   **Modify sorting** (e.g., sort by "Created On" descending).
        *   **Edit filters** to change which records are displayed using a graphical filter builder.
        *   **Change the default view** for the table.

### **89. What is Power Apps Grid Control, and how do you use it?**

*   **What it is:** The **Power Apps Grid Control** (also known as the Editable Grid) is a modern control that can be added to a main form or as a subgrid. It allows users to **view and edit multiple records in a spreadsheet-like experience** without opening each record individually.
*   **Key Features:**
    *   **Inline Editing:** Click a cell and edit the value directly.
    *   **Bulk Editing:** Change a value for multiple selected rows at once.
    *   **Sorting and Filtering:** Users can easily sort columns and apply filters.
    *   **Excel-like Experience:** Provides a familiar interface for data manipulation.
*   **How to Use It:**
    1.  **On a Main Form:** In the form designer, you can add the grid control to display related rows. For example, on a Project form, you could add a grid of related "Project Tasks" and edit them inline.
    2.  **As a Subgrid:** You can add the grid control to a form in a subgrid section to show related records from another table. This is the most common use case, allowing users to manage child records (like tasks or contacts) directly from the parent record's form.

### **90. How do you implement auditing in Dataverse?**

*   **What it is:** **Dataverse Auditing** is a feature that automatically tracks changes made to records, including what was changed, who changed it, and when. This is crucial for compliance and troubleshooting.
*   **Implementation Steps:**
    1.  **Enable Auditing at the Environment Level:** An administrator must go to the **Power Platform Admin Center** -> **Your Environment** -> **"Settings"** -> **"Audit and logs"** -> **"Audit settings"** and turn on **"Start Auditing"**.
    2.  **Enable Auditing for Specific Tables:** For each table you want to audit, open the table in the maker portal. Go to its **"Settings"** and ensure **"Auditing"** is enabled under the "Data" section.
    3.  **Enable Auditing for Specific Columns (Optional):** For each table, you can drill down and select which specific columns should be audited. This is important for performance and relevance, as auditing every field can generate significant data.
*   **Viewing Audit Data:**
    *   **Via the UI:** In a model-driven app, you can view the audit history for a specific record by going to the **"Related"** menu and selecting **"Audit History"**.
    *   **Via Advanced Find:** You can create an Advanced Find query on the "Audit" table to generate custom audit reports.
    *   **Via Power BI:** You can connect Power BI to the Dataverse audit table to create comprehensive compliance dashboards.
