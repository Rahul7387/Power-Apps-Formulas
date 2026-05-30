# ✂️ Split

> **Category:** Text | **Works in:** Canvas Apps

---

## Overview

Splits a text string into a single-column table of substrings using a separator character.

---

## Syntax

```plaintext
Split( Text, Separator )
```

---

## Simple Examples

### 1. Split CSV string
```plaintext
Split("a,b,c", ",")  // Table: {Value:"a"}, {Value:"b"}, {Value:"c"}
```

### 2. Count tags
```plaintext
CountRows(Split(ThisItem.Tags, ","))
```

### 3. Check if tag exists
```plaintext
"PowerApps" in Split(ThisItem.Tags, ",").Value
```

---

## Complex Examples

### 4. Gallery of tags from a comma-separated field
```plaintext
// Tag chip gallery Items:
ForAll(
    Split(Gallery1.Selected.Tags, ","),
    { Tag: TrimEnds(Value) }
)
```

### 5. Convert pipe-separated string to searchable collection
```plaintext
ClearCollect(
    varTagList,
    ForAll(Split(varTagString, "|"), { Tag: TrimEnds(Value) })
)
// Search: !IsEmpty(Filter(varTagList, StartsWith(Tag, txtSearch.Text)))
```

### 6. Validate that all tags are from an allowed list
```plaintext
// Check every tag in user input is in the allowed set
And(
    ForAll(
        Split(txtTags.Text, ","),
        TrimEnds(Value) in AllowedTags.TagName
    )
)
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
| [`Filter`](./Filter.md) | Related function |
| [`Collect`](./Collect.md) | Related function |
| [`ForAll`](./ForAll.md) | Related function |
| [`StringFunctions`](./StringFunctions.md) | Related function |

---

## 🔗 Official Documentation
[Split – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-split)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*