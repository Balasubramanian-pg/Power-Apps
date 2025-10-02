### **1. Power Apps Basics**
**Types of Apps**:
- **Canvas Apps**: Start from scratch, full UI control (use for custom designs).
- **Model-Driven Apps**: Built on Dataverse, component-focused (use for complex business processes).
- **Portals**: External-facing websites for users outside your org.

**Key Components**:
| Component | Purpose |
|-----------|---------|
| **Screens** | Containers for controls (e.g., `HomeScreen`, `DetailScreen`). |
| **Controls** | UI elements (e.g., `TextInput`, `Button`, `Gallery`, `Form`). |
| **Data Sources** | Connect to Dataverse, SharePoint, SQL, Excel, etc. |
| **Formulas** | Excel-like expressions (e.g., `Filter()`, `Search()`, `Patch()`). |
| **Variables** | Store temporary data (`Set(varName, value)`; `UpdateContext({varName: value})`). |

### **2. Core Formulas**
**Essential Functions**:
| Category | Functions | Example |
|----------|-----------|---------|
| **Data** | `Filter()`, `Search()`, `Sort()`, `LookUp()` | `Filter(Employees, Department="HR")` |
| **Logical** | `If()`, `Switch()`, `IsBlank()`, `IsEmpty()` | `If(Age > 18, "Adult", "Minor")` |
| **Text** | `Concatenate()`, `Left()`, `Right()`, `Substitute()` | `Concatenate(FirstName, " ", LastName)` |
| **Math** | `Sum()`, `Average()`, `Round()`, `Mod()` | `Sum(Sales, Amount)` |
| **Dates** | `Today()`, `Now()`, `DateDiff()`, `DateAdd()` | `DateDiff(Today(), Deadline, Days)` |
| **Collections** | `ClearCollect()`, `Collect()`, `Clear()` | `ClearCollect(MyList, {Name:"Alice", Age:30})` |
| **Patch/Update** | `Patch()`, `Update()`, `Remove()` | `Patch(Employees, LookUp(Employees, ID=1), {Salary: 50000})` |

**Special Variables**:
- `User()`: Current user’s info (e.g., `User().Email`).
- `Location`: Device GPS data (e.g., `Location.Latitude`).
- `Param()`: URL parameters (e.g., `Param("id")`).

---

### **3. Data Sources & Connectors**
**Common Connectors**:
| Connector | Use Case |
|-----------|----------|
| **Dataverse** | Enterprise-grade data storage (tables, relationships). |
| **SharePoint** | Lists/libraries (use `Filter()` for large datasets). |
| **SQL Server** | Direct queries (use ` delegation warnings`). |
| **Excel/OneDrive** | Quick prototyping (not for production). |
| **REST APIs** | Custom connectors (use `JSON()` and `ParseJSON()`). |
| **Power Automate** | Trigger flows from apps (use `PowerAutomate.Run()`). |

**Delegation**:
- **Delegable Functions**: `Filter`, `Search`, `Sort`, `LookUp` (work on large datasets).
- **Non-Delegable**: `IsBlank`, `Left`, `Right` (may fail with >500 records).
- **Workaround**: Use `Collections` or Dataverse for non-delegable ops.

### **4. Controls & UI**
**Key Controls**:
| Control | Purpose | Example Property |
|---------|---------|------------------|
| **Gallery** | Display lists (vertical/horizontal). | `Items: Filter(DataSource, Status="Active")` |
| **Form** | Edit/create records (auto-generates fields). | `DataSource: Employees; Item: SelectedEmployee` |
| **Button** | Trigger actions. | `OnSelect: Patch(Employees, Defaults(Employees), {Name: "New"})` |
| **Text Input** | User input. | `Default: "Enter name"; OnChange: Set(varName, Value)` |
| **Dropdown** | Select from options. | `Items: ["Option1", "Option2"]; SelectedValue: varSelected` |
| **Toggle** | Boolean switch. | `OnCheck: Set(varEnabled, true)` |
| **Chart** | Visualize data. | `Items: GroupBy(Sales, "Category", "TotalSales")` |

**Responsive Design**:
- Use `%` for widths/heights (e.g., `Width: Parent.Width * 0.5`).
- **Containers**: Group controls for consistent layout.
- **Theming**: Apply corporate colors/fonts via `Theme` property.

