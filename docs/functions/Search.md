# 🔍 Search

> **Category:** Data Read | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`Search` returns rows where **any of the specified text columns contains** the search string (case-insensitive substring match). Unlike `Filter` with `StartsWith`, `Search` matches anywhere in the string — but is **not delegable** to most server data sources.

---

## Syntax

```plaintext
Search( Table, SearchString, "Column1" [, "Column2", ...] )
```

---

## Simple Examples

### 1. Search a single column
```plaintext
Search(Products, txtSearch.Text, "Name")
```

### 2. Search multiple columns
```plaintext
Search(Customers, txtSearch.Text, "Name", "Email", "Phone")
```

### 3. Combine with Filter
```plaintext
Filter(
    Search(Tasks, txtSearch.Text, "Title", "Description"),
    Status = "Open"
)
```

### 4. Empty search shows all
```plaintext
// This works because Search returns everything when SearchString is ""
Search(Products, txtSearch.Text, "Name")
```

### 5. Search a collection (no delegation issue)
```plaintext
Search(varProducts, txtSearch.Text, "Name", "SKU", "Category")
```

---

## Complex Examples

### 6. Live search gallery with category filter
```plaintext
// Gallery Items
Filter(
    Search(varProducts, txtSearch.Text, "Name", "Description", "SKU"),
    ddlCategory.Selected.Value = "All" || Category = ddlCategory.Selected.Value,
    tglActiveOnly.Value = false || IsActive = true
)
```

### 7. Delegable workaround — load to collection first
```plaintext
// App OnStart or Screen OnVisible
ClearCollect(varCustomers, Customers);  // load all to collection

// Gallery Items — non-delegable Search is fine on a collection
Search(varCustomers, txtSearch.Text, "Name", "Email", "CompanyName")
```

### 8. Search with result count display
```plaintext
// Gallery Items
Set(varSearchResults,
    Search(varEmployees, txtSearch.Text, "FullName", "Department", "JobTitle")
);
varSearchResults

// Result count label
CountRows(varSearchResults) & " employee(s) found"
```

### 9. Search with no-results handling
```plaintext
// "No results" label Visible
IsEmpty(Search(varProducts, txtSearch.Text, "Name", "Description"))

// "No results" label Text
"No products match "" & txtSearch.Text & """
```

### 10. Multi-source search (search across two tables)
```plaintext
// Combine results from two collections into one for display
ClearCollect(
    varSearchResults,
    AddColumns(Search(varTasks,    txtSearch.Text, "Title"), "Source", "Task"),
    AddColumns(Search(varProjects, txtSearch.Text, "Title", "Description"), "Source", "Project")
)
```

---

## Search vs Filter (StartsWith)

| | `Search` | `Filter + StartsWith` |
|-|----------|-----------------------|
| Match position | Anywhere in string | Starts of string only |
| Delegable | ❌ No | ✅ Yes |
| Good for | Small/local collections | Large server data sources |
| Case-sensitive | No | No |

---

## Best Practices

1. **Use on collections, not large server lists** — load with `ClearCollect` first.
2. **Use `StartsWith` in `Filter`** when querying SharePoint/Dataverse with 500+ rows.
3. **Guard empty search** — `Search` returns all rows when the search string is `""`.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Filter`](./Filter.md) | Delegable alternative with `StartsWith` |
| [`ClearCollect`](./ClearCollect.md) | Load data locally before `Search` |
| [`Sort`](./Sort.md) | Sort search results |

---

## 🔗 Official Documentation
[Filter, Search, LookUp – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-filter-lookup)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*