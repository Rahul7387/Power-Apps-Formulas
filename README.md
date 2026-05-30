# ⚡ Power Apps Functions – Complete Reference Guide

> A comprehensive reference for the most important **Power Fx** functions in Microsoft Power Apps Canvas Apps — including syntax, parameters, examples, and best practices. Ready for GitHub documentation.

---

## 📋 Table of Contents

| # | Function | Category | Description |
|---|----------|----------|-------------|
| 1 | [`SubmitForm`](#1-submitform) | Forms | Submit a form control to a data source |
| 2 | [`Notify`](#2-notify) | UI Feedback | Show a toast/banner notification |
| 3 | [`Defaults`](#3-defaults) | Forms / Data | Get default values for a data source |
| 4 | [`Gallery.Selected`](#4-galleryselected) | Gallery | Get the currently selected gallery item |
| 5 | [`Navigate`](#5-navigate) | Navigation | Change the displayed screen |
| 6 | [`Set`](#6-set) | Variables | Create/update a global variable |
| 7 | [`UpdateContext`](#7-updatecontext) | Variables | Create/update a context (screen) variable |
| 8 | [`Patch`](#8-patch) | Data | Create or update records in a data source |
| 9 | [`Filter`](#9-filter) | Data | Filter a table based on a condition |
| 10 | [`LookUp`](#10-lookup) | Data | Find a single record matching a condition |
| 11 | [`If`](#11-if) | Logic | Conditional branching |
| 12 | [`Switch`](#12-switch) | Logic | Multi-branch conditional |
| 13 | [`Collect`](#13-collect) | Collections | Add records to a collection |
| 14 | [`ClearCollect`](#14-clearcollect) | Collections | Clear and repopulate a collection |
| 15 | [`Remove`](#15-remove) | Data | Remove records from a data source or collection |
| 16 | [`IsBlank`](#16-isblank) | Validation | Check if a value is blank/empty |
| 17 | [`IsEmpty`](#17-isempty) | Validation | Check if a table/collection is empty |
| 18 | [`Concatenate` / `&`](#18-concatenate--) | Text | Join text strings together |
| 19 | [`Text`](#19-text) | Text | Format a value as a string |
| 20 | [`DateValue` / `Now` / `Today`](#20-datevalue--now--today) | Date & Time | Date/time functions |
| 21 | [`Sort` / `SortByColumns`](#21-sort--sortbycolumns) | Data | Sort tables |
| 22 | [`Search`](#22-search) | Data | Search text columns in a table |
| 23 | [`CountRows` / `Count`](#23-countrows--count) | Aggregation | Count records |
| 24 | [`Sum` / `Average` / `Min` / `Max`](#24-sum--average--min--max) | Aggregation | Aggregate numeric values |
| 25 | [`ResetForm`](#25-resetform) | Forms | Reset a form to its default state |
| 26 | [`NewForm`](#26-newform) | Forms | Set a form to New (create) mode |
| 27 | [`EditForm`](#27-editform) | Forms | Set a form to Edit mode |
| 28 | [`ViewForm`](#28-viewform) | Forms | Set a form to View (read-only) mode |
| 29 | [`Refresh`](#29-refresh) | Data | Reload data from a data source |
| 30 | [`User`](#30-user) | Context | Get the current logged-in user |
| 31 | [`Back`](#31-back) | Navigation | Return to the previous screen |
| 32 | [`Launch`](#32-launch) | Navigation | Open a URL or app |
| 33 | [`Distinct`](#33-distinct) | Data | Get unique values from a column |
| 34 | [`AddColumns`](#34-addcolumns) | Data Transform | Add a calculated column to a table |
| 35 | [`GroupBy`](#35-groupby) | Data Transform | Group table rows |
| 36 | [`ForAll`](#36-forall) | Iteration | Run a formula over each row |
| 37 | [`Split`](#37-split) | Text | Split a string into a table |
| 38 | [`Trim` / `Upper` / `Lower`](#38-trim--upper--lower) | Text | Common string operations |
| 39 | [`Value`](#39-value) | Type Conversion | Convert text to a number |
| 40 | [`Round` / `RoundUp` / `RoundDown`](#40-round--roundup--rounddown) | Math | Rounding functions |

---

## 1. SubmitForm

### Overview
`SubmitForm` submits the data in a **Form control** to the connected data source (SharePoint, Dataverse, SQL, etc.). It automatically handles both **create** (New mode) and **update** (Edit mode).

### Syntax
```plaintext
SubmitForm( FormName )
```

### Parameters
| Parameter | Description |
|-----------|-------------|
| `FormName` | The name of the Form control to submit |

### Form Modes
| Mode | Action on Submit |
|------|-----------------|
| `FormMode.New` | Creates a new record |
| `FormMode.Edit` | Updates the existing record |
| `FormMode.View` | Read-only — Submit has no effect |

### Examples

```plaintext
// Basic submit on button click
SubmitForm(Form1)
```

```plaintext
// Submit with validation check first
If(
    IsBlank(DataCardValue_Name.Text),
    Notify("Name is required", NotificationType.Error),
    SubmitForm(EditForm1)
)
```

```plaintext
// Form OnSuccess — navigate after successful submit
Navigate(
    ConfirmationScreen,
    ScreenTransition.Cover,
    { SubmittedRecord: EditForm1.LastSubmit }
)
```

```plaintext
// Form OnFailure — show error message
Notify("Save failed: " & EditForm1.Error, NotificationType.Error)
```

### Key Properties

| Property | Description |
|----------|-------------|
| `Form.LastSubmit` | The record that was last submitted (use in `OnSuccess`) |
| `Form.Error` | Error message if submission failed (use in `OnFailure`) |
| `Form.Valid` | `true` if all required fields pass validation |

### Best Practices
- Always handle `OnSuccess` (navigate or notify) and `OnFailure` (show error).
- Check `Form.Valid` before calling `SubmitForm` to avoid unnecessary server calls.
- Use `EditForm1.LastSubmit` to get the newly created record's ID after a `New` submission.

---

## 2. Notify

### Overview
`Notify` shows a **toast notification** (banner message) at the top of the screen. Useful for success confirmations, warnings, and error messages.

### Syntax
```plaintext
Notify( Message [, NotificationType [, Timeout ]] )
```

### Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `Message` | Text | The message to display |
| `NotificationType` | Enum | The style/color of the notification (optional) |
| `Timeout` | Number | Milliseconds to show the notification (optional, default: 2000) |

### Notification Types

| Type | Color | Use Case |
|------|-------|----------|
| `NotificationType.Success` | 🟢 Green | Operation completed successfully |
| `NotificationType.Error` | 🔴 Red | Something went wrong |
| `NotificationType.Warning` | 🟡 Yellow | User should be cautious |
| `NotificationType.Information` | 🔵 Blue | General information (default) |

### Examples

```plaintext
// Success notification
Notify("Record saved successfully!", NotificationType.Success)
```

```plaintext
// Error with long timeout (5 seconds)
Notify("Failed to save. Please try again.", NotificationType.Error, 5000)
```

```plaintext
// Warning with dynamic message
Notify("You are editing " & Gallery1.Selected.Title, NotificationType.Warning)
```

```plaintext
// Information (default style)
Notify("Loading data, please wait...")
```

```plaintext
// Validation before form submit
If(
    IsBlank(txtEmail.Text),
    Notify("Email is required.", NotificationType.Error),
    If(
        !IsMatch(txtEmail.Text, Email),
        Notify("Please enter a valid email address.", NotificationType.Warning),
        SubmitForm(Form1)
    )
)
```

### Best Practices
- Always pair `SubmitForm` with notifications in `OnSuccess` / `OnFailure`.
- Keep messages short and actionable.
- Use `5000`–`8000` ms for errors so users have time to read them.

---

## 3. Defaults

### Overview
`Defaults` returns a **record with default values** for all columns in a data source. It is most commonly used to pre-populate a new form or set initial values.

### Syntax
```plaintext
Defaults( DataSource )
```

### Parameters
| Parameter | Description |
|-----------|-------------|
| `DataSource` | The table/list/entity to get defaults from |

### Examples

```plaintext
// Set a form's Item to defaults for a new record
EditForm1.Item = Defaults(SharePointList)
```

```plaintext
// Navigate to an Edit screen and pre-set defaults
Navigate(
    NewItemScreen,
    ScreenTransition.Cover,
    { FormItem: Defaults(Products) }
)
// On EditScreen, set Form1.Item = FormItem
```

```plaintext
// Merge defaults with a specific value
Patch(
    Customers,
    Defaults(Customers),
    {
        Name: txtName.Text,
        Status: "Active",
        CreatedDate: Now()
    }
)
```

```plaintext
// Use Defaults with Patch to create a new record
Patch(
    Tasks,
    Defaults(Tasks),
    {
        Title: "New Task",
        AssignedTo: User().Email,
        DueDate: Today() + 7
    }
)
```

### Best Practices
- Use `Defaults(DataSource)` as the base record in `Patch` when creating new records.
- Combine with `NewForm(Form1)` to put the form in New mode with defaults applied.
- `Defaults` respects column-level default values defined in the data source (e.g., SharePoint column defaults).

---

## 4. Gallery.Selected

### Overview
`Gallery.Selected` is a **property** (not a function) that returns the **currently selected record** in a Gallery control. It's one of the most used patterns in Power Apps for Master-Detail navigation.

### Syntax
```plaintext
GalleryName.Selected
GalleryName.Selected.ColumnName
```

### Common Usage

```plaintext
// Access the full selected record
Gallery1.Selected

// Access a specific field of the selected record
Gallery1.Selected.Title
Gallery1.Selected.Email
Gallery1.Selected.Price
```

```plaintext
// Navigate to detail screen with selected item
// OnSelect of Gallery1:
Navigate(
    DetailScreen,
    ScreenTransition.Cover,
    { SelectedProduct: Gallery1.Selected }
)
```

```plaintext
// Bind a form to the selected gallery item
// Form1.Item property:
Gallery1.Selected
```

```plaintext
// Show selected item details in labels (outside the gallery)
// Label.Text:
Gallery1.Selected.ProductName

// Another label:
"Price: " & Text(Gallery1.Selected.UnitPrice, "$#,##0.00")
```

```plaintext
// Edit button that opens the selected record for editing
Navigate(
    EditScreen,
    ScreenTransition.Cover,
    {
        RecordToEdit: Gallery1.Selected,
        FormMode: FormMode.Edit
    }
)
```

```plaintext
// Delete the selected record
Remove(Products, Gallery1.Selected);
Notify("Product deleted", NotificationType.Success)
```

### Related Properties

| Property | Description |
|----------|-------------|
| `Gallery.Selected` | The selected record (full row) |
| `Gallery.SelectedText` | The text of the selected item's text property |
| `Gallery.AllItems` | All records displayed in the gallery |
| `Gallery.AllItemsCount` | Total number of items shown |

### Best Practices
- Always check `IsBlank(Gallery1.Selected)` before using the value in actions.
- Set the form's `Item` property directly to `Gallery1.Selected` for the simplest master-detail pattern.
- In nested galleries, use `ThisItem` for the current row and `Parent.Selected` for the outer gallery's selection.

---

## 5. Navigate

> See the separate detailed guide: [PowerApps_Navigate_Function.md](./PowerApps_Navigate_Function.md)

### Quick Reference
```plaintext
Navigate( Screen [, Transition [, UpdateContextRecord ]] )

Navigate(HomeScreen)
Navigate(DetailScreen, ScreenTransition.Cover, { SelectedItem: Gallery1.Selected })
Navigate(LoginScreen, ScreenTransition.Fade)
```

---

## 6. Set

### Overview
`Set` creates or updates a **global variable** — accessible from any screen in the app.

### Syntax
```plaintext
Set( VariableName, Value )
```

### Examples

```plaintext
// Set a simple boolean flag
Set(varIsLoggedIn, true)

// Set a text variable
Set(varUserRole, "Admin")

// Set a record variable
Set(varCurrentUser, LookUp(Employees, Email = User().Email))

// Set a number
Set(varCartItemCount, varCartItemCount + 1)

// Toggle a boolean
Set(varShowPanel, !varShowPanel)
```

```plaintext
// App OnStart — initialize globals
Set(varCurrentUser, User());
Set(varAppVersion, "1.0.3");
Set(varIsAdmin, User().Email in AdminList.Email)
```

### Set vs UpdateContext

| Feature | `Set` | `UpdateContext` |
|---------|-------|-----------------|
| Scope | **Global** — all screens | **Local** — current screen only |
| Syntax | `Set(varName, value)` | `UpdateContext({varName: value})` |
| Best for | Auth state, user info, shared flags | Screen-specific UI state |
| Performance | Slightly slower (global) | Faster (local) |

---

## 7. UpdateContext

### Overview
`UpdateContext` creates or updates **context variables** — scoped to the **current screen only**.

### Syntax
```plaintext
UpdateContext( { VariableName: Value [, VariableName2: Value2, ...] } )
```

### Examples

```plaintext
// Toggle a modal dialog visibility
UpdateContext({ showModal: true })

// Update multiple variables at once
UpdateContext({
    isLoading: true,
    errorMessage: "",
    selectedTab: "Details"
})

// Increment a counter
UpdateContext({ stepCount: stepCount + 1 })
```

```plaintext
// Cancel button — reset and close
UpdateContext({ showModal: false, editRecord: Blank() })
```

```plaintext
// Wizard pattern — move to next step
UpdateContext({ currentStep: currentStep + 1 })
// Progress bar Width formula:
(currentStep / totalSteps) * Parent.Width
```

---

## 8. Patch

### Overview
`Patch` **creates or modifies** one or more records in a data source. It's one of the most powerful data-writing functions.

### Syntax
```plaintext
// Single record
Patch( DataSource, BaseRecord, ChangeRecord [, ...] )

// Multiple records at once
Patch( DataSource, Table_of_Changes )
```

### Parameters
| Parameter | Description |
|-----------|-------------|
| `DataSource` | The data source to write to |
| `BaseRecord` | Existing record to update, or `Defaults(DataSource)` to create new |
| `ChangeRecord` | Fields to create/overwrite |

### Examples

```plaintext
// Create a new record
Patch(
    Customers,
    Defaults(Customers),
    {
        Name: txtName.Text,
        Email: txtEmail.Text,
        Status: "Active"
    }
)
```

```plaintext
// Update an existing record (from gallery selection)
Patch(
    Products,
    Gallery1.Selected,
    { Price: Value(txtNewPrice.Text) }
)
```

```plaintext
// Update a specific record by ID
Patch(
    Tasks,
    LookUp(Tasks, ID = varTaskID),
    { Status: "Completed", CompletedDate: Now() }
)
```

```plaintext
// Bulk update — patch multiple records
Patch(
    Orders,
    Table(
        { ID: 1, Status: "Shipped" },
        { ID: 2, Status: "Shipped" },
        { ID: 3, Status: "Shipped" }
    )
)
```

```plaintext
// Patch and capture the result (for the new ID)
Set(
    varNewRecord,
    Patch(
        Invoices,
        Defaults(Invoices),
        { CustomerName: txtCustomer.Text, Total: numTotal }
    )
);
Notify("Invoice #" & varNewRecord.ID & " created!", NotificationType.Success)
```

### Patch vs SubmitForm

| Feature | `Patch` | `SubmitForm` |
|---------|---------|--------------|
| Use case | Manual/programmatic writes | Form-bound writes |
| Control | Full control over fields | Automatic from form cards |
| Validation | Manual | Automatic (form card rules) |
| Best for | Bulk updates, background writes | User-facing data entry forms |

---

## 9. Filter

### Overview
`Filter` returns a **subset of a table** where each row satisfies the given condition. Used to show only relevant records.

### Syntax
```plaintext
Filter( Table, Condition [, Condition2, ...] )
```

### Examples

```plaintext
// Gallery Items property — show only active products
Filter(Products, Status = "Active")
```

```plaintext
// Filter with a search input
Filter(
    Customers,
    StartsWith(Name, txtSearch.Text)
)
```

```plaintext
// Multiple conditions (AND)
Filter(
    Orders,
    Status = "Pending" && OrderDate >= Today() - 30
)
```

```plaintext
// Filter by current user
Filter(Tasks, AssignedTo = User().Email)
```

```plaintext
// Combined Filter + Search
Filter(
    Products,
    (IsBlank(txtSearch.Text) || StartsWith(Name, txtSearch.Text)) &&
    (ddlCategory.Selected.Value = "All" || Category = ddlCategory.Selected.Value)
)
```

### Delegation Warning
> ⚠️ Some functions like `Search`, `In`, and `Mid` are **not delegable** to certain data sources (SharePoint, Dataverse). Power Apps will show a yellow warning triangle and cap results at 500–2000 rows. Prefer `StartsWith`, `=`, `>`, `<` for delegable queries.

---

## 10. LookUp

### Overview
`LookUp` finds and returns **a single record** from a table that matches a condition. Returns blank if no match.

### Syntax
```plaintext
LookUp( Table, Condition [, ReduceFormula] )
```

### Examples

```plaintext
// Find a customer by email
LookUp(Customers, Email = txtSearchEmail.Text)
```

```plaintext
// Get a specific field from the found record
LookUp(Products, ID = varProductID).ProductName
```

```plaintext
// Use with Patch to update a specific record
Patch(
    Employees,
    LookUp(Employees, EmployeeID = varID),
    { Department: "Engineering" }
)
```

```plaintext
// Check if a record exists
If(
    IsBlank(LookUp(Users, Email = txtEmail.Text)),
    Notify("User not found", NotificationType.Warning),
    Navigate(ProfileScreen)
)
```

```plaintext
// Get a calculated field (3rd parameter)
LookUp(Products, Category = "Electronics", Max(Price))
```

### Filter vs LookUp

| Feature | `Filter` | `LookUp` |
|---------|----------|----------|
| Returns | Table (multiple rows) | Single record |
| Use in Gallery | Yes | No |
| Use to get one value | No (use `First(Filter(...))`) | Yes |

---

## 11. If

### Overview
`If` evaluates conditions and returns different values or runs different actions depending on the result. Supports multi-branch (else-if) logic.

### Syntax
```plaintext
// Two-branch
If( Condition, ThenResult, ElseResult )

// Multi-branch (else-if)
If( Condition1, Result1, Condition2, Result2, ..., DefaultResult )
```

### Examples

```plaintext
// Simple toggle
If(Toggle1.Value, "Yes", "No")
```

```plaintext
// Button color based on status
If(
    ThisItem.Status = "Active", Color.Green,
    ThisItem.Status = "Pending", Color.Orange,
    Color.Red
)
```

```plaintext
// Multi-step validation
If(
    IsBlank(txtName.Text),
    Notify("Name is required", NotificationType.Error),
    IsBlank(txtEmail.Text),
    Notify("Email is required", NotificationType.Error),
    !IsMatch(txtEmail.Text, Email),
    Notify("Invalid email format", NotificationType.Warning),
    SubmitForm(Form1)
)
```

```plaintext
// Visible property — show button only for admins
User().Email in AdminEmails.Email
// Or:
If(varUserRole = "Admin", true, false)
```

---

## 12. Switch

### Overview
`Switch` matches a value against multiple cases, returning the result of the first match. Cleaner than nested `If` for multi-value checks.

### Syntax
```plaintext
Switch( Formula, Match1, Result1 [, Match2, Result2, ...] [, DefaultResult] )
```

### Examples

```plaintext
// Status badge color
Switch(
    ThisItem.Status,
    "Active",   Color.Green,
    "Pending",  Color.Orange,
    "Closed",   Color.Gray,
    "Rejected", Color.Red,
    Color.Black   // default
)
```

```plaintext
// Navigate based on user role
Switch(
    varUserRole,
    "Admin",   Navigate(AdminDashboard),
    "Manager", Navigate(ManagerScreen),
    "User",    Navigate(HomeScreen)
)
```

---

## 13. Collect

### Overview
`Collect` **adds records** to a collection (an in-memory table). If the collection doesn't exist, it creates it.

### Syntax
```plaintext
Collect( CollectionName, Record_or_Table )
```

### Examples

```plaintext
// Add a single record to a cart
Collect(
    varCart,
    { ProductID: Gallery1.Selected.ID, Name: Gallery1.Selected.Name, Qty: 1 }
)
```

```plaintext
// Load data into a collection on app start
Collect(varProducts, Filter(Products, IsActive = true))
```

```plaintext
// Build a collection from user inputs
Collect(varSelectedItems, Gallery1.Selected)
```

### Collect vs ClearCollect

| Function | Behavior |
|----------|----------|
| `Collect` | **Appends** to existing collection |
| `ClearCollect` | **Clears first**, then populates |

---

## 14. ClearCollect

### Overview
`ClearCollect` **clears a collection** and then adds records to it — equivalent to `Clear(col); Collect(col, ...)`.

### Syntax
```plaintext
ClearCollect( CollectionName, Record_or_Table )
```

### Examples

```plaintext
// Refresh a local collection with fresh server data
ClearCollect(varOrders, Filter(Orders, UserID = varCurrentUser.ID))
```

```plaintext
// App OnStart — load reference data
ClearCollect(varCategories, Categories);
ClearCollect(varStatuses, ["Active", "Inactive", "Pending"])
```

```plaintext
// Reset selected items list
ClearCollect(varSelectedIDs, [])
```

---

## 15. Remove

### Overview
`Remove` **deletes one or more records** from a data source or collection.

### Syntax
```plaintext
Remove( DataSource, Record [, ALL] )
Remove( DataSource, Table )
```

### Examples

```plaintext
// Remove the selected gallery item
Remove(Products, Gallery1.Selected);
Notify("Product deleted.", NotificationType.Success)
```

```plaintext
// Remove with confirmation dialog pattern
If(
    varConfirmDelete,
    Remove(Tasks, varTaskToDelete);
    UpdateContext({ varConfirmDelete: false });
    Notify("Task deleted.", NotificationType.Success)
)
```

```plaintext
// Remove from a local collection
Remove(varCart, varSelectedCartItem)
```

```plaintext
// Remove all records matching a condition
Remove(
    OldLogs,
    Filter(OldLogs, LogDate < Today() - 90),
    RemoveFlags.All
)
```

---

## 16. IsBlank

### Overview
`IsBlank` returns `true` if a value is **blank, null, or an empty string**.

### Syntax
```plaintext
IsBlank( Value )
```

### Examples

```plaintext
// Required field check
If(IsBlank(txtName.Text), Notify("Name is required", NotificationType.Error))
```

```plaintext
// Show placeholder text
If(IsBlank(Gallery1.Selected), "No item selected", Gallery1.Selected.Title)
```

```plaintext
// Validate before Patch
If(
    IsBlank(txtTitle.Text) || IsBlank(ddlCategory.Selected.Value),
    Notify("Please fill all required fields", NotificationType.Error),
    Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text })
)
```

### IsBlank vs IsEmpty

| Function | Checks | Returns true for |
|----------|--------|-----------------|
| `IsBlank` | A single value | `""`, `null`, `blank` |
| `IsEmpty` | A table/collection | A table with zero rows |

---

## 17. IsEmpty

### Overview
`IsEmpty` returns `true` if a **table or collection has zero rows**.

### Syntax
```plaintext
IsEmpty( Table )
```

### Examples

```plaintext
// Show a "No results" message
If(IsEmpty(Filter(Products, Status = "Active")), "No active products found", "")
```

```plaintext
// Disable submit if cart is empty
Button.DisplayMode = If(IsEmpty(varCart), DisplayMode.Disabled, DisplayMode.Edit)
```

```plaintext
// Guard before using First()
If(
    !IsEmpty(varResults),
    Set(varTopResult, First(varResults))
)
```

---

## 18. Concatenate / `&`

### Overview
Joins two or more text strings. The `&` operator is the shorthand for `Concatenate`.

### Syntax
```plaintext
Concatenate( String1, String2 [, ...] )
String1 & String2
```

### Examples

```plaintext
// Build a full name
txtFirstName.Text & " " & txtLastName.Text

// Build a greeting
"Hello, " & User().FullName & "!"

// Concatenate with Text() conversion
"Total: " & Text(Sum(varCart, Price * Qty), "$#,##0.00")

// Build a multi-line label
txtAddress.Text & Char(10) & txtCity.Text & ", " & txtState.Text
```

> `Char(10)` inserts a newline character. Set the label's `WhiteSpace` to `WhiteSpace.PreWrap`.

---

## 19. Text

### Overview
`Text` **formats a number, date, or other value as a string** using a format pattern.

### Syntax
```plaintext
Text( Value [, FormatString] )
```

### Common Format Strings

| Pattern | Example Output |
|---------|---------------|
| `"$#,##0.00"` | `$1,234.56` |
| `"#,##0"` | `1,235` |
| `"0.0%"` | `12.5%` |
| `"dd/mm/yyyy"` | `30/05/2026` |
| `"mmmm d, yyyy"` | `May 30, 2026` |
| `"hh:mm AM/PM"` | `02:30 PM` |
| `"[$-en-US]mmm yyyy"` | `May 2026` |

### Examples

```plaintext
Text(12345.678, "$#,##0.00")     // → "$12,345.68"
Text(0.1234, "0.0%")             // → "12.3%"
Text(Today(), "dd mmm yyyy")     // → "30 May 2026"
Text(Now(), "hh:mm:ss AM/PM")    // → "02:30:00 PM"
Text(Gallery1.Selected.Price, "$#,##0") // → "$500"
```

---

## 20. DateValue / Now / Today

### Overview
Core date/time functions for working with dates.

| Function | Returns | Example |
|----------|---------|---------|
| `Today()` | Current date (no time) | `Today()` → `5/30/2026` |
| `Now()` | Current date + time | `Now()` → `5/30/2026 2:30 PM` |
| `DateValue("5/30/2026")` | Converts a string to a date | — |
| `DateAdd(date, n, unit)` | Adds time to a date | `DateAdd(Today(), 7, Days)` |
| `DateDiff(date1, date2, unit)` | Difference between dates | `DateDiff(StartDate, Today(), Days)` |
| `Year(date)` | Extracts the year | `Year(Today())` → `2026` |
| `Month(date)` | Extracts the month | `Month(Today())` → `5` |
| `Day(date)` | Extracts the day | `Day(Today())` → `30` |

### Examples

```plaintext
// Due date = today + 30 days
DateAdd(Today(), 30, Days)

// Days overdue
DateDiff(DueDate, Today(), Days)

// Show "Overdue" label
If(ThisItem.DueDate < Today(), "Overdue", "On Time")

// Default value for a date picker
Today()
```

---

## 21. Sort / SortByColumns

### Overview
`Sort` and `SortByColumns` return a table sorted by one or more columns.

### Syntax
```plaintext
Sort( Table, Column [, SortOrder.Ascending | SortOrder.Descending] )

SortByColumns( Table, ColumnName1 [, SortOrder1] [, ColumnName2, SortOrder2, ...] )
```

### Examples

```plaintext
// Gallery Items — sort by name A→Z
Sort(Products, Name, SortOrder.Ascending)

// Sort by date, newest first
Sort(Orders, OrderDate, SortOrder.Descending)

// Sort by multiple columns
SortByColumns(Employees, "Department", SortOrder.Ascending, "LastName", SortOrder.Ascending)

// Dynamic sort (toggle A/Z)
Sort(Products, Title, If(varSortAscending, SortOrder.Ascending, SortOrder.Descending))
```

---

## 22. Search

### Overview
`Search` returns rows from a table where **any of the specified columns** contain the search string.

### Syntax
```plaintext
Search( Table, SearchString, Column1 [, Column2, ...] )
```

### Examples

```plaintext
// Search products by name or description
Search(Products, txtSearch.Text, "Name", "Description")

// Combined with Filter
Filter(
    Search(Customers, txtSearch.Text, "Name", "Email"),
    Status = "Active"
)
```

> ⚠️ `Search` is **not delegable** to most data sources for large datasets. Combine with `Filter` on delegable columns first, then `Search` the reduced set.

---

## 23. CountRows / Count

### Overview
Count records in a table or non-blank numeric values in a column.

### Syntax
```plaintext
CountRows( Table )           // Count all rows in a table
Count( Column )              // Count non-blank numeric values
CountA( Column )             // Count non-blank values (any type)
CountIf( Table, Condition )  // Count rows matching a condition
```

### Examples

```plaintext
// Show total product count
"Total: " & CountRows(Products)

// Count active tasks
CountIf(Tasks, Status = "Active")

// Show count in a badge
Text(CountRows(varCart), "0") & " items"
```

---

## 24. Sum / Average / Min / Max

### Overview
Aggregate numeric columns across a table.

### Syntax
```plaintext
Sum( Table, NumericColumn )
Average( Table, NumericColumn )
Min( Table, Column )
Max( Table, Column )
```

### Examples

```plaintext
// Cart total
Sum(varCart, Price * Quantity)

// Average order value
Average(Orders, TotalAmount)

// Highest priced product
Max(Products, Price)

// Oldest record date
Min(AuditLog, CreatedDate)

// Sum with filter
Sum(Filter(Sales, Region = "West"), Revenue)
```

---

## 25. ResetForm

### Overview
`ResetForm` resets all fields in a Form control to their **original values** (the values from `Form.Item`), discarding any unsaved user edits.

### Syntax
```plaintext
ResetForm( FormName )
```

### Examples

```plaintext
// Cancel button — discard changes
ResetForm(Form1);
Navigate(ListScreen, ScreenTransition.UnCover)
```

```plaintext
// After successful submit, reset for next entry
// Form OnSuccess:
ResetForm(Form1);
NewForm(Form1);
Notify("Saved! Ready for next entry.", NotificationType.Success)
```

---

## 26. NewForm

### Overview
`NewForm` sets a Form control to **New mode**, clearing all fields and preparing for a new record entry.

### Syntax
```plaintext
NewForm( FormName )
```

### Examples

```plaintext
// Add new button
NewForm(EditForm1);
Navigate(EditScreen, ScreenTransition.Cover)
```

```plaintext
// After submit — go back to New mode for another entry
NewForm(Form1);
Notify("Record created! Add another.", NotificationType.Success)
```

---

## 27. EditForm

### Overview
`EditForm` sets a Form control to **Edit mode** so the user can modify the bound record.

### Syntax
```plaintext
EditForm( FormName )
```

### Examples

```plaintext
// Edit button in a View screen
EditForm(Form1)
```

```plaintext
// Navigate to edit with selected item
EditForm(Form1);
Navigate(EditScreen, ScreenTransition.Cover)
// Form1.Item = Gallery1.Selected (set in the Item property)
```

---

## 28. ViewForm

### Overview
`ViewForm` sets a Form control to **View (read-only) mode**.

### Syntax
```plaintext
ViewForm( FormName )
```

### Examples

```plaintext
// After save — switch back to View mode
// Form OnSuccess:
ViewForm(Form1);
Notify("Saved successfully!", NotificationType.Success)
```

---

## 29. Refresh

### Overview
`Refresh` forces a reload of data from the connected data source, clearing the local cache.

### Syntax
```plaintext
Refresh( DataSource )
```

### Examples

```plaintext
// Pull-to-refresh button
Refresh(SharePointList);
ClearCollect(varData, SharePointList)
```

```plaintext
// After Patch, refresh to see updated data
Patch(Inventory, Gallery1.Selected, { Qty: Qty - 1 });
Refresh(Inventory)
```

---

## 30. User

### Overview
`User` returns information about the **currently logged-in user**.

### Syntax
```plaintext
User()
```

### Properties

| Property | Description | Example |
|----------|-------------|---------|
| `User().FullName` | Display name | `"Rahul Sharma"` |
| `User().Email` | Email address | `"rahul@company.com"` |
| `User().Image` | Profile photo URL | Use in Image control |

### Examples

```plaintext
// Show user name in a label
"Welcome, " & User().FullName

// Filter data by current user
Filter(MyTasks, AssignedTo = User().Email)

// Stamp who created a record
Patch(
    Tickets,
    Defaults(Tickets),
    { Title: txtTitle.Text, CreatedBy: User().Email, CreatedOn: Now() }
)

// Check admin status
Set(varIsAdmin, User().Email in AdminList.Email)
```

---

## 31. Back

### Overview
`Back` returns to the **previously displayed screen**, using the reverse of the transition used by `Navigate`.

### Syntax
```plaintext
Back( [Transition] )
```

### Examples

```plaintext
Back()
Back(ScreenTransition.UnCover)
```

---

## 32. Launch

### Overview
`Launch` opens a **URL, another app, or a file** in the browser or appropriate app.

### Syntax
```plaintext
Launch( URL [, Parameter, Value, ...] )
```

### Examples

```plaintext
// Open a website
Launch("https://www.microsoft.com")

// Open an email
Launch("mailto:" & txtEmail.Text & "?subject=Hello")

// Open a phone dialer
Launch("tel:" & txtPhone.Text)

// Open another Power App
Launch(
    "https://apps.powerapps.com/play/YOUR_APP_ID",
    "RecordID", Gallery1.Selected.ID
)

// Open a SharePoint file
Launch(Gallery1.Selected.FileURL)
```

---

## 33. Distinct

### Overview
`Distinct` returns a **one-column table of unique values** from a column — useful for dropdown lists and de-duplication.

### Syntax
```plaintext
Distinct( Table, Column )
```

### Examples

```plaintext
// Dropdown Items — unique list of categories
Distinct(Products, Category)

// Use with Sort for a sorted dropdown
Sort(Distinct(Products, Category), Value)

// Count unique customers
CountRows(Distinct(Orders, CustomerID))
```

---

## 34. AddColumns

### Overview
`AddColumns` returns a copy of a table with **one or more new calculated columns** added — without modifying the original data source.

### Syntax
```plaintext
AddColumns( Table, ColumnName, Formula [, ColumnName2, Formula2, ...] )
```

### Examples

```plaintext
// Add a TotalPrice column
AddColumns(OrderItems, "TotalPrice", Quantity * UnitPrice)

// Add a full name column
AddColumns(Employees, "FullName", FirstName & " " & LastName)

// Combine Filter + AddColumns
ClearCollect(
    varEnrichedOrders,
    AddColumns(
        Filter(Orders, Status = "Pending"),
        "DaysOverdue",
        DateDiff(OrderDate, Today(), Days)
    )
)
```

---

## 35. GroupBy

### Overview
`GroupBy` groups rows of a table by one or more columns, creating a subtable column.

### Syntax
```plaintext
GroupBy( Table, ColumnName1 [, ColumnName2], GroupColumnName )
```

### Examples

```plaintext
// Group sales by region
GroupBy(Sales, "Region", "SalesData")
// Then: Sum(ThisGroup.SalesData, Revenue)
```

```plaintext
// Gallery showing category groups
GroupBy(Products, "Category", "Items")
```

---

## 36. ForAll

### Overview
`ForAll` iterates over each row of a table and runs a formula — used for bulk operations.

### Syntax
```plaintext
ForAll( Table, Formula )
```

### Examples

```plaintext
// Send approval email to each manager
ForAll(
    varSelectedManagers,
    Patch(ApprovalRequests, Defaults(ApprovalRequests), { ManagerEmail: Email, Status: "Pending" })
)
```

```plaintext
// Bulk status update
ForAll(
    Filter(Tasks, DueDate < Today()),
    Patch(Tasks, ThisRecord, { Status: "Overdue" })
)
```

> ⚠️ `ForAll` is **not asynchronous** — it runs sequentially. For large datasets, prefer bulk `Patch` with a table.

---

## 37. Split

### Overview
`Split` splits a text string into a **single-column table** of substrings, using a separator.

### Syntax
```plaintext
Split( Text, Separator )
```

### Examples

```plaintext
// Split a comma-separated tag list
Split(ThisItem.Tags, ",")

// Count number of tags
CountRows(Split(ThisItem.Tags, ","))

// Check if a tag exists
"PowerApps" in Split(ThisItem.Tags, ",").Value
```

---

## 38. Trim / Upper / Lower

### Overview
Common string manipulation functions.

| Function | Description | Example |
|----------|-------------|---------|
| `Trim(text)` | Remove leading/trailing spaces | `Trim("  Hello  ")` → `"Hello"` |
| `TrimEnds(text)` | Same as Trim | — |
| `Upper(text)` | Convert to uppercase | `Upper("hello")` → `"HELLO"` |
| `Lower(text)` | Convert to lowercase | `Lower("HELLO")` → `"hello"` |
| `Proper(text)` | Title case | `Proper("john doe")` → `"John Doe"` |
| `Len(text)` | Length of string | `Len("Hello")` → `5` |
| `Left(text, n)` | First n characters | `Left("Hello", 3)` → `"Hel"` |
| `Right(text, n)` | Last n characters | `Right("Hello", 3)` → `"llo"` |
| `Mid(text, start, n)` | Substring | `Mid("Hello", 2, 3)` → `"ell"` |
| `Find(needle, haystack)` | Position of substring | `Find("lo", "Hello")` → `4` |
| `Replace(text, start, n, new)` | Replace by position | — |
| `Substitute(text, old, new)` | Replace by value | `Substitute("Hello World", "World", "Power Apps")` |

---

## 39. Value

### Overview
`Value` converts a **text string to a number**.

### Syntax
```plaintext
Value( Text )
```

### Examples

```plaintext
// Convert text input to number for calculation
Value(txtQuantity.Text) * Gallery1.Selected.UnitPrice

// Patch with numeric value
Patch(Products, Gallery1.Selected, { Stock: Value(txtNewStock.Text) })

// Guard against blank
If(IsBlank(txtQty.Text), 0, Value(txtQty.Text))
```

---

## 40. Round / RoundUp / RoundDown

### Overview
Rounding functions for numeric calculations.

### Syntax
```plaintext
Round( Number, DecimalPlaces )
RoundUp( Number, DecimalPlaces )
RoundDown( Number, DecimalPlaces )
```

### Examples

```plaintext
Round(3.567, 2)      // → 3.57
RoundUp(3.001, 2)    // → 3.01
RoundDown(3.999, 2)  // → 3.99

// Round cart total to 2 decimals
Round(Sum(varCart, Price * Qty), 2)

// Round up to next dollar
RoundUp(totalAmount, 0)
```

---

## 🗂️ Quick Reference Cheat Sheet

```
NAVIGATION          FORMS               DATA WRITE         DATA READ
──────────────      ──────────────      ──────────────     ──────────────
Navigate()          SubmitForm()        Patch()            Filter()
Back()              ResetForm()         Collect()          LookUp()
Launch()            NewForm()           ClearCollect()     Search()
                    EditForm()          Remove()           Sort()
                    ViewForm()          Refresh()          SortByColumns()
                                                          Distinct()
VARIABLES           VALIDATION          AGGREGATION        TEXT
──────────────      ──────────────      ──────────────     ──────────────
Set()               IsBlank()           Sum()              Text()
UpdateContext()     IsEmpty()           Average()          Concatenate() / &
Defaults()          If()                Min() / Max()      Trim/Upper/Lower
                    Switch()            CountRows()        Split()
                                        CountIf()          Substitute()

USER / DATE         ITERATION           TRANSFORM
──────────────      ──────────────      ──────────────
User()              ForAll()            AddColumns()
Today()             With()              GroupBy()
Now()                                   ShowColumns()
DateAdd()                               DropColumns()
DateDiff()
```

---

## 🔗 Official References

- [Power Fx formula reference](https://learn.microsoft.com/en-us/power-platform/power-fx/formula-reference)
- [Navigate and Back](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-navigate)
- [Patch function](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-patch)
- [Filter, Search, and LookUp](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-filter-lookup)
- [Delegation overview](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/delegation-overview)
- [Form functions](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)
- [Collect and ClearCollect](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-clear-collect-clearcollect)

---

*Last updated: May 2026 | Power Apps Canvas App | Power Fx*
