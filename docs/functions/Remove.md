# 🗑️ Remove

> **Category:** Data Write | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`Remove` **deletes one or more records** from a data source or collection. It can delete a specific record (by reference), all records matching a filter, or an entire data set.

---

## Syntax

```plaintext
// Delete a specific record
Remove( DataSource, Record [, RemoveFlags.All] )

// Delete all records in a table
Remove( DataSource, RecordTable )
```

---

## Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `DataSource` | ✅ | Data source or collection to delete from |
| `Record` | ✅ | The record to remove (from gallery, variable, or LookUp) |
| `RemoveFlags.All` | ❌ | Remove ALL matching records (use with Filter result) |

---

## Simple Examples

### 1. Delete the selected gallery item
```plaintext
Remove(Products, Gallery1.Selected);
Notify("Product deleted.", NotificationType.Success)
```

### 2. Delete from a collection
```plaintext
Remove(varCart, LookUp(varCart, ProductID = varRemoveID))
```

### 3. Delete using ThisItem inside gallery
```plaintext
// Trash icon OnSelect (inside gallery)
Remove(Tasks, ThisItem)
```

### 4. Remove from collection where a flag is set
```plaintext
Remove(varCart, Filter(varCart, IsSelected), RemoveFlags.All)
```

### 5. Remove and navigate
```plaintext
Remove(Projects, Gallery_Projects.Selected);
Navigate(ProjectListScreen, ScreenTransition.UnCover);
Notify("Project removed.", NotificationType.Success)
```

---

## Complex Examples

### 6. Confirm-before-delete modal pattern
```plaintext
// Delete button (in gallery row) — set context, show modal
UpdateContext({ showConfirm: true, pendingDelete: ThisItem })

// Confirm modal "Yes, Delete"
Remove(Tasks, pendingDelete);
UpdateContext({ showConfirm: false, pendingDelete: Blank() });
ClearCollect(varMyTasks, Filter(Tasks, AssignedTo = User().Email));
Notify(""" & pendingDelete.Title & "" deleted.", NotificationType.Success)

// Confirm modal "Cancel"
UpdateContext({ showConfirm: false, pendingDelete: Blank() })
```

### 7. Bulk delete selected items
```plaintext
// "Delete Selected" button
Set(varDeleteCount, CountRows(Filter(varSelected, IsSelected)));
ForAll(Filter(varSelected, IsSelected), Remove(Tasks, LookUp(Tasks, ID = ID)));
ClearCollect(varSelected, []);
ClearCollect(varMyTasks, Filter(Tasks, AssignedTo = User().Email));
Notify(varDeleteCount & " task(s) deleted.", NotificationType.Success)
```

### 8. Remove old log records (data housekeeping)
```plaintext
// Admin maintenance button
Remove(
    AuditLogs,
    Filter(AuditLogs, LogDate < DateAdd(Today(), -90, Days)),
    RemoveFlags.All
);
Notify("Old logs removed.", NotificationType.Success)
```

### 9. Undo pattern using a backup collection
```plaintext
// Before delete — save to undo buffer
Collect(varUndoBuffer, Gallery1.Selected);
Remove(Tasks, Gallery1.Selected);
Notify("Task deleted. Undo available for 10 seconds.", NotificationType.Warning, 10000)

// Undo button (visible for 10 seconds via Timer)
Patch(Tasks, Defaults(Tasks), First(varUndoBuffer));
ClearCollect(varUndoBuffer, []);
Notify("Delete undone.", NotificationType.Success)
```

### 10. Remove item from cart and recalculate total
```plaintext
// Cart gallery delete button
Remove(varCart, ThisItem);
Set(varCartTotal, Sum(varCart, Price * Qty));
Set(varCartCount, CountRows(varCart));
If(varCartCount = 0, Notify("Your cart is empty.", NotificationType.Information))
```

---

## Best Practices

1. **Always confirm destructive deletes** — use a modal or `Notify` with a warning.
2. **Refresh or `ClearCollect` after `Remove`** so galleries reflect the deletion.
3. **Use `RemoveFlags.All` carefully** — it can delete many records silently.
4. **Implement an undo buffer** for user-facing delete flows.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| `Remove(DS, Filter(...))` without `RemoveFlags.All` | Only removes first matching row | Add `RemoveFlags.All` |
| No refresh after remove | Gallery still shows deleted item | `Refresh(DS)` or `ClearCollect` |
| No confirmation | Users accidentally delete | Add a confirm modal pattern |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Patch`](./Patch.md) | Create/update records |
| [`Filter`](./Filter.md) | Identify records to remove |
| [`Collect`](./Collect.md) | Undo buffer / backup before remove |
| [`Refresh`](./Refresh.md) | Reload data after remove |

---

## 🔗 Official Documentation
[Remove and RemoveIf – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-remove-removeif)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*