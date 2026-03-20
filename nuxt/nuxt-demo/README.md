# utable-filtering

## References:

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

- used USelectMenu to have search functionality inside dropdown
- Add columnFilters and bind it to the table
- v-model:column-filters="columnFilters" =>Means, Table filters <----> columnFilters
- i.e connects the table filtering system to your variable.

_In Vue, v-model is a shorthand for two-way binding. Internally it uses :model-value to pass data to the component and @update:model-value to listen for changes from the component._

`v-model = :model-value + @update:model-value`

# Global Filter in UTable

`UTable` provides a **global search feature** that filters table rows based on text entered by the user.

It internally searches across **all column values** and displays only the rows that match the typed text.

## References: https://ui.nuxt.com/docs/components/table#with-global-filter

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

# Form With Valibot Validation

## References:

https://ui.nuxt.com/docs/components/form
https://ui.nuxt.com/docs/components/form#schema-validation

=> A form component with built-in validation and submission handling.

- Valibot is a validation library for JavaScript/TypeScript — used to validate form data, APIs, and objects in a clean and type-safe way.
- Valibot helps you:
  Define rules for your data
  Check if data is valid
  Get errors if something is wrong

Steps:

1. STEP 1:Add state (data storage)
2. STEP 2: Connect form to state
3. STEP 3: Add v-model to inputs
4. STEP 4: Update submit function
5. STEP 5: Add validation (valibot)
   - Install
     npm install valibot
   - Create Validation Schema
   - Connect with <UForm>
   - Show Errors Automatically
6. STEP 6: Add infer type if needed(optional)

**Validation Basics**
v.string() → required (but allows empty)
v.minLength(1) → required + non-empty
v.optional() → optional field

**Notes**

- Use v-model.number for numeric inputs
- Ensure state and schema types match
- Cross-field validation (e.g. min salary ≤ max salary) is handled inside onSubmit
  because Valibot does not directly display object-level errors in UI.
- Valibot supports object-level validation, but Nuxt UI does not display those errors directly in the UI. So cross-field validation is handled in onSubmit to show feedback using toast.

**Toast**

## Reference: https://ui.nuxt.com/docs/components/toast

- add <UToaster /> in app.vue
  Because:
  - useToast() provides the toast API
  - <UToaster /> is required to render toast notifications in the UI

<!-- (Currently skill input is static; can be extended to dynamic list) -->
