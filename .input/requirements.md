
# Business Requirements 💼

## Part B — Atlanta FabCon Planning

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
| **ATL-ATTRACT-004** | Map-Based Itinerary | As an **Attendee**, I want to see attractions on a map and visualize a simple itinerary so that I can plan my day. | Add an **Azure Maps** visual in the center of the *Atlanta FabCon Planning* page. Use Address as the Location |
| **ATL-ATTRACT-005** | Attraction Image Gallery | As an **Attendee**, I want a visual gallery of attractions where clicking an image filters the rest of the page, so I can quickly compare options. | Create a **3×2 button slicer** of key attractions. Buttons should be the first Image for each Atlanta Attraction. Selecting an image filters the map and KPI cards for that attraction. |

### Data Source Information 🛢️

- **Data**: `Atlanta_Attractions_Data.xlsx`
- This file contains the hotel and attraction data used for the **Atlanta | FabCon Planning** report.
- Expected tables/sheets: `Hotels`, `Attractions`
- Key columns:
  - **Hotels**: Hotel Name, Address, Image URL, Distance to GWCC (mi), Price, Rating, Number of reviews, Neighborhood, Review rating, Brand
  - **Attractions**: Attraction Name, Category, Address, Neighborhood, Rating, Admission Price, Notes, Image URL, Category Icon, Number of Reviews