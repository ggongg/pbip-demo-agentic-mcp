# 🔍 What is Agentic Development?

Agentic development is a new paradigm where developers shift from writing code or using UI applications to guiding intelligent agents. The developer defines the what — the requirements, rules, and intent — and the AI agent handles the how — generating specs, implementing the model, running validations, fixing issues, and iterating toward a working solution.

You can achieve agentic development in Power BI combining the following features:
- Open file formats with [Power BI Project file format (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview) with [TMDL language](https://learn.microsoft.com/analysis-services/tmdl/tmdl-overview) and [Power BI enhanced Report format (PBIR)](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-report?tabs=v2%2Cdesktop).
- MCP Server [powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp)
- AI context files that encode your Power BI development style and schema rules. See [.kb/](.kb/) for an example.

>[!IMPORTANT]
>**powerbi-modeling-mcp** is currently in public preview. If you're interested in trying it out, check it out [here](https://github.com/microsoft/powerbi-modeling-mcp).

# ⚙️ How does it work?

The workflow has three phases, each guided by knowledge base files in `.kb/`:

### Phase 1 — Prepare environment
1. Open the starter `.pbix` file in Power BI Desktop.
2. Connect the AI agent (via GitHub Copilot Agent mode) to the running PBI Desktop instance using `powerbi-modeling-mcp`.
3. The agent explores the existing semantic model (tables, columns, relationships, measures).

### Phase 2 — Spec & review
1. Provide the agent with business requirements (`.input/requirements.md`) and a prompt (`.input/prompt.md`).
2. The agent generates a development spec (`dev-spec.md`) mapping every requirement to proposed tables, measures, DAX, and visuals.
3. You review the spec and say **"Proceed"** when ready.

### Phase 3 — Autonomous implementation
1. The agent creates the full PBIP folder structure under `src/` — semantic model (TMDL) + report definition (PBIR).
2. It creates measures via the MCP server against the live PBI Desktop model, then exports TMDL to disk.
3. It builds all report pages and visuals as individual JSON files following the PBIR schema rules in `.kb/powerbi-pbir-schemas-kb.md`.
4. You open the generated `.pbip` file in Power BI Desktop to review the result.

>[!IMPORTANT]
>It's very important to provide AI knowledge/context about your Power BI development style and Power BI concepts. The `.kb/` folder contains schema references, pitfall documentation, and implementation guidelines that prevent common errors.

# 📊 Demo Report — Atlanta FabCon Planning

This demo builds the **Atlanta | FabCon Planning** report using hotel and attraction data from a Fabric SQL endpoint. The report has two pages:

### Page 1 — Hotels

Hotel portfolio analysis for conference planners and budget owners.

| Visual | Type | Description |
|--------|------|-------------|
| Number of Hotels | Card | Count of distinct hotels near GWCC |
| Average Price | Card | Average daily hotel price across all dates/brands |
| Average Rating | Card | Average guest rating |
| Total Reviews | Card | Sum of all guest reviews |
| Brand slicer | Dropdown slicer | Filter by hotel brand |
| Rating slicer | Numeric between-slicer | Filter by rating range |
| Hotel Name slicer | Checklist slicer | Filter to specific hotels |
| Price Trends by Brand | Line chart | Average price by date, series = Brand |
| Price vs Distance | Scatter/bubble chart | X = distance to GWCC, Y = avg price, bubble = avg rating |
| Hotel Details | Table | Image, name, address, distance, reviews, rating |

### Page 2 — Attractions

Attraction exploration for conference attendees.

| Visual | Type | Description |
|--------|------|-------------|
| Attraction Count | Card | Count of attractions |
| Average Admission Price | Card | Average admission cost |
| Average Attraction Rating | Card | Average attraction rating |
| Total Attraction Reviews | Card | Sum of attraction reviews |
| Category slicer | Dropdown slicer | Filter by attraction category |
| Top Attraction | Card | Highest-rated attraction name |
| Attractions by Neighborhood | Bar chart | Attraction count by neighborhood |
| Attraction Locations | Azure Maps | Map of attraction addresses |
| Attraction Image Gallery | Slicer (list) | Image-based slicer for filtering |

# 📁 Repository Structure

```
.bpa/                              # Best Practice Analysis rules and script
  bpa-rules-report.json
  bpa-rules-semanticmodel.json
  bpa.ps1

.input/                            # Inputs for the agentic workflow
  requirements.md                  # Business requirements (ATL-HOTELS-*, ATL-ATTRACT-*)
  prompt.md                        # Prompt to kick off the AI agent
  Atlanta_Attractions_Data.xlsx    # Source data

.kb/                               # Knowledge base — AI context files
  powerbi-modeling-kb.md           # Semantic model rules and best practices
  powerbi-pbip-kb.md               # PBIP implementation phases and file structure
  powerbi-pbir-schemas-kb.md       # PBIR schema reference, patterns, and pitfall rules
  schemas/                         # JSON schema files for all PBIR definition types
    report/                        #   report.json schema (v3.1.0)
    page/                          #   page.json schema (v2.0.0)
    pagesMetadata/                 #   pages.json schema (v1.0.0)
    visualContainer/               #   visual.json schema (v2.6.0)
    visualConfiguration/           #   embedded visual config schema (v2.2.0)
    filterConfiguration/           #   filter config schema (v1.2.0)
    semanticQuery/                 #   query/expression definitions (v1.3.0)
    formattingObjectDefinitions/   #   formatting object definitions (v1.4.0)
    versionMetadata/               #   version.json schema (v1.0.0)
    bookmark/                      #   bookmark schema (v2.0.0)
    bookmarksMetadata/             #   bookmarks metadata schema (v1.0.0)
    reportExtension/               #   report extension schema (v1.0.0)
  report-layout-page1.png          # Reference layout for Page 1
  report-layout-page2.png          # Reference layout for Page 2

Atlanta_FabCon_Report_Theme_Starter.pbix  # Starter file with data source connections

src/                               # Generated output (created by the agent)
  Atlanta_FabCon_Planning.pbip     # PBIP entry point — open this in PBI Desktop
  Atlanta_FabCon_Planning.Report/
    definition.pbir                # Report ↔ semantic model binding
    definition/
      version.json                 # Report definition version (must be "2.0.0")
      report.json                  # Report-level config: themes, settings, resource packages
      pages/
        pages.json                 # Page order and active page
        Hotels/                    # Page 1
          page.json
          visuals/                 # 10 visual folders, each with visual.json
        Attractions/               # Page 2
          page.json
          visuals/                 # 9 visual folders, each with visual.json
  Atlanta_FabCon_Planning.SemanticModel/
    definition.pbism               # Semantic model definition properties
    definition/
      model.tmdl                   # Model metadata and table references
      database.tmdl                # Database compatibility settings
      relationships.tmdl           # Table relationships
      tables/                      # One .tmdl file per table (6 tables)
      cultures/                    # Locale settings (en-US)
  dev-spec.md                      # Technical development spec (agent-generated)
```

# 🧪 Try it yourself

1. Clone this repo.
2. Install [Visual Studio Code](https://code.visualstudio.com/).
3. Install [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot), [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat), and [Power BI Modeling MCP Server](https://marketplace.visualstudio.com/items?itemName=analysis-services.powerbi-modeling-mcp).
4. Open `Atlanta_FabCon_Report_Theme_Starter.pbix` in Power BI Desktop.
5. Open GitHub Copilot Chat in [Agent mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode).
6. Ask: *"Connect to my Power BI Desktop file Atlanta_FabCon_Report_Theme_Starter.pbix"*
7. Paste the contents of [.input/prompt.md](.input/prompt.md) into the chat.
8. Review the generated `dev-spec.md`, then tell the agent to proceed.
9. Open the generated `src/Atlanta_FabCon_Planning.pbip` in Power BI Desktop to view the report.

>[!NOTE]
>The generated `src/` folder is not committed to `main`. It is created by the agent during execution.
