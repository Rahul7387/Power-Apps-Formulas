# ⚡ Power Apps Formulas – Complete Reference

> A comprehensive, example-driven reference for **Power Fx** functions in Microsoft Power Apps Canvas Apps.
> Every function has its own dedicated page with **simple and complex real-world examples**.

---

## 🗂️ Function Index

### 📨 Forms

| Function | Description |
|----------|-------------|
| [SubmitForm](./docs/functions/SubmitForm.md) | Submit a Form control to the data source (create or update) |
| [NewForm](./docs/functions/NewForm.md) | Set a form to New (create) mode |
| [EditForm](./docs/functions/EditForm.md) | Set a form to Edit mode |
| [ViewForm](./docs/functions/ViewForm.md) | Set a form to View (read-only) mode |
| [ResetForm](./docs/functions/ResetForm.md) | Reset form fields to original values, discarding unsaved edits |

---

### 🧭 Navigation

| Function | Description |
|----------|-------------|
| [Navigate](./docs/functions/Navigate.md) | Change the displayed screen with optional transition and context vars |
| [Back](./docs/functions/Back.md) | Return to the previously displayed screen |
| [Launch](./docs/functions/Launch.md) | Open a URL, email, phone link, or another Power App |

---

### 💾 Data Write

| Function | Description |
|----------|-------------|
| [Patch](./docs/functions/Patch.md) | Create or update records in a data source |
| [Remove](./docs/functions/Remove.md) | Delete records from a data source or collection |
| [Refresh](./docs/functions/Refresh.md) | Reload data from the data source, clearing the local cache |

---

### 🔍 Data Read

| Function | Description |
|----------|-------------|
| [Filter](./docs/functions/Filter.md) | Return rows matching one or more conditions |
| [LookUp](./docs/functions/LookUp.md) | Find and return a single matching record |
| [Search](./docs/functions/Search.md) | Return rows where text columns contain a search string |
| [Sort / SortByColumns](./docs/functions/Sort.md) | Sort a table by one or more columns |
| [Distinct](./docs/functions/Distinct.md) | Return a one-column table of unique values |
| [First / Last](./docs/functions/First.md) | Return the first or last record from a table |
| [Gallery.Selected](./docs/functions/GallerySelected.md) | The currently selected record in a Gallery control |

---

### 📦 Variables

| Function | Description |
|----------|-------------|
| [Set](./docs/functions/Set.md) | Create or update a global variable |
| [UpdateContext](./docs/functions/UpdateContext.md) | Create or update screen-local context variables |

---

### 📦 Defaults and User

| Function | Description |
|----------|-------------|
| [Defaults](./docs/functions/Defaults.md) | Get default column values for a data source |
| [User](./docs/functions/User.md) | Properties of the currently logged-in user |

---

### 🗃️ Collections

| Function | Description |
|----------|-------------|
| [Collect](./docs/functions/Collect.md) | Append records to an in-memory collection |
| [ClearCollect](./docs/functions/ClearCollect.md) | Clear and repopulate a collection |

---

### 🔀 Logic

| Function | Description |
|----------|-------------|
| [If](./docs/functions/If.md) | Conditional branching — single or multi-branch |
| [Switch](./docs/functions/Switch.md) | Multi-value exact-match branching |
| [IsBlank](./docs/functions/IsBlank.md) | Check if a value is blank, null, or empty string |
| [IsEmpty](./docs/functions/IsEmpty.md) | Check if a table or collection has zero rows |
| [Coalesce](./docs/functions/Coalesce.md) | Return the first non-blank value from a list |
| [With](./docs/functions/With.md) | Evaluate a formula in a named record scope |

---

### 🔄 Data Transform

| Function | Description |
|----------|-------------|
| [AddColumns](./docs/functions/AddColumns.md) | Add calculated columns to a table (non-destructive) |
| [GroupBy](./docs/functions/GroupBy.md) | Group rows by one or more columns |
| [ForAll](./docs/functions/ForAll.md) | Iterate over rows and run a formula on each |
| [ShowColumns / DropColumns](./docs/functions/ShowColumns.md) | Select or remove specific columns from a table |

