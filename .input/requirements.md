
# Business Requirements 💼

## Part B — Atlanta FabCon Planning (Scenarios 5–8)

These requirements describe the **Atlanta | FabCon Planning** report built on the Atlanta hotels & attractions data.

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **ATL-HOTELS-001** | Hotel Portfolio Overview | As a **Conference Planner**, I want to see how many hotels we have near the convention center and the overall price/rating profile so that I can quickly assess our lodging options. | Create KPI measures for **[Number of Hotels]**, **[Average Price]**, **[Average Rating]**, and **[Total Reviews]**. Display them as cards at the top of the *Hotel Analysis* page. |
| **ATL-HOTELS-002** | Brand & Rating Filtering | As a **Planner**, I want to filter hotels by brand and minimum rating so that I can focus on options that meet our quality standards. | Add a **Brand** dropdown slicer and a **Rating** numeric between-slicer on the left sidebar of the *Hotel Analysis* page. Both should filter all visuals on the page. |
| **ATL-HOTELS-003** | Price Trends by Brand | As a **Budget Owner**, I want to understand price trends by hotel brand leading up to the conference dates so I can negotiate and budget effectively. | Create a line chart showing **Average Price by Date** with **Brand** as the series legend. Use the `[Average Price]` measure. Ensure it respects brand, rating, and hotel filters. |
| **ATL-HOTELS-004** | Price vs Distance Trade-off | As a **Planner**, I want to understand how hotel price varies by distance to the conference venue so I can choose the right balance of cost and convenience. | Create a scatter/bubble chart with **Distance To GWCC (mi)** on the X-axis, **Average Price** on the Y-axis, and **Average Rating** as bubble size. Each point represents a hotel. |
| **ATL-HOTELS-005** | Detailed Hotel Comparison | As a **Planner**, I want a detailed list of hotels with pictures, addresses, distance, ratings, and reviews so that I can share options with stakeholders. | Build a table visual with columns: Image URL (rendered as image), Hotel Name, Address, Distance To GWCC (mi), Number of Reviews, Rating. Sort by Rating descending. |
| **ATL-HOTELS-006** | Hotel Filter | As a **Planner**, I want to filter to one or more specific hotels so that I can compare a shortlist. | Add a **Hotel Name** checklist slicer on the left sidebar of the *Hotel Analysis* page. |

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **ATL-ATTRACT-001** | Attractions Portfolio Overview | As a **Conference Attendee**, I want to see the key attractions near the venue, including typical admission price and rating, so I can plan my free time. | Create KPI measures for **[Attraction Count]**, **[Average Admission Price]**, **[Average Attraction Rating]**, and **[Total Attraction Reviews]**. Display them as cards at the top of the *Attractions & Itinerary* page. |
| **ATL-ATTRACT-002** | Category & Neighborhood Exploration | As an **Attendee**, I want to explore attractions by category (Attraction, Museum, Park, etc.) and neighborhood so I can find experiences that match my interests. | Add a **Category** tile-button slicer and a bar chart showing **Attraction Count by Neighborhood** on the left sidebar of the *Atlanta FabCon Planning* page. Both should filter all visuals on the page. |
| **ATL-ATTRACT-003** | Top Attraction Spotlight | As a **Marketing Lead**, I want to highlight the single top-rated attraction with image and rating to feature in conference materials. | Create a **Top Attraction** card on the left sidebar showing the highest-rated attraction with its image, name, and rating. |
| **ATL-ATTRACT-004** | Map-Based Itinerary | As an **Attendee**, I want to see attractions on a map and visualize a simple itinerary so that I can plan my day. | Add an **Azure Maps** visual in the center of the *Atlanta FabCon Planning* page. Plot the selected hotel and selected attractions as markers. Connect hotel to attractions with a route line. Use Grayscale (Light) map style. |
| **ATL-ATTRACT-005** | Attraction Image Gallery | As an **Attendee**, I want a visual gallery of attractions where clicking an image filters the rest of the page, so I can quickly compare options. | Create a **2×2 image grid** of key attractions in the Create Itinerary section. Selecting an image filters the map and KPI cards for that attraction. |

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **ATL-ITIN-001** | Accommodation Selection | As a **Conference Attendee**, I want to see which hotel I have selected alongside my name, so my itinerary is personalized. | Display an **Accommodation card** at the top-center of the *Atlanta FabCon Planning* page showing the attendee name (from Itineraries), the label "ACCOMMODATION", the selected hotel name, and a "Last saved" timestamp. |
| **ATL-ITIN-002** | Featured Attraction Highlights | As a **Conference Attendee**, I want to see a curated row of key attractions with images, ratings, addresses, and descriptions so I can quickly preview top options. | Display **3 Featured Attraction cards** across the top of the *Atlanta FabCon Planning* page for: **Atlanta Botanical Garden**, **Centennial Olympic Park**, and **Chick-fil-A College Football Hall of Fame**. Each card shows: name, rating, address, and short description. |
| **ATL-ITIN-003** | Create Itinerary | As a **Conference Attendee**, I want to select up to 2 attractions and a hotel to build a personal itinerary, so I can plan my FabCon day trip. | Build a **Create Itinerary** section on the right side of the *Atlanta FabCon Planning* page. Show a 2×2 image grid of selectable attractions (Atlanta Botanical Garden, Centennial Olympic Park, Chick-fil-A College Football Hall of Fame, Georgia Aquarium). Include a **Name** text input, a **Selected Hotel** dropdown input, and a **Submit** button. On submit, save the entry to the Itineraries table. |
| **ATL-ITIN-004** | Submitted Itineraries Log | As a **Conference Organizer**, I want to see all submitted itineraries in a table so I can track attendee plans and hotel demand. | Display a **Submitted Itineraries table** below the Create Itinerary section. Columns: CreatedUTC, Attendee, Hotel, Attraction 1, Attraction 2. Sort by CreatedUTC descending. |

### Data Source Information 🛢️

- **Data (Scenarios 5–8)**: `Atlanta_Attractions_Data.xlsx`
- This file contains the hotel and attraction data used for the **Atlanta | FabCon Planning** report.
- Expected tables/sheets: `Hotels`, `Attractions`, `Itineraries`
- Key columns:
  - **Hotels**: Hotel Name, Brand, Address, Distance To GWCC (mi), Price, Rating, Number of Reviews, Image URL, Latitude, Longitude
  - **Attractions**: Attraction Name, Category, Neighborhood, Rating, Address, Description, Image URL, Latitude, Longitude, Admission Price
  - **Itineraries**: CreatedUTC, Attendee, Hotel, Attraction 1, Attraction 2
