# Power BI Report Themes – Knowledgebase

This knowledgebase provides comprehensive instructions for creating a **custom Power BI report theme** from scratch using the official JSON schema.

**Sources:**
- [Report Theme JSON Schema (v2.149)](https://github.com/microsoft/powerbi-desktop-samples/blob/main/Report%20Theme%20JSON%20Schema/reportThemeSchema-2.149.json) — local copy at `.kb/schemas/reportTheme/reportThemeSchema-2.149.json`
- [Microsoft Docs: Create custom report themes](https://learn.microsoft.com/en-us/power-bi/create-reports/report-themes-create-custom)

---

## Quick Start — Minimal Theme

The only **required** property is `name`. Everything else is optional; Power BI falls back to defaults.

```json
{
    "name": "My Custom Theme"
}
```

To enable IDE auto-complete and validation, add a `$schema` reference pointing to the local schema copy or the GitHub URL:

```json
{
    "$schema": "reportThemeSchema-2.149.json",
    "name": "My Custom Theme"
}
```

---

## Theme File Structure Overview

A report theme JSON file has **four main sections**:

| Section | Purpose |
|---|---|
| **Theme colors** (`dataColors`, `good`, `bad`, etc.) | Define the palette for data series and conditional formatting |
| **Structural colors** (`firstLevelElements`, `background`, etc.) | Control the report chrome: axes, gridlines, backgrounds, text |
| **Text classes** (`textClasses`) | Set default font size, family, color for labels, titles, callouts |
| **Visual styles** (`visualStyles`) | Per-visual-type formatting defaults and style presets |

---

## 1. Theme Colors

### Data Colors

`dataColors` is an array of hex color strings. These are assigned sequentially to data series in visuals. You can specify as many as you want; Power BI auto-generates more when the list is exhausted.

```json
{
    "name": "My Custom Theme",
    "dataColors": [
        "#118DFF",
        "#12239E",
        "#E66C37",
        "#6B007B",
        "#E044A7",
        "#744EC2",
        "#D9B300",
        "#D64550",
        "#197278",
        "#1AAB40"
    ]
}
```

### Status & Conditional Formatting Colors

| Property | Purpose |
|---|---|
| `good` | Positive status (waterfall, KPI) |
| `neutral` | Neutral status |
| `bad` | Negative status |
| `maximum` | Top of conditional formatting gradient |
| `center` | Middle of conditional formatting gradient |
| `minimum` | Bottom of conditional formatting gradient |
| `null` | Color for null values in conditional formatting |

Example:

```json
{
    "good": "#1AAB40",
    "neutral": "#D9B300",
    "bad": "#D64554",
    "maximum": "#118DFF",
    "center": "#D9B300",
    "minimum": "#DEEFFF",
    "null": "#FF7F48"
}
```

### Color Format Rules

Colors must be valid hex strings matching the pattern: `^#[0-9a-fA-F]{8}$|^#(?:[0-9a-fA-F]{3}){1,2}$`

This means:
- 3-digit hex: `#FFF`
- 6-digit hex: `#FFFFFF`
- 8-digit hex (with alpha): `#FFFFFFFF`

---

## 2. Structural Colors

Structural colors control the **report chrome** — axes, gridlines, backgrounds, labels. There are six primary structural color classes plus several aliases.

### Primary Structural Colors

| Property | Also Known As | What It Controls |
|---|---|---|
| `firstLevelElements` | `foreground` | Primary text, data labels, trend lines, axis text, card values, KPI text, slicer text |
| `secondLevelElements` | `foregroundNeutralSecondary` | Legend labels, axis labels, table/matrix headers, slicer colors, button text |
| `thirdLevelElements` | `backgroundLight` | Axis gridlines, table grid, slicer header background, gauge arc background, shape fill |
| `fourthLevelElements` | `foregroundNeutralTertiary` | Legend dimmed color, category labels, multi-row card bar, disabled button text |
| `background` | — | Data point label backgrounds, slicer dropdown background, donut/treemap strokes, button fill, filter pane background, tooltip background |
| `secondaryBackground` | `backgroundNeutral` | Table/matrix grid outline, shape map default color, ribbon fill, disabled button fill |

### Additional Structural Color Properties

The schema also supports these additional color properties (all accept hex color strings):

`tableAccent`, `foregroundLight`, `foregroundDark`, `foregroundNeutralLight`, `foregroundNeutralDark`, `foregroundNeutralSecondaryAlt`, `foregroundNeutralSecondaryAlt2`, `foregroundNeutralTertiaryAlt`, `foregroundSelected`, `foregroundButton`, `backgroundLight`, `backgroundDark`, `hyperlink`, `visitedHyperlink`, `shapeStroke`, `disabledText`, `mapPushpin`, `accent`

### Example

```json
{
    "name": "My Custom Theme",
    "firstLevelElements": "#252423",
    "secondLevelElements": "#605E5C",
    "thirdLevelElements": "#F3F2F1",
    "fourthLevelElements": "#B3B0AD",
    "background": "#FFFFFF",
    "secondaryBackground": "#C8C6C4",
    "tableAccent": "#118DFF"
}
```

> **Dark theme tip:** When building a dark theme, always set `firstLevelElements` to a light color, `background` to a dark color, and ensure `secondLevelElements` provides enough contrast against the background.

---

## 3. Text Classes

Text classes control default fonts across the report. There are **4 primary classes** and **10 secondary classes** that inherit from the primaries.

### Primary Text Classes

| Class | JSON Key | Default Font | Default Size | Controls |
|---|---|---|---|---|
| **Callout** | `callout` | DIN | 45pt | Card data labels, KPI indicators |
| **Header** | `header` | Segoe UI Semibold | 12pt | Key influencer headers |
| **Title** | `title` | DIN | 12pt | Axis titles, slicer headers, multi-row card title |
| **Label** | `label` | Segoe UI | 10pt | Table/matrix column headers, row headers, values |

### Secondary Text Classes (auto-inherit from primaries)

| Class | JSON Key | Inherits From | Override |
|---|---|---|---|
| Large Title | `largeTitle` | title | 14pt |
| Semibold | `semiboldLabel` | label | Segoe UI Semibold |
| Large | `largeLabel` | label | 12pt |
| Small | `smallLabel` | label | 9pt |
| Light | `lightLabel` | label | color: `secondLevelElements` |
| Bold | `boldLabel` | label | Segoe UI Bold |
| Large and Light | `largeLightLabel` | label | color: `secondLevelElements`, 12pt |
| Small and Light | `smallLightLabel` | label | color: `secondLevelElements`, 9pt |
| Data Title | `dataTitle` | — | — |
| Small Data Label | `smallDataLabel` | — | — |

### Text Class Properties

Each text class is an object with these optional properties:

```json
{
    "fontFace": "string",
    "fontSize": 10,
    "fontWeight": "string",
    "color": "#hexcolor"
}
```

- `fontSize` must be between **6** and **45** (inclusive)
- `color` follows the same hex color pattern as other colors

### Example

```json
{
    "name": "My Custom Theme",
    "textClasses": {
        "callout": {
            "fontSize": 45,
            "fontFace": "DIN",
            "color": "#252423"
        },
        "title": {
            "fontSize": 12,
            "fontFace": "DIN",
            "color": "#252423"
        },
        "header": {
            "fontSize": 12,
            "fontFace": "Segoe UI Semibold",
            "color": "#252423"
        },
        "label": {
            "fontSize": 10,
            "fontFace": "Segoe UI",
            "color": "#252423"
        }
    }
}
```

> **Tip:** You only need to set the 4 primary classes. Secondary classes automatically inherit from their parent primary. Override a secondary class only if you want to break the default inheritance.

---

## 4. Visual Styles

Visual styles let you set **per-visual-type defaults** and **named style presets**. This is the most powerful and complex section of a theme.

### Structure

```json
{
    "visualStyles": {
        "<visualName>": {
            "<stylePresetName>": {
                "<cardName>": [{
                    "<propertyName>": "<propertyValue>"
                }]
            }
        }
    }
}
```

| Placeholder | Description |
|---|---|
| `visualName` | Internal name of the visual type (see table below), or `"*"` for all visuals |
| `stylePresetName` | `"*"` for the default style, or a quoted name (e.g., `"My Preset"`) to create a named style preset |
| `cardName` | Formatting card (e.g., `"categoryAxis"`, `"legend"`, `"title"`), or `"*"` for all cards |
| `propertyName` | Property within the card (e.g., `"fontSize"`, `"show"`, `"color"`) |
| `propertyValue` | The value to set |

### Supported Visual Type Names

The following visual type names are defined in the schema (use these exact strings):

| Visual Type Key | Description |
|---|---|
| `barChart` | Stacked bar chart |
| `columnChart` | Stacked column chart |
| `clusteredBarChart` | Clustered bar chart |
| `clusteredColumnChart` | Clustered column chart |
| `hundredPercentStackedBarChart` | 100% stacked bar |
| `hundredPercentStackedColumnChart` | 100% stacked column |
| `lineChart` | Line chart |
| `areaChart` | Area chart |
| `stackedAreaChart` | Stacked area chart |
| `hundredPercentStackedAreaChart` | 100% stacked area |
| `lineStackedColumnComboChart` | Line and stacked column combo |
| `lineClusteredColumnComboChart` | Line and clustered column combo |
| `ribbonChart` | Ribbon chart |
| `waterfallChart` | Waterfall chart |
| `funnel` | Funnel chart |
| `scatterChart` | Scatter chart |
| `pieChart` | Pie chart |
| `donutChart` | Donut chart |
| `treemap` | Treemap |
| `map` | Map (bubble) |
| `filledMap` | Filled map (choropleth) |
| `shapeMap` | Shape map |
| `azureMap` | Azure map |
| `gauge` | Gauge |
| `card` | Card (legacy) |
| `cardVisual` | Card (new) |
| `multiRowCard` | Multi-row card |
| `kpi` | KPI |
| `slicer` | Slicer (legacy) |
| `advancedSlicerVisual` | Advanced slicer |
| `textSlicer` | Text slicer |
| `listSlicer` | List slicer |
| `tableEx` | Table |
| `pivotTable` | Matrix |
| `scriptVisual` | R script visual |
| `pythonVisual` | Python visual |
| `keyDriversVisual` | Key influencers |
| `decompositionTreeVisual` | Decomposition tree |
| `qnaVisual` | Q&A |
| `aiNarratives` | Smart narrative |
| `scorecard` | Scorecard |
| `rdlVisual` | Paginated report visual |
| `actionButton` | Button |
| `bookmarkNavigator` | Bookmark navigator |
| `textbox` | Text box |
| `pageNavigator` | Page navigator |
| `shape` | Shape |
| `image` | Image |

Additionally, `"*"` as a visual name applies to **all** visual types.

### Common Formatting Cards

These cards are shared across most visuals (from the `commonCards` definition in the schema):

| Card | Purpose | Key Properties |
|---|---|---|
| `background` | Visual background | `color`, `show`, `transparency` |
| `border` | Visual border | `color`, `radius`, `show`, `width` |
| `title` | Visual title | `show`, `text`, `fontColor`, `fontFamily`, `fontSize`, `bold`, `italic`, `underline`, `alignment`, `background`, `heading`, `titleWrap` |
| `subTitle` | Visual subtitle | Same properties as `title` |
| `general` | General properties | `altText`, `width`, `height`, `x`, `y`, `keepLayerOrder` |
| `padding` | Inner padding | `top`, `bottom`, `left`, `right` |
| `dropShadow` | Shadow effects | `show`, `color`, `preset`, `transparency`, `shadowBlur`, `shadowDistance`, `shadowSpread`, `position`, `angle` |
| `visualHeader` | Header icon bar | `show`, `foreground`, `background`, `border`, `transparency`, plus many `show*Button` toggles |
| `visualHeaderTooltip` | Header tooltip | `type`, `text`, `fontFamily`, `fontSize`, `bold`, `italic`, `background` |
| `divider` | Divider line | `show`, `color`, `width`, `style`, `ignorePadding` |
| `lockAspect` | Lock aspect ratio | `show` |
| `spacing` | Spacing controls | `customizeSpacing`, `spaceBelowTitle`, `spaceBelowSubTitle`, `spaceBelowTitleArea`, `verticalSpacing` |
| `stylePreset` | Style preset selection | `name` |

### Chart-Specific Cards

Depending on the visual type, additional formatting cards are available. Common ones include:

- `categoryAxis` / `valueAxis` — axis formatting (gridlines, labels, colors, titles)
- `legend` — legend position, font, colors
- `labels` / `dataLabels` — data label formatting
- `dataPoint` — data point colors and formatting
- `plotArea` — chart plot area background
- `lineStyles` — line chart styling (width, dash, markers)
- `bubbles` — scatter chart bubble size

> **Use the schema file** at `.kb/schemas/reportTheme/reportThemeSchema-2.149.json` as the definitive reference. Each visual type is defined under `#/definitions/visual-{visualName}` with its full list of supported cards and properties.

### Color Value Format in Visual Styles

When setting colors inside `visualStyles`, use the **fill** format (not bare hex strings):

```json
{ "solid": { "color": "#118DFF" } }
```

You can also reference theme data colors dynamically:

```json
{ "solid": { "color": { "expr": { "ThemeDataColor": { "ColorId": 0, "Percent": 0 } } } } }
```

- `ColorId` — zero-based index into the `dataColors` array
- `Percent` — lightness adjustment from `-1` (darker) to `1` (lighter)

### Example: Visual Styles

```json
{
    "name": "My Custom Theme",
    "visualStyles": {
        "*": {
            "*": {
                "*": [{
                    "wordWrap": true
                }],
                "title": [{
                    "fontFamily": "Segoe UI Semibold",
                    "fontSize": 12,
                    "bold": true
                }],
                "background": [{
                    "show": false
                }],
                "border": [{
                    "show": false
                }],
                "categoryAxis": [{
                    "gridlineStyle": "dotted"
                }]
            }
        },
        "lineChart": {
            "*": {
                "lineStyles": [{
                    "strokeWidth": 3
                }]
            }
        },
        "scatterChart": {
            "*": {
                "bubbles": [{
                    "bubbleSize": -10
                }]
            }
        }
    }
}
```

---

## 5. Style Presets

Style presets let you define **named formatting configurations** that users can switch between in the Format pane.

### Defining Presets

1. Under the visual type's `"*"` (default) key, add a `stylePreset` card listing the default preset name.
2. Add sibling keys for each named preset with their formatting overrides.

```json
{
    "visualStyles": {
        "columnChart": {
            "*": {
                "stylePreset": [{
                    "name": "Clean Look"
                }],
                "title": [{
                    "show": true,
                    "fontSize": 14
                }]
            },
            "Clean Look": {
                "legend": [{
                    "position": "BottomCenter"
                }],
                "valueAxis": [{
                    "gridlineColor": { "solid": { "color": "#CCCCCC" } }
                }]
            },
            "Bold Accent": {
                "legend": [{
                    "position": "Right",
                    "italic": true
                }],
                "valueAxis": [{
                    "gridlineColor": { "solid": { "color": "#0000FF" } }
                }]
            }
        }
    }
}
```

Named presets inherit from the `"*"` default style; only specify overrides.

---

## 6. Filter Cards & Filter Pane

Report themes can also style the **filter pane** and **filter cards** via the `page` visual style:

```json
{
    "visualStyles": {
        "page": {
            "*": {
                "outspacePane": [{
                    "backgroundColor": { "solid": { "color": "#F3F2F1" } },
                    "foregroundColor": { "solid": { "color": "#252423" } },
                    "fontFamily": "Segoe UI",
                    "titleSize": 14,
                    "headerSize": 12,
                    "border": true,
                    "borderColor": { "solid": { "color": "#E0E0E0" } }
                }],
                "filterCard": [
                    {
                        "$id": "Applied",
                        "foregroundColor": { "solid": { "color": "#252423" } },
                        "backgroundColor": { "solid": { "color": "#FFFFFF" } },
                        "border": true
                    },
                    {
                        "$id": "Available",
                        "border": true,
                        "backgroundColor": { "solid": { "color": "#F9F9F9" } }
                    }
                ]
            }
        }
    }
}
```

---

## 7. Complete Theme Template

Below is a comprehensive template incorporating all major sections. Copy and customize as needed.

```json
{
    "$schema": "reportThemeSchema-2.149.json",
    "name": "My Organization Theme",

    "dataColors": [
        "#118DFF", "#12239E", "#E66C37", "#6B007B",
        "#E044A7", "#744EC2", "#D9B300", "#D64550"
    ],

    "good": "#1AAB40",
    "neutral": "#D9B300",
    "bad": "#D64554",
    "maximum": "#118DFF",
    "center": "#D9B300",
    "minimum": "#DEEFFF",
    "null": "#FF7F48",

    "firstLevelElements": "#252423",
    "secondLevelElements": "#605E5C",
    "thirdLevelElements": "#F3F2F1",
    "fourthLevelElements": "#B3B0AD",
    "background": "#FFFFFF",
    "secondaryBackground": "#C8C6C4",
    "tableAccent": "#118DFF",

    "textClasses": {
        "callout": {
            "fontSize": 45,
            "fontFace": "DIN",
            "color": "#252423"
        },
        "title": {
            "fontSize": 12,
            "fontFace": "DIN",
            "color": "#252423"
        },
        "header": {
            "fontSize": 12,
            "fontFace": "Segoe UI Semibold",
            "color": "#252423"
        },
        "label": {
            "fontSize": 10,
            "fontFace": "Segoe UI",
            "color": "#252423"
        }
    },

    "visualStyles": {
        "*": {
            "*": {
                "title": [{
                    "show": true,
                    "fontColor": { "solid": { "color": "#252423" } },
                    "fontFamily": "Segoe UI Semibold",
                    "fontSize": 12
                }],
                "background": [{
                    "show": false
                }],
                "border": [{
                    "show": false
                }],
                "visualHeader": [{
                    "show": false
                }]
            }
        }
    }
}
```

---

## 8. How to Apply a Custom Theme

1. **Power BI Desktop:** Go to **View** → **Themes** → **Browse for themes** and select your `.json` file.
2. **Programmatically:** Use [Semantic Link Labs](https://semantic-link-labs.readthedocs.io/en/stable/sempy_labs.report.html) in Microsoft Fabric notebooks to extract/apply themes at scale.
3. **JavaScript API:** Use the [Power BI JavaScript API](https://learn.microsoft.com/en-us/javascript/api/overview/powerbi/apply-report-themes) to apply themes in embedded reports.

---

## 9. Tips & Best Practices

- **Only set what you need.** Omitted properties fall back to Power BI defaults.
- **Use the schema for auto-complete.** Point `$schema` at the local schema file and use VS Code's IntelliSense (`Ctrl+Space`) to explore available properties.
- **Test incrementally.** Start with `name` + `dataColors`, import, verify, then add sections.
- **Color format context matters:**
  - Top-level structural colors (`firstLevelElements`, `background`, etc.): bare hex strings (`"#FFFFFF"`)
  - Colors inside `visualStyles`: fill objects (`{ "solid": { "color": "#FFFFFF" } }`)
  - Colors inside `dataColors`: bare hex strings
- **Conditional formatting cannot be set via themes.** It must be applied per-visual after importing the theme.
- **`fontSize` range:** must be between 6 and 45 (inclusive).
- **Finding visual property names:** Save a report as PBIP, manually format a visual, then inspect the visual's JSON file to discover the correct card and property names. Translate the PBIR `objects` format to the theme `visualStyles` format (see the Microsoft docs for examples).
- **Dark themes:** Be sure to set `firstLevelElements` (light text), `background` (dark), and `secondLevelElements` (mid-tone) to maintain readability.

---

## 10. Schema Reference

The full JSON schema is stored locally at:

```
.kb/schemas/reportTheme/reportThemeSchema-2.149.json
```

This schema defines every valid property, visual type, formatting card, and value type. Use it as the definitive reference when the documentation above doesn't cover a specific property.

### Key Schema Definitions

| Definition | Type | Description |
|---|---|---|
| `color` | `string` | Hex color (`#RGB`, `#RRGGBB`, or `#AARRGGBB`) |
| `fill` | `object` | Color container: `{ "solid": { "color": ... } }`, gradient, or pattern |
| `themeDataColor` | `object` | Dynamic reference to a theme data color by index |
| `textClass` | `object` | Font properties: `fontFace`, `fontSize`, `fontWeight`, `color` |
| `fontSize` | `number` | Font size, min 6, max 45 |
| `alignment` | `string` | `"left"`, `"center"`, or `"right"` |
| `image` | `object` | Image reference with `name`, `url`, and optional `scaling` |
| `fillRule` | `object` | Conditional formatting gradient rules (`linearGradient2`, `linearGradient3`) |
| `dataBars` | `object` | Data bar formatting with colors and direction |

### Visual-Specific Schema References

Each visual type's full property set is defined at `#/definitions/visual-{visualName}` in the schema. For example:
- `#/definitions/visual-barChart` — all formatting cards for bar charts
- `#/definitions/visual-lineChart` — all formatting cards for line charts
- `#/definitions/visual-cardVisual` — all formatting cards for the new card visual

Use these definitions to discover all available formatting cards and their properties for each visual type.
