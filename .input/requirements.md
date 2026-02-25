
# Business Requirements 💼

## Part B — Atlanta FabCon Planning (Scenarios 5–8)

These requirements describe the **Atlanta | FabCon Planning** report built on the Atlanta hotels & attractions data.

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **ATL-HOTELS-001** | Hotel Portfolio Overview | As a **Conference Planner**, I want to see how many hotels we have near the convention center and the overall price/rating profile so that I can quickly assess our lodging options. | Create KPI measures for **[Hotel Count]**, **[Average Hotel Price]**, **[Average Hotel Rating]**, and **[Total Hotel Reviews]`. Display them as cards at the top of the *Hotels* page. |
| **ATL-HOTELS-002** | Brand & Rating Filtering | As a **Planner**, I want to filter hotels by brand and minimum rating so that I can focus on options that meet our quality standards. | Add slicers for **Brand** and **Rating** that filter all visuals on the *Hotels* page, including charts and the hotel table. Rating should support a numeric slider to set a minimum rating threshold. |
| **ATL-HOTELS-003** | Price Trends by Brand | As a **Budget Owner**, I want to understand price trends by hotel brand leading up to the conference dates so I can negotiate and budget effectively. | Create a line chart showing **average nightly price by date (or stay index)** with **brand** as the series. Ensure it uses a measure like `[Average Hotel Price]` and respects brand, rating, and date filters. |
| **ATL-HOTELS-004** | Price vs Distance Trade-off | As a **Planner**, I want to understand how hotel price varies by distance to the conference venue so I can choose the right balance of cost and convenience. | Create a bubble/scatter chart with **Distance to GWCC** on the X-axis, **Average Price** on the Y-axis, and **Rating** as bubble size or color. Each point should represent a hotel. |
| **ATL-HOTELS-005** | Detailed Hotel Comparison | As a **Planner**, I want a detailed list of hotels with pictures, addresses, distance, ratings, and reviews so that I can share options with stakeholders. | Build a table visual that includes: Image/thumbnail, Hotel Name, Address, Distance to GWCC, Number of Reviews, and Rating. Clicking a row should cross-filter other visuals on the page. |

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **ATL-ATTRACT-001** | Attractions Portfolio Overview | As a **Conference Attendee**, I want to see the key attractions near the venue, including typical admission price and rating, so I can plan my free time. | Create KPI measures for **[Attraction Count]**, **[Average Admission Price]**, **[Average Attraction Rating]**, and **[Total Attraction Reviews]`. Display them as cards at the top of the *Attractions & Itinerary* page. |
| **ATL-ATTRACT-002** | Category & Neighborhood Exploration | As an **Attendee**, I want to explore attractions by category (Attraction, Museum, Park, etc.) and neighborhood so I can find experiences that match my interests. | Add a category slicer (with icons if possible) and a bar chart showing **Attraction Count by Neighborhood**. Both should slice the rest of the visuals on the page. |
| **ATL-ATTRACT-003** | Top Attraction Spotlight | As a **Marketing Lead**, I want to highlight a single top attraction with image and rating to feature in conference materials. | Create a “Top Attraction” card that shows the highest-rated attraction (or one flagged as “Top”) with image, name, and rating. |
| **ATL-ATTRACT-004** | Map-Based Itinerary | As an **Attendee**, I want to see attractions on a map and visualize a simple itinerary so that I can plan my day. | Add a map visual that plots attractions (and optionally the selected hotel). When multiple attractions are selected, approximate an itinerary route or ordering. Map styling should align with the FabCon theme. |
| **ATL-ATTRACT-005** | Attraction Image Gallery | As an **Attendee**, I want a visual gallery of attractions where clicking an image filters the rest of the page, so I can quickly compare options. | Create a grid of image cards for key attractions. Selecting an image filters the map and KPI cards for that attraction. |

### Data Source Information 🛢️

- **Data (Scenarios 5–8)**: `Atlanta_Attractions_Data.xlsx`  
- This file contains the hotel and attraction data used for the **Atlanta | FabCon Planning** report.
``
