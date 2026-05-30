# 🖼️ Gallery.Selected

> **Category:** Gallery | **Works in:** Canvas Apps

---

## Overview

A property of a Gallery control that returns the currently selected record (the full row). The foundation of master-detail patterns in Power Apps.

---

## Syntax

```plaintext
GalleryName.Selected
GalleryName.Selected.ColumnName
```

---

## Simple Examples

### 1. Access selected record fields
```plaintext
Gallery1.Selected.Title
Gallery1.Selected.Email
Gallery1.Selected.Price
```

### 2. Bind form to gallery selection
```plaintext
// EditForm1.Item property:
Gallery1.Selected
```

### 3. Navigate with selected item
```plaintext
// Gallery1 OnSelect:
Navigate(DetailScreen, ScreenTransition.Cover, { CurrentItem: Gallery1.Selected })
```

---

## Complex Examples

### 4. Full master-detail edit workflow
```plaintext
// Edit button outside gallery
Set(varRecord, Gallery1.Selected);
EditForm(EditForm1);
Navigate(EditScreen, ScreenTransition.Cover)
// EditForm1.Item: varRecord
// Back: ResetForm(EditForm1); Back(ScreenTransition.UnCover)
```

### 5. Confirm delete of selected item
```plaintext
If(
    IsBlank(Gallery1.Selected),
    Notify("Please select an item first.", NotificationType.Warning),
    UpdateContext({ showDeleteModal: true, deleteTarget: Gallery1.Selected })
)
// Confirm button:
Remove(Products, deleteTarget);
UpdateContext({ showDeleteModal: false });
Notify("Deleted: " & deleteTarget.Name, NotificationType.Success)
```

### 6. Multi-selection with a collection
```plaintext
// Checkbox OnCheck inside gallery:
If(
    Self.Value,
    Collect(varSelected, { ID: ThisItem.ID, Title: ThisItem.Title }),
    Remove(varSelected, LookUp(varSelected, ID = ThisItem.ID))
)
// Select all button:
ClearCollect(varSelected, ShowColumns(Gallery1.AllItems, "ID", "Title"))
// Deselect all:
ClearCollect(varSelected, [])
```

---

## Best Practices

- Validate inputs before using in write operations.
- Combine with related functions for complete workflows.
- Test with both empty and populated data sources.
- Consider delegation when working with large data sets.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Navigate`](./Navigate.md) | Related function |
| [`Filter`](./Filter.md) | Related function |
| [`Remove`](./Remove.md) | Related function |
| [`Patch`](./Patch.md) | Related function |
| [`EditForm`](./EditForm.md) | Related function |

---

## 🔗 Official Documentation
[Gallery control – Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/controls/control-gallery)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*