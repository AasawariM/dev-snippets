# utable-filtering

- reference :
  - https://ui.nuxt.com/docs/components/select-menu
  - https://ui.nuxt.com/docs/components/table#columns
  - https://ui.nuxt.com/docs/components/table#with-column-filters

- logic:
  USelectMenu
  ↓
  Dropdown change
  ↓
  User selects "paid"
  ↓
  val = "paid"
  ↓
  Get selected value
  ↓
  if "All Status"
  remove filter
  else
  filter by that status(paid here)

- table?.tableApi?.getColumn('status')?.setFilterValue(val)
  means:
  Update table filter → status = selected value
  show rows where status = Selected Value

- val === 'All Status' ? undefined : val
  means:
  if selected value is "All Status"
  remove filter
  else
  apply filter

- ?? 'All Status'
  means,
  if filter value exists → show it
  else → show "All Status"
  because initially -> getFilterValue() → undefined , so it shows nothing

# Global Filter in UTable

`UTable` provides a **global search feature** that filters table rows based on text entered by the user.

It internally searches across **all column values** and displays only the rows that match the typed text.

## Reference

https://ui.nuxt.com/docs/components/table#with-global-filter

## Logic

1. Create a reactive variable called `globalFilter`.
2. Bind this variable to both **UInput** and **UTable** using `v-model`.
3. When the user types in the input, the value updates `globalFilter`.
4. `UTable` listens to `v-model:global-filter` and automatically filters rows.

### Flow

UInput → updates `globalFilter`
`globalFilter` → controls table filtering
UTable → filters rows automatically

### Implementation Idea

- `globalFilter` stores the search text.
- `UInput` updates the filter value.
- `UTable` uses that value to filter rows across all columns.

This allows users to quickly search table data without implementing custom filtering logic.
