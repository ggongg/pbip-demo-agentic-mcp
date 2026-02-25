
# Detailed Tasks

You are working in a PBIP repository for the FABCON demo.

## Phase 2 — Atlanta FabCon Planning Report (Scenarios 5–8)
- Use the **Atlanta_Attractions_Data.xlsx** dataset from the `.input/` folder and any existing semantic model built on top of it.
- Create or update the Power BI Project (PBIP): Build out the **Atlanta | FabCon Planning** report so that it satisfies the requirements in `requirements.md` (Part B) and the report layout guidelines in `.kb/templateReport/template-report-kb.md`.
- **Do not bypass the spec step:** for each reporting scenario (5–8), update or extend `dev-spec.md`. The spec should:
  - Summarize current model state.
  - List proposed tables, relationships, measures, and DAX.
  - Map each proposal to a requirement ID from `requirements.md`.

## Report Pages

The final report must have **2 pages** in this order:

### Page 1 — "Atlanta | FabCon Planning"

Purpose: Attraction exploration and itinerary planning for conference attendees.

Visuals to implement (refer to `.kb/templateReport/template-report-kb.md` for layout positions and JSON configuration):

| Visual | Type | Fields |
|--------|------|--------|
| `logo` | Image | Report logo (top-left) |
| `title` | Text box | "Atlanta \| FabCon Planning" |
| `accommodationCard` | Card | Itineraries[Attendee], Itineraries[Hotel], Itineraries[CreatedUTC] |
| `featuredCard1` | Card | Atlanta Botanical Garden — Name, Rating, Address, Description |
| `featuredCard2` | Card | Centennial Olympic Park — Name, Rating, Address, Description |
| `featuredCard3` | Card | Chick-fil-A College Football Hall of Fame — Name, Rating, Address, Description |
| `categorySlicer` | Slicer (Tile) | Attractions[Category] |
| `barChart` | Horizontal bar chart | X: Attractions[Neighborhood], Y: [Attraction Count] |
| `topAttractionCard` | Card | Top-rated Attraction Name, Image URL, Rating |
| `mapVisual` | Azure Maps | Hotels Lat/Long + Attractions Lat/Long; route line from hotel to selected attractions |
| `itineraryGrid` | Slicer (Tile/Image) | Attractions[Attraction Name] with Attractions[Image URL] |
| `submittedItinerariesTable` | Table | Itineraries[CreatedUTC], [Attendee], [Hotel], [Attraction 1], [Attraction 2] |

### Page 2 — "Hotel Analysis"

Purpose: Hotel portfolio analysis for conference planners and budget owners.

Visuals to implement (refer to `.kb/templateReport/template-report-kb.md` for layout positions and JSON configuration):

| Visual | Type | Fields |
|--------|------|--------|
| `logo` | Image | Report logo (top-left) |
| `title` | Text box | "Atlanta \| FabCon Planning" |
| `topCard` | Card (4-up) | [Number of Hotels], [Average Price], [Average Rating], [Total Reviews] |
| `brandSlicer` | Slicer (Dropdown) | Hotels[Brand] |
| `ratingSlicer` | Slicer (Between, numeric) | Hotels[Rating] |
| `hotelSlicer` | Slicer (List/Checklist) | Hotels[Hotel Name] |
| `priceTrendsChart` | Line chart | X: Date[Date], Legend: Hotels[Brand], Y: [Average Price] |
| `scatterChart` | Scatter/Bubble chart | X: Hotels[Distance To GWCC (mi)], Y: [Average Price], Size: [Average Rating], Details: Hotels[Hotel Name] |
| `hotelTable` | Table | Hotels[Image URL], [Hotel Name], [Address], [Distance To GWCC (mi)], [Number of Reviews], [Rating] |

## General Rules

- **All source files and created documents must stay under `src/` folder.**  Do not create content or work outside that folder.
- Always respect the knowledge base files in `.kb/`:
  - `.kb/powerbi-modeling-kb.md`
  - `.kb/powerbi-pbip-kb.md`
  - `.kb/templateReport/template-report-kb.md`
  - Theme files and layout PNGs in `.kb/templateReport/`
- Before making changes, explain your plan in `dev-spec.md` so I can review it.
