
# template-report-kb

This file defines the **rules** to adapt the template report using PBIR file format to a semantic model being developed.

**The report is using the PBIR file format**, that is a public format for Power BI reports using JSON files. More information in the [docs](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-report?tabs=v2%2Cdesktop#pbir-format). Each page, visual, bookmark, etc., is organized into a separate, individual file within a folder structure.

---

## Report Structure (2 Pages)

The template contains **2 pages** in this order:

| Page folder | Page display name | Purpose |
|---|---|---|
| `mainPage` | Atlanta \| FabCon Planning | Attraction exploration + itinerary planning |
| `hotelPage` | Hotel Analysis | Hotel portfolio analysis for planners |

```text
templateReport/
├── definition/
│   ├── pages/
│   │   ├── pages.json                         ← page order registry
│   │   ├── mainPage/
│   │   │   ├── page.json
│   │   │   └── visuals/
│   │   │       ├── logo/visual.json
│   │   │       ├── title/visual.json
│   │   │       ├── accommodationCard/visual.json
│   │   │       ├── featuredCard1/visual.json
│   │   │       ├── featuredCard2/visual.json
│   │   │       ├── featuredCard3/visual.json
│   │   │       ├── dateSlicer/visual.json      ← Category tile slicer
│   │   │       ├── barChart/visual.json        ← Neighborhood bar chart
│   │   │       ├── topCard/visual.json         ← Top Attraction card
│   │   │       ├── mapVisual/visual.json
│   │   │       ├── itineraryGrid/visual.json
│   │   │       └── timeSeries/visual.json      ← Submitted Itineraries table
│   │   └── hotelPage/
│   │       ├── page.json
│   │       └── visuals/
│   │           ├── logo/visual.json
│   │           ├── title/visual.json
│   │           ├── topCard/visual.json         ← 4 KPI cards
│   │           ├── brandSlicer/visual.json
│   │           ├── ratingSlicer/visual.json
│   │           ├── hotelSlicer/visual.json
│   │           ├── priceTrendsChart/visual.json
│   │           ├── scatterChart/visual.json
│   │           └── hotelTable/visual.json
```

---

## Before You Start

- **CRITICAL:** Make sure you understand the semantic model — especially table names, column names, and measure names. Use exact case-sensitive names in all JSON.
- The visual folder names in the template are descriptive aliases. Do not rename folders.

---

## Copy Template Report to PBIP

Copy the `templateReport` content from the `.kb/` folder to the `*.Report/` folder of the semantic model. Overwrite any existing files.

---

## Page 1 — Atlanta | FabCon Planning (mainPage)

Canvas: 1280 × 720 (16:9), light theme.

### Layout Overview

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ [logo][  title  ]  [ accommodationCard ]  [ featuredCard1 ][ featuredCard2 ][ featuredCard3 ] │
│                                                                                      │
│  [categorySlicer ]  [          mapVisual (center)         ]  [ itineraryGrid      ] │
│  [  barChart     ]  [                                     ]  [ Name input         ] │
│  [ topAttractionCard]                                        [ Hotel input        ] │
│                                                              [       Submit       ] │
│                                                              [ submittedItinerariesTable ] │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

### Visual Configurations — Page 1

#### `logo`
No changes needed. Leave as-is.

#### `title`
Pre-filled with "Atlanta | FabCon Planning". No changes needed.

#### `accommodationCard`
Shows the last submitted itinerary entry: attendee name, hotel, and timestamp.

Configure the `query.queryState.Data.projections` with these fields from the `Itineraries` table:
- `Itineraries[Attendee]` (Column)
- `Itineraries[Hotel]` (Column)
- `Itineraries[CreatedUTC]` (Column)

```json
"query": {
  "queryState": {
    "Data": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Itineraries" } },
              "Property": "Attendee"
            }
          },
          "queryRef": "Itineraries.Attendee",
          "nativeQueryRef": "Attendee"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Itineraries" } },
              "Property": "Hotel"
            }
          },
          "queryRef": "Itineraries.Hotel",
          "nativeQueryRef": "Hotel"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Itineraries" } },
              "Property": "CreatedUTC"
            }
          },
          "queryRef": "Itineraries.CreatedUTC",
          "nativeQueryRef": "CreatedUTC"
        }
      ]
    }
  }
}
```

#### `featuredCard1`, `featuredCard2`, `featuredCard3`
Each shows a single featured attraction (pre-filtered by name). Configure `query.queryState.Data.projections` with:
- `Attractions[Attraction Name]` (Column)
- `Attractions[Rating]` (Column)
- `Attractions[Address]` (Column)
- `Attractions[Description]` (Column)
- `Attractions[Image URL]` (Column)

Apply a visual-level filter on `Attractions[Attraction Name]` equals the specific attraction name:
- `featuredCard1` → "Atlanta Botanical Garden"
- `featuredCard2` → "Centennial Olympic Park"
- `featuredCard3` → "Chick-fil-A College Football Hall of Fame"

#### `dateSlicer` — Category Tile Slicer
Pre-configured for `Attractions[Category]` in Tile mode. No changes needed unless the column name differs in your model.

#### `barChart` — Attraction Count by Neighborhood
Pre-configured with `Attractions[Neighborhood]` as Category and `[Attraction Count]` measure as Y. Verify measure name matches your model.

#### `topCard` — Top Attraction Card
Pre-configured to show top-rated attraction: `Attractions[Attraction Name]`, `Attractions[Image URL]`, and `[Average Attraction Rating]`. Add a TopN filter to show only the single highest-rated attraction:

```json
"filters": [
  {
    "filter": {
      "TopN": {
        "Top": true,
        "Count": 1,
        "OrderBy": [
          {
            "field": {
              "Measure": {
                "Expression": { "SourceRef": { "Entity": "Attractions" } },
                "Property": "Average Attraction Rating"
              }
            }
          }
        ]
      }
    }
  }
]
```

#### `mapVisual` — Itinerary Map (Azure Maps)
Configure with hotel and attraction coordinates. Add both latitude/longitude fields from Hotels and Attractions.

```json
"query": {
  "queryState": {
    "Latitude": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Latitude"
            }
          },
          "queryRef": "Hotels.Latitude",
          "nativeQueryRef": "Latitude"
        }
      ]
    },
    "Longitude": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Longitude"
            }
          },
          "queryRef": "Hotels.Longitude",
          "nativeQueryRef": "Longitude"
        }
      ]
    }
  }
}
```

Set map style to Grayscale (Light) via the visual objects `style` property.

#### `itineraryGrid` — Attraction Image Grid (Tile Slicer)
Slicer in Tile mode showing attraction images. Configure with `Attractions[Attraction Name]` as the Values field. Enable multi-select.

#### `timeSeries` — Submitted Itineraries Table
Pre-configured with columns from `Itineraries` table: CreatedUTC, Attendee, Hotel, Attraction 1, Attraction 2. No changes needed unless column names differ.

---

## Page 2 — Hotel Analysis (hotelPage)

Canvas: 1280 × 720 (16:9), light theme.

### Layout Overview

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ [logo][ title ]  [ topCard: # Hotels | Avg Price | Avg Rating | Total Reviews      ] │
│                                                                                      │
│ [brandSlicer ]  [  priceTrendsChart (line chart)          ]  [                    ] │
│ [ratingSlicer]  [  Average Price by Date × Brand          ]  [   hotelTable       ] │
│ [hotelSlicer ]  [  scatterChart (bubble chart)            ]  [                    ] │
│              ]  [  Price vs Distance × Rating             ]  [                    ] │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

### Visual Configurations — Page 2

#### `logo`
No changes needed. Leave as-is.

#### `title`
Pre-filled with "Atlanta | FabCon Planning". No changes needed.

#### `topCard` — 4 KPI Cards
Add the 4 hotel measures to `query.queryState.Data.projections`:

```json
"query": {
  "queryState": {
    "Data": {
      "projections": [
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Number of Hotels"
            }
          },
          "queryRef": "Hotels.Number of Hotels",
          "nativeQueryRef": "Number of Hotels"
        },
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Average Price"
            }
          },
          "queryRef": "Hotels.Average Price",
          "nativeQueryRef": "Average Price"
        },
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Average Rating"
            }
          },
          "queryRef": "Hotels.Average Rating",
          "nativeQueryRef": "Average Rating"
        },
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Total Reviews"
            }
          },
          "queryRef": "Hotels.Total Reviews",
          "nativeQueryRef": "Total Reviews"
        }
      ]
    }
  }
}
```

#### `brandSlicer` — Brand Dropdown Slicer
Configure with `Hotels[Brand]` as the Values field.

```json
"query": {
  "queryState": {
    "Values": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Brand"
            }
          },
          "queryRef": "Hotels.Brand",
          "nativeQueryRef": "Brand",
          "active": true
        }
      ]
    }
  }
}
```

#### `ratingSlicer` — Rating Between Slicer (numeric)
Configure with `Hotels[Rating]` as the Values field.

```json
"query": {
  "queryState": {
    "Values": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Rating"
            }
          },
          "queryRef": "Hotels.Rating",
          "nativeQueryRef": "Rating",
          "active": true
        }
      ]
    }
  }
}
```

#### `hotelSlicer` — Hotel Name Checklist Slicer
Configure with `Hotels[Hotel Name]` as the Values field.

```json
"query": {
  "queryState": {
    "Values": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Hotel Name"
            }
          },
          "queryRef": "Hotels.Hotel Name",
          "nativeQueryRef": "Hotel Name",
          "active": true
        }
      ]
    }
  }
}
```

#### `priceTrendsChart` — Average Price Trends Line Chart
Configure with Date on Category, Hotels[Brand] on Series, and [Average Price] on Y.

```json
"query": {
  "queryState": {
    "Category": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Date" } },
              "Property": "Date"
            }
          },
          "queryRef": "Date.Date",
          "nativeQueryRef": "Date",
          "active": true
        }
      ]
    },
    "Series": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Brand"
            }
          },
          "queryRef": "Hotels.Brand",
          "nativeQueryRef": "Brand"
        }
      ]
    },
    "Y": {
      "projections": [
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Average Price"
            }
          },
          "queryRef": "Hotels.Average Price",
          "nativeQueryRef": "Average Price"
        }
      ]
    }
  }
}
```

#### `scatterChart` — Price by Distance to GWCC (Bubble Chart)
Configure X = Distance To GWCC, Y = Average Price, Size = Average Rating, Details = Hotel Name.

```json
"query": {
  "queryState": {
    "X": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Distance To GWCC (mi)"
            }
          },
          "queryRef": "Hotels.Distance To GWCC (mi)",
          "nativeQueryRef": "Distance To GWCC (mi)",
          "active": true
        }
      ]
    },
    "Y": {
      "projections": [
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Average Price"
            }
          },
          "queryRef": "Hotels.Average Price",
          "nativeQueryRef": "Average Price"
        }
      ]
    },
    "Size": {
      "projections": [
        {
          "field": {
            "Measure": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Average Rating"
            }
          },
          "queryRef": "Hotels.Average Rating",
          "nativeQueryRef": "Average Rating"
        }
      ]
    },
    "Details": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Hotel Name"
            }
          },
          "queryRef": "Hotels.Hotel Name",
          "nativeQueryRef": "Hotel Name"
        }
      ]
    }
  }
}
```

#### `hotelTable` — Hotel Detail Table
Configure Values with these columns in order: Image URL, Hotel Name, Address, Distance To GWCC (mi), Number of Reviews, Rating. Set `Image URL` column data category to `ImageUrl` in the semantic model so it renders as an image.

```json
"query": {
  "queryState": {
    "Values": {
      "projections": [
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Image URL"
            }
          },
          "queryRef": "Hotels.Image URL",
          "nativeQueryRef": "Image URL"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Hotel Name"
            }
          },
          "queryRef": "Hotels.Hotel Name",
          "nativeQueryRef": "Hotel Name"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Address"
            }
          },
          "queryRef": "Hotels.Address",
          "nativeQueryRef": "Address"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Distance To GWCC (mi)"
            }
          },
          "queryRef": "Hotels.Distance To GWCC (mi)",
          "nativeQueryRef": "Distance To GWCC (mi)"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Number of Reviews"
            }
          },
          "queryRef": "Hotels.Number of Reviews",
          "nativeQueryRef": "Number of Reviews"
        },
        {
          "field": {
            "Column": {
              "Expression": { "SourceRef": { "Entity": "Hotels" } },
              "Property": "Rating"
            }
          },
          "queryRef": "Hotels.Rating",
          "nativeQueryRef": "Rating"
        }
      ]
    }
  }
}
```
