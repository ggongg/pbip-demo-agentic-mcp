# powerbi-pbip-kb

This file defines the **implementation guidelines** an agent must follow when creating a new Power BI Project.

---

## High-level implementation phases 

Ensure you respect the following phases when implementing the Power BI Project.

### Phase 1: Prepare environment
1. **Critical**: Make sure **powerbi-modeling-mcp** is available and is used for model development. If not, stop and prompt the user to install it. Learn about the **powerbi-modeling-mcp** tools and always prioritize using Batch tools. For example, when creating objects always try to create them in batches to minimize round-trips.
2. Create the Power BI Project (PBIP) folder structure as explained in [PBIP file structure](#pbip-file-structure)
3. Because its PBIP development, there is no Analysis Services server. Thereforce: cannot run DAX queries to test data; cannot run refresh commands; its ok to have partitions in `NoData` state.

### Phase 2: Semantic Model implementation
1. Understand the Data Source schema: tables, columns, relationships.
2. Create a new semantic model, avoid creating tables and columns that are not necessary for the business requirements.
3. **Critical:** Ensure you follow the semantic model development rules and best practices in `powerbi-modeling-kb.md`.

### Phase 3: Report implementation

**Critical:** Read `powerbi-pbir-schemas-kb.md` before starting report work. It contains the full PBIR file structure, schema inventory, schema relationships, and common JSON patterns you will need.

1. Create the PBIR report definition files inside the `*.Report/definition/` folder following the folder hierarchy documented in `powerbi-pbir-schemas-kb.md` (see "PBIR Report File Structure").
2. Every JSON file **must** include a `$schema` property — see `powerbi-pbir-schemas-kb.md` for the exact URL per schema type.
3. Use the JSON schema files under `.kb/schemas/` as the definitive reference for all valid properties, types, and enums.
4. Use exact case-sensitive entity and property names from the semantic model when defining visual queries and filters.
5. Refer to the "Common Patterns for Agents" section in `powerbi-pbir-schemas-kb.md` for column/measure references, filters, and positioning.

## PBIP file structure

- When creating a new semantic model, ensure you start by creating a PBIP folder structure like the following:

    ```text
    root/
    ├── [Name].SemanticModel/
    |   ├── /definition # Empty folder that will hold the TMDL definition of the semantic model
    |   ├──  definition.pbism # The semantic model definition file
    ├── [Name].Report/        
    |   ├── /definition # Empty folder that will hold the PBIR definition of the report
    |   ├──  definition.pbir # The report definition file with a byPath relative reference to the semantic model folder
    └── [Name].pbip # A shortcut file to the report folder
    ```    

    **Example of a definition.pbism file**

    No modifications are needed—just create the file exactly as shown in the example.

    ```json
    {
        "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/semanticModel/definitionProperties/1.0.0/schema.json",
        "version": "4.2",
        "settings": {
            "qnaEnabled": true
        }
    }
    ```

    **Example of a definition.pbir file**

    The `byPath` property should reference the semantic model folder using a relative path like below.

    ```json
    {
        "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definitionProperties/2.0.0/schema.json",
        "version": "4.0",
        "datasetReference": {
            "byPath": {
                "path": "../{Name of the Semantic Model}.SemanticModel"
            }
        }
    }
    ```

     **Example of a {Name of the Semantic Model}.pbip file**

     ```json
        {
            "$schema": "https://developer.microsoft.com/json-schemas/fabric/pbip/pbipProperties/1.0.0/schema.json",
            "version": "1.0",
            "artifacts": [
                {
                "report": {
                    "path": "{Name of the Semantic Model}.Report"
                }
                }
            ],
            "settings": {
                "enableAutoRecovery": true
            }
        }
     ```