---

### 📝 Text

| Function | Description |
|----------|-------------|
| [Concatenate / &](./docs/functions/Concatenate.md) | Join text strings together |
| [Text](./docs/functions/Text.md) | Format a value as a string with a format pattern |
| [Split](./docs/functions/Split.md) | Split a string into a table of substrings |
| [String Functions](./docs/functions/StringFunctions.md) | Trim, Upper, Lower, Proper, Len, Left, Right, Mid, Find, Substitute |
| [Value](./docs/functions/Value.md) | Convert a text string to a number |
| [IsMatch](./docs/functions/IsMatch.md) | Validate a string against a pattern or regex |
| [EncodeUrl / JSON](./docs/functions/EncodeUrl.md) | Encode strings for URLs; convert records to JSON |

---

### 📅 Date & Time

| Function | Description |
|----------|-------------|
| [Date & Time Functions](./docs/functions/DateFunctions.md) | Today, Now, DateAdd, DateDiff, DateValue, Year, Month, Day, Weekday |

---

### 📊 Aggregation

| Function | Description |
|----------|-------------|
| [CountRows / CountIf](./docs/functions/CountRows.md) | Count rows in a table or matching a condition |
| [Sum / Average / Min / Max](./docs/functions/Aggregation.md) | Aggregate numeric column values |
| [Round / RoundUp / RoundDown](./docs/functions/Round.md) | Round numbers to a specified decimal precision |

---

### 🛠️ Debugging & Error Handling

| Function | Description |
|----------|-------------|
| [Trace](./docs/functions/Trace.md) | Log messages to Power Apps Monitor for debugging |
| [IsError / IfError](./docs/functions/IsError.md) | Detect and handle errors from functions like Patch |

---

## 🧭 Quick Reference Cheat Sheet

```
NAVIGATION           FORMS               DATA WRITE         DATA READ
──────────────       ──────────────      ──────────────     ──────────────
Navigate()           SubmitForm()        Patch()            Filter()
Back()               NewForm()           Remove()           LookUp()
Launch()             EditForm()          Refresh()          Search()
                     ViewForm()                             Sort()
                     ResetForm()                            Distinct()
                                                           First() / Last()

VARIABLES            LOGIC               COLLECTIONS        TEXT
──────────────       ──────────────      ──────────────     ──────────────
Set()                If()                Collect()          Text()
UpdateContext()      Switch()            ClearCollect()     Concatenate() / &
Defaults()           IsBlank()                              Split()
User()               IsEmpty()                              Trim/Upper/Lower
                     Coalesce()                             IsMatch()
                     With()                                 Value()

AGGREGATION          DATA TRANSFORM      DEBUG / ERRORS     DATE & TIME
──────────────       ──────────────      ──────────────     ──────────────
Sum()                AddColumns()        Trace()            Today() / Now()
Average()            GroupBy()           IsError()          DateAdd()
Min() / Max()        ForAll()            IfError()          DateDiff()
CountRows()          ShowColumns()                          Text() (dates)
CountIf()            DropColumns()
Round()
```

---

## 📚 How to Use This Reference

Each function page includes:
- **Overview** — what the function does and when to use it
- **Syntax** — the exact formula signature with all parameters
- **Parameters table** — every argument explained
- **Simple examples** — 3 quick, easy-to-understand examples
- **Complex examples** — 3–6 real-world patterns used in production apps
- **Best practices** — proven tips to avoid common mistakes
- **Related functions** — direct links to connected functions
- **Official Microsoft documentation** link

---

## 🔗 Official Resources

- [Power Fx Formula Reference (complete list)](https://learn.microsoft.com/en-us/power-platform/power-fx/formula-reference)
- [Delegation overview](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/delegation-overview)
- [Power Apps Canvas App controls](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/reference-properties)
- [Power Apps community](https://powerusers.microsoft.com/t5/Power-Apps-Community/ct-p/PowerApps1)

---

*Last updated: May 2026 | Microsoft Power Apps Canvas Apps | Power Fx*
