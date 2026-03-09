
# Detailed Tasks
- Create a new Power Bi project (PBIP) for the Atlanta FabCon Planning report, following the requirements in requirements.md. 
- Strictly follow the development guidance in .kb/powerbi-pbip-kb.md ( Be sure to review any referenced KB documents included in this document). 
- Dont implement the project directly! Start with a good technical development spec document with name dev-spec.md for my review. 
- All source code and created documents should go to the src/ folder. Do not work outside of that folder. 

## Atlanta FabCon Planning Report
- Use the **Atlanta_Attractions_Data.xlsx** dataset from the `.input/` folder and any existing semantic model built on top of it.
- Create or update the Power BI Project (PBIP): Build out the **Atlanta | FabCon Planning** report so that it satisfies the requirements in `requirements.md` (Part B).
- **Do not bypass the spec step:** for each reporting scenario, update or extend `dev-spec.md`. The spec should:
  - Summarize current model state.
  - List proposed tables, relationships, measures, and DAX.
  - Map each proposal to a requirement ID from `requirements.md`.

## Report Pages

The final report must have **2 pages** in this order:

### Page 1 — "Hotels"

Purpose: Hotel portfolio analysis for conference planners and budget owners.

Implement visuals as per requirements.md


### Page 2 — "Attractions"

Purpose: Attraction exploration for conference attendees.

Implement visuals as per requirements.md

## General Rules

- **All source files and created documents must stay under `src/` folder.**  Do not create content or work outside that folder.
- Always respect the knowledge base files in `.kb/`:
  - `.kb/powerbi-modeling-kb.md`
  - `.kb/powerbi-pbip-kb.md`
  - `.kb/powerbi-pbir-schemas-kb.md`
- Before making changes, explain your plan in `dev-spec.md` so I can review it.

## PBIR Schema Safety (Important)

- Keep every visual JSON valid against its schema (see `.kb/powerbi-pbir-schemas-kb.md` for current versions).
- Do **not** add `visual.filters` in visual files.
- If a filter is needed, use `filterConfig.filters` with a valid schema-compliant `filter.Where` payload.
- Do **not** add custom `TopN` objects under `filterConfig.filters[*].filter` unless the exact schema supports it.
- For top-attraction behavior, prefer model/DAX-driven logic (for example a rank measure + filterable field), not custom JSON shapes.
- After PBIR edits, validate by reopening the PBIP in Power BI Desktop before continuing further changes.