### **5. Model-Driven Apps**
**Key Concepts**:
- **Site Map**: Navigation structure (edit in [App Designer](https://make.powerapps.com)).
- **Entities (Tables)**: Core data objects (e.g., `Account`, `Contact`).
- **Forms/Views**: Define how data is displayed/edited.
- **Business Rules**: Client-side logic (e.g., show/hide fields).
- **Process Flows**: Guide users through stages (e.g., "Lead to Opportunity").

**Customizations**:
- **Command Bar**: Add buttons for actions (e.g., "Approve").
- **Dashboards**: Combine charts, lists, and visuals.
- **Security Roles**: Control access via Azure AD/RBAC.

### **6. Advanced Topics**
**Performance Optimization**:
- **Load Data On Demand**: Use `LoadData()` for large datasets.
- **Avoid Delegation Warnings**: Pre-filter data in the source (e.g., SQL views).
- **Lazy Loading**: Load galleries/forms only when visible.

**Offline Mode**:
- Enable in **App Settings** > **Offline**.
- Use `SaveData()`/`LoadData()` to cache data locally.

**Custom Components (PCF)**:
- Build reusable controls with **Power Apps Component Framework** (JavaScript/TypeScript).

**Integration**:
- **Power Automate**: Trigger flows from apps (e.g., send email on button click).
- **Power BI**: Embed reports in apps (`PowerBI.Tile` control).
- **Teams**: Embed apps as tabs or bots.

**Error Handling**:
```powerfx
IfError(
   Patch(Employees, LookUp(Employees, ID=1), {Salary: 50000}),
   Notify("Error updating record!", NotificationType.Error)
)
```

### **7. Testing & Debugging**
**Tools**:
- **Monitor**: Real-time formula evaluation (`View` > `Monitor`).
- **Checkpoints**: Log variables (`Notify(varDebug, NotificationType.Info)`).
- **Test Studio**: Automated testing (premium feature).

**Common Issues**:
| Issue | Fix |
|-------|-----|
| Delegation warning | Use `Collections` or Dataverse. |
| Slow loading | Reduce controls, optimize data calls. |
| Formula errors | Check `Monitor` for syntax. |
| Permission errors | Verify data source access (Azure AD). |

### **8. Deployment & ALM**
**Solutions**:
- Package apps/components for transport between environments (Dev/Test/Prod).
- **Steps**:
  1. Create a **Solution** in [Power Apps Maker Portal](https://make.powerapps.com).
  2. Add components (apps, flows, tables).
  3. Export/import as `.zip`.

**Version Control**:
- Use **Git** with Power Apps CLI (`pac cli`).
- Store solution files in Azure DevOps/GitHub.

**Environments**:
- **Default**: Shared development.
- **Production**: Locked down, user-facing.
- **Sandbox**: Testing before prod.

### **9. Security**
**Data Security**:
- **Dataverse**: Use **Row-Level Security (RLS)** and **Column-Level Security**.
- **SharePoint**: Set list/item permissions.
- **Connections**: Restrict via **Data Policies** (Admin Center).

**App Security**:
- **Share Apps**: Assign users/groups via `Share` button.
- **Embed in Teams/SharePoint**: Control access via parent platform.

### **10. Power Apps CLI (Command Line)**
| Command | Purpose |
|---------|---------|
| `pac auth create` | Create authentication profile. |
| `pac solution init` | Initialize a solution project. |
| `pac solution add-reference` | Add components to solution. |
| `pac solution pack` | Package solution for deployment. |
| `pac solution publish` | Publish to an environment. |

### **11. Quick Tips**
- **Keyboard Shortcuts**:
  - `F5`: Run app.
  - `Ctrl+S`: Save.
  - `Alt+T`: Open **Tree View**.
- **Copy/Paste Controls**: Use `Ctrl+C`/`Ctrl+V` (even across apps).
- **Reuse Components**: Save as **Custom Components** (library).
- **Templates**: Start from templates (e.g., "Employee Onboarding").

### **12. Resources**
- **Docs**: [Microsoft Learn (Power Apps)](https://learn.microsoft.com/en-us/power-apps/)
- **Community**: [Power Users Forum](https://powerusers.microsoft.com/)
- **Tools**:
  - [Power Apps Checker](https://checker.powerapps.com/) (performance analysis).
  - [XRM Toolbox](https://www.xrmtoolbox.com/) (Dataverse management).

**Pro Tip**: Bookmark this cheat sheet and use `Ctrl+F` to jump to topics during development!
