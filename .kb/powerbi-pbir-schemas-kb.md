# Power BI Report (PBIR) JSON Schema Reference

This knowledgebase provides the **latest JSON schemas** for the Power BI Enhanced Report format (PBIR). Use these schemas to understand and generate valid PBIR report definitions.

**Source:** [microsoft/json-schemas](https://github.com/microsoft/json-schemas/tree/main/fabric/item/report/definition) on GitHub.

---

## Schema Inventory

All schema files are organized under `.kb/schemas/` in per-schema subdirectories.

| Schema | Latest Version | Path | Purpose |
|---|---|---|---|
| **report** | 3.1.0 | `schemas/report/schema.json` | Top-level report definition: themes, report-level filters, formatting objects, settings |
| **page** | 2.0.0 | `schemas/page/schema.json` | Single report page: dimensions, display options, page-level filters, bindings |
| **pagesMetadata** | 1.0.0 | `schemas/pagesMetadata/schema.json` | Page ordering and active page selection |
| **visualContainer** | 2.6.0 | `schemas/visualContainer/schema.json` | A visual or visual group on a page: position, size, z-index, tab order, filters |
| **visualConfiguration** | 2.2.0 | `schemas/visualConfiguration/schema.json`, `schema-embedded.json` | Visual chart config: visual type, query/data bindings, formatting, sync groups |
| **filterConfiguration** | 1.2.0 | `schemas/filterConfiguration/schema.json`, `schema-embedded.json` | Filter definitions for report, page, or visual level |
| **semanticQuery** | 1.3.0 | `schemas/semanticQuery/schema.json` | Shared definitions for queries, filters, and expressions (referenced by other schemas) |
| **formattingObjectDefinitions** | 1.4.0 | `schemas/formattingObjectDefinitions/schema.json` | Shared definitions for visual/report formatting objects and selectors |
| **bookmark** | 2.0.0 | `schemas/bookmark/schema.json` | Bookmark state capture: display name, exploration state, target visuals |
| **bookmarksMetadata** | 1.0.0 | `schemas/bookmarksMetadata/schema.json` | Bookmark groups and ordering |
| **reportExtension** | 1.0.0 | `schemas/reportExtension/schema.json` | Report-level extensions: additional measures on semantic model entities |
| **versionMetadata** | 1.0.0 | `schemas/versionMetadata/schema.json` | Report definition version tracking (major.minor.0) |

---

## PBIR Report File Structure

A PBIR report definition uses a folder hierarchy where each entity (page, visual, bookmark) is a separate JSON file:

```text
*.Report/
└── definition/
    ├── version.json                          ← versionMetadata schema
    ├── report.json                           ← report schema (top-level)
    ├── pages/
    │   ├── pages.json                        ← pagesMetadata schema (page order)
    │   ├── {pageName}/
    │   │   ├── page.json                     ← page schema
    │   │   └── visuals/
    │   │       ├── {visualName}/
    │   │       │   └── visual.json           ← visualContainer schema
    │   │       └── ...
    │   └── ...
    ├── bookmarks/
    │   ├── bookmarks.json                    ← bookmarksMetadata schema
    │   └── {bookmarkName}.json              ← bookmark schema
    └── extensions/
        └── {extensionName}.json              ← reportExtension schema
```

---

## Schema Relationships

The schemas reference each other via `$ref`. The dependency graph is:

```
report.json
  ├── uses filterConfiguration (embedded) for report-level filters
  └── uses formattingObjectDefinitions for report formatting objects

page.json
  └── uses filterConfiguration (embedded) for page-level filters

visualContainer (visual.json)
  ├── uses visualConfiguration (embedded) for the visual chart definition
  └── uses filterConfiguration (embedded) for visual-level filters

visualConfiguration
  ├── uses formattingObjectDefinitions for visual formatting objects
  └── uses semanticQuery definitions for query expressions, filters, projections

filterConfiguration
  └── uses semanticQuery definitions for filter expressions
```

**Embedded vs standalone schemas:** Some schemas have both `schema.json` (standalone, with `$id`) and `schema-embedded.json` (for `$ref` from parent schemas). When a schema is referenced inline from another schema (e.g., `visualConfiguration` inside `visualContainer`), the embedded variant is used.

---

## Key Schema Details

### report (v3.1.0)
- **Required:** `$schema`
- **Key properties:** `themeCollection`, `filterConfig`, `objects`, `reportSource`, `publicCustomVisuals`, `settings`
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/report/3.1.0/schema.json`

### page (v2.0.0)
- **Required:** `$schema`
- **Key properties:** `name` (unique ID, max 50 chars), `displayName`, `displayOption`, `height`, `width`, `filterConfig`, `pageBinding`, `objects`, `type`
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/page/2.0.0/schema.json`

### pagesMetadata (v1.0.0)
- **Required:** `$schema`
- **Key properties:** `pageOrder` (array of page names), `activePageName`
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/pagesMetadata/1.0.0/schema.json`

### visualContainer (v2.6.0)
- **Required:** `$schema`
- **Key properties:** `name` (unique per page, max 50 chars), `position` (x, y, width, height, z, tabOrder), `visual` (→ visualConfiguration), `visualGroup`, `parentGroupName`, `filterConfig`, `isHidden`, `howCreated`
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.6.0/schema.json`

### visualConfiguration (v2.2.0)
- **Required:** `$schema`, `visualType`
- **Key properties:** `visualType` (e.g., `"barChart"`, `"lineChart"`, `"card"`, `"slicer"`, `"tableEx"`, `"pivotTable"`), `query`, `objects`, `visualContainerObjects`, `syncGroup`, `drillFilterOtherVisuals`
- `query.queryState` maps data roles (e.g., `Category`, `Y`, `Values`, `Series`, `X`, `Size`, `Details`, `Latitude`, `Longitude`) to projections
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualConfiguration/2.2.0/schema.json`

### filterConfiguration (v1.2.0)
- **Required:** `$schema`
- **Key properties:** `filters` (array of filter items with type, filter expression, display settings)
- `$schema` must be: `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/filterConfiguration/1.2.0/schema.json`

### semanticQuery (v1.3.0)
- **No standalone file** — this is a definitions-only schema referenced by others
- **Key definitions:** `FilterDefinition` (Version, From, Where), `QueryFilter` (Target, Condition), `QueryExpressionContainer`, `EntitySource`, column/measure/aggregation expressions

### formattingObjectDefinitions (v1.4.0)
- **No standalone file** — definitions-only schema
- **Key definitions:** `DataViewObjectDefinitions`, `DataViewObjectDefinition` (selector + properties), `Selector` (data, metadata, id scoping)

### bookmark (v2.0.0)
- **Required:** `$schema`, `displayName`, `name`, `explorationState`
- **Key properties:** `options` (applyOnlyToTargetVisuals, targetVisualNames, suppressData, suppressDisplay)

### versionMetadata (v1.0.0)
- **Required:** `$schema`, `version`
- `version` format: `major.minor.0` (e.g., `"4.0"` pattern: `^[1-9][0-9]*\.(0|[1-9][0-9]*)\.0$`)

---

## Common Patterns for Agents

### Referencing a Column in a Query Projection

```json
{
  "field": {
    "Column": {
      "Expression": { "SourceRef": { "Entity": "TableName" } },
      "Property": "ColumnName"
    }
  },
  "queryRef": "TableName.ColumnName",
  "nativeQueryRef": "ColumnName"
}
```

### Referencing a Measure in a Query Projection

```json
{
  "field": {
    "Measure": {
      "Expression": { "SourceRef": { "Entity": "TableName" } },
      "Property": "MeasureName"
    }
  },
  "queryRef": "TableName.MeasureName",
  "nativeQueryRef": "MeasureName"
}
```

### Visual Container Position

```json
{
  "position": {
    "x": 0,
    "y": 0,
    "width": 300,
    "height": 200,
    "z": 1000,
    "tabOrder": 0
  }
}
```

### Page-Level Filter (Basic Example)

```json
{
  "filters": [
    {
      "type": "Categorical",
      "filter": {
        "Version": 2,
        "From": [{ "Name": "t", "Entity": "TableName", "Type": 0 }],
        "Where": [
          {
            "Condition": {
              "In": {
                "Expressions": [
                  { "Column": { "Expression": { "SourceRef": { "Source": "t" } }, "Property": "ColumnName" } }
                ],
                "Values": [[{ "Literal": { "Value": "'SomeValue'" } }]]
              }
            }
          }
        ]
      }
    }
  ]
}
```

### TopN Filter

```json
{
  "type": "TopN",
  "filter": {
    "TopN": {
      "Top": true,
      "Count": 5,
      "OrderBy": [
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "TableName" } },
              "Property": "MeasureName"
            }
          }
        }
      ]
    }
  }
}
```

---

## Common Schema Pitfalls & Validation Rules

These rules were discovered through real schema validation errors in Power BI Desktop. **Always follow these to avoid import failures.**

### version.json

1. **`version` must be `"2.0.0"`, NOT a higher value.** The `version` field in `version.json` (versionMetadata schema) must follow the pattern `major.minor.0` and must be a version recognized by PBI Desktop. Using `"4.0.0"` or other unsupported values causes PBI Desktop to silently fail loading pages, resulting in a blank report with `Cannot read properties of undefined (reading 'visualContainers')` JavaScript error. Always use `"2.0.0"`.

### report.json

2. **`themeCollection` is effectively required.** Power BI Desktop expects a `themeCollection` object with at least a `baseTheme`. Omitting it causes an import error. Always include:
   ```json
   "themeCollection": {
     "baseTheme": {
       "name": "CY24SU06",
       "reportVersionAtImport": { "visual": "2.6.0", "page": "2.0.0", "report": "3.1.0" },
       "type": "SharedResources"
     }
   }
   ```

3. **`resourcePackages` is required for themes to load.** PBI Desktop needs explicit resource package entries to map theme names to their internal paths. Without this, the base theme cannot be resolved and the report fails to render. Always include:
   ```json
   "resourcePackages": [
     {
       "name": "SharedResources",
       "type": "SharedResources",
       "items": [
         { "name": "CY24SU06", "path": "BaseThemes/CY24SU06.json", "type": "BaseTheme" }
       ]
     }
   ]
   ```

4. **Always include `slowDataSourceSettings` for DirectQuery reports.** Omitting this for DirectQuery-backed reports can cause rendering issues. Include it with default values:
   ```json
   "slowDataSourceSettings": {
     "isCrossHighlightingDisabled": false,
     "isSlicerSelectionsButtonEnabled": false,
     "isFilterSelectionsButtonEnabled": false,
     "isFieldWellButtonEnabled": false,
     "isApplyAllButtonEnabled": false
   }
   ```

5. **`reportVersionAtImport` must be an object, NOT a string.** The schema defines `ThemeVersion` as an object with three semver string properties (`visual`, `page`, `report`). Writing `"reportVersionAtImport": "5.54"` will fail validation.

6. **`settings` does NOT accept arbitrary keys.** Only properties explicitly defined in the report schema are allowed. For example, `useNewFilterPaneExperience` is **not** a valid setting and will cause an import error. Always validate property names against the schema before including them.

### visual.json (visualContainer + embedded visualConfiguration)

7. **Do NOT put `$schema` inside the `visual` object.** The `visual` property in a visualContainer is validated against the **embedded** visualConfiguration schema (`schema-embedded.json`), which does **not** define a `$schema` property and has `additionalProperties: false`. Only the **top-level** visualContainer object gets `$schema`. Incorrect:
   ```json
   {
     "$schema": "...visualContainer/2.6.0/schema.json",
     "visual": {
       "$schema": "...visualConfiguration/2.2.0/schema.json",  // ❌ INVALID
       "visualType": "barChart"
     }
   }
   ```
   Correct:
   ```json
   {
     "$schema": "...visualContainer/2.6.0/schema.json",
     "visual": {
       "visualType": "barChart"  // ✅ No $schema here
     }
   }
   ```

8. **`query` does NOT support `sortOrder`.** The `QueryState` schema does not define a `sortOrder` property. Sorting is configured through visual formatting objects (`objects`), not in the query definition itself.

9. **`dataViewWildcard.matchingOption` must be a number, NOT a string.** The enum values are numeric:
   - `0` = InstancesAndTotals
   - `1` = Instances (only)
   - `2` = Totals (only)
   
   Writing `"matchingOption": "InstancesAndTotals"` will fail. Use `"matchingOption": 0`.

### General Rules

10. **Embedded schemas reject unknown properties.** When a schema is referenced via `$ref` from a parent (e.g., visualConfiguration embedded in visualContainer, or filterConfiguration embedded in page/visual), the embedded variant typically sets `additionalProperties: false`. Never add extra properties not defined in the schema.

11. **Always check property types against the schema.** Several properties use non-obvious types:
   - `matchingOption` → number (not string)
   - `reportVersionAtImport` → object (not string)
   - `displayOption` → string enum (e.g., `"FitToPage"`, not a number)
   - Filter `Type` → number (not string)

---

## Notes

- All JSON files **must** include the `$schema` property with the exact URL for the specific schema version.
- Entity/table names and property/column names are **case-sensitive** and must match the semantic model exactly.
- `name` properties on pages and visuals are internal identifiers (not display names) and must be unique within their scope.
- The `schema-embedded.json` variants are used when the schema is referenced inline via `$ref` from a parent schema; use `schema.json` for standalone validation.
- For full schema details, refer to the JSON schema files under `.kb/schemas/{schemaName}/`.
