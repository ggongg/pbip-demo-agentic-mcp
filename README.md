# 🔍 What is Agentic Development?

Agentic development is a new paradigm where developers shift from writing code or using UI applications to guiding intelligent agents. The developer defines the what - the requirements, rules, and intent - and the AI agent handles the how - generating specs, implementing the model, running validations, fixing issues, and iterating toward a working solution.

You can achieve agentic development in Power BI combining the following features:
- Open file formats with [Power BI Project file format (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview) with [TMDL language](https://learn.microsoft.com/analysis-services/tmdl/tmdl-overview) and [Power BI enhanced Report format (PBIR)](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-report?tabs=v2%2Cdesktop).
- MCP Server [powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp)
- Ensure the AI has context information on your Power BI development style. See [.kb/](.kb/) for an example.

>[!IMPORTANT]
>**powerbi-modeling-mcp** is currently in private preview. If you're interested in trying it out, sign up [here](https://forms.office.com/r/0MXYd6uzwE).

▶️End-to-End demo: [here](https://www.youtube.com/watch?v=7fNDTvLbN2A)

See the branch [final](https://github.com/RuiRomano/pbip-demo-agentic-mcp/tree/final) to inspect the final output.

# ⚙️ How does it work?

1. Create a high-level requirement doc, similar to [requirements.md](.input/requirements.md).
2. Prompt the AI agent to create a new semantic model — but generate a spec before implementation. See [prompt example](.input/prompt.md). Give it context about your Power BI development style.
3. AI creates a development spec – readable, reviewable, and adjustable.
4. You review the spec, and when ready, just say **GO** and the agent starts implementing autonomously.
5. You can ground the development with Best Practice Analysis (see [.bpa/](.bpa/)). The agent autonomously fixes issues and iterates until there are no errors.
6. You open the Power BI application and review the result, just like you would with a teammate's pull request.

>[!IMPORTANT]
>It's very important to provide AI knowledge/context about your Power BI development style and Power BI concepts such as PBIP structure. See [.kb/ folder](.kb/).

# 📊 Demo Report — Atlanta FabCon Planning

This demo builds the **Atlanta | FabCon Planning** report using hotel and attraction data from `Atlanta_Attractions_Data.xlsx`. The report has two pages:

### Page 1 — Atlanta | FabCon Planning
An attraction exploration and itinerary planning page for conference attendees.

| Visual | Description |
|--------|-------------|
| Accommodation card | Shows the selected attendee name, hotel, and last saved timestamp |
| Featured attraction cards | Highlights Atlanta Botanical Garden, Centennial Olympic Park, and Chick-fil-A College Football Hall of Fame with ratings, addresses, and descriptions |
| Category slicer | Tile-button slicer to filter by attraction category (Attraction, Museum, Park) |
| Attraction Count by Neighborhood | Horizontal bar chart showing attraction distribution across neighborhoods |
| Top Attraction card | Spotlights the highest-rated attraction with image and rating |
| Itinerary map | Azure Maps visual showing hotel and attraction markers with a route line (Grayscale Light style) |
| Create Itinerary | Image grid of selectable attractions with Name input, Selected Hotel input, and Submit button |
| Submitted Itineraries table | Log of all submitted itineraries: CreatedUTC, Attendee, Hotel, Attraction 1, Attraction 2 |

### Page 2 — Hotel Analysis
A hotel portfolio analysis page for conference planners and budget owners.

| Visual | Description |
|--------|-------------|
| KPI cards | Number of Hotels, Average Price, Average Rating, Total Reviews |
| Brand slicer | Dropdown to filter by hotel brand |
| Rating slicer | Numeric between-slicer to filter by rating range |
| Hotel slicer | Checklist to filter to specific hotels |
| Average Price Trends | Line chart showing average nightly price by date, broken out by brand |
| Price by Distance to GWCC | Scatter/bubble chart: X = distance, Y = price, bubble size = rating |
| Hotel detail table | Image, name, address, distance to GWCC, number of reviews, rating |

# 📁 Repository Structure

```
.bpa/                          # Best Practice Analysis rules and script
  bpa-rules-report.json
  bpa-rules-semanticmodel.json
  bpa.ps1

.input/                        # Inputs for the agentic workflow
  requirements.md              # Business requirements (Part B — Scenarios 5–8)
  prompt.md                    # Prompt to kick off the AI agent
  Atlanta_Attractions_Data.xlsx

.kb/                           # Knowledge base — AI context files
  powerbi-modeling-kb.md       # Semantic model rules and best practices
  powerbi-pbip-kb.md           # PBIP implementation phases and file structure
  report-layout-page1.png      # Reference layout for Page 1
  report-layout-page2.png      # Reference layout for Page 2
  templateReport/              # PBIR report template (2 pages, all visuals pre-configured)
    template-report-kb.md      # Guide for adapting template visuals to the semantic model
    definition/
      pages/
        mainPage/              # Page 1: Atlanta | FabCon Planning (12 visuals)
        hotelPage/             # Page 2: Hotel Analysis (9 visuals)
```

# 🧪 Try it yourself

1. Clone this repo to your laptop.
2. Install [Visual Studio Code](https://code.visualstudio.com/).
3. Install [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot), [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) and [Power BI Modeling MCP Server](https://marketplace.visualstudio.com/items?itemName=analysis-services.powerbi-modeling-mcp).
4. Open GitHub Copilot Chat in [Agent mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode).
5. Open the starter file "Atlanta_FabCon_Report_Theme_Starter.pbix" 
6. Ask in the chat " Connect to my Power BI Desktop file "Atlanta_FabCon_Report_Theme_Starter.pbix" 
7. Copy and paste the [prompt](.input/prompt.md) into the chat.
8. Review the generated development spec, then prompt Copilot to proceed with implementation.
9. Open the generated .pbip file in Power BI Desktop to view the created report 
