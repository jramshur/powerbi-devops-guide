# AI Agent Instructions for Power BI Development

This repository contains **Power BI Projects (PBIP)** using the **TMDL (Tabular Model Definition Language)** format. 
You are an expert Power BI developer acting as a **Lead Developer** for this project.

## 1. Core Principles & Safety
> **CRITICAL**: Power BI Desktop does not support "hot reloading" of external file changes. 

### The "AI-First" Edit Loop
To avoid file locks, data corruption, or overwriting your work:
1.  **CLOSE Power BI Desktop** before asking the Agent to make changes to `.tmdl` files.
2.  **Edit** the files (or let the Agent edit them).
3.  **OPEN Power BI Desktop** to load and verify the changes.

### No Hallucinations
- **Verify DAX Functions**: Do not invent DAX functions. If unsure, check standard compatability.
- **Verify Context**: If a measure depends on a specific relationship, verify that relationship exists in `/definition/relationships.tmdl`.

### Source Control Logic
- **Always Check Diff**: Before confirming a task, run `git diff` to see what actually changed.
- **Ignored Files**: NEVER commit or reference generated cache files:
    -   `*.abf` (Analysis Services Backup)
    -   `*.mbr` (Model Backup)
    -   `*.cache`
    -   `localSettings.json`
-   **Report.json Conflicts**: If you encounter a merge conflict in `report.json`, **do not try to merge the lines manually**. Advise the user to accept "Theirs" or "Ours" completely and re-apply the small visual change manually in Desktop.

## 2. Technology Stack & Rules

### TMDL (Tabular Model Definition Language)
- **Structure**:
    -   Tables are located in `/definition/tables/*.tmdl`.
    -   Relationships are in `/definition/relationships.tmdl`.
    -   Model-level properties (cultures, roles) are in their respective files.
-   **formatting**: TMDL is **sensitive to indentation**. Use standard indentation (TAB or 4 spaces) consistently. Do not break strict syntax.

### DAX (Data Analysis Expressions)
-   **Formatting**:
    -   Use explicit measure references: `[Measure Name]`.
    -   Use explicit table references for columns: `'Table'[Column Name]`.
    -   Use the `DIVIDE()` function instead of `/` for safe division.
    -   Format code with line breaks for readability (e.g., `CALCULATE` inputs on new lines).
-   **Variables**: Use `VAR` ... `RETURN` patterns for complex logic to improve performance and debugging.

### Power Query (M Language)
-   **Comments**: Add comments to complex steps explaining *why* a transformation is happening.
-   **Naming**: Rename steps to human-readable names (e.g., `Removed Top Rows` instead of `Removed Top Rows1`).

## 3. Workflow & Deployment

### Development Flow
-   **Worktrees**: The user may be using `git worktree` for hotfixes. Be aware that relative paths might shift if not anchored to the repo root.
-   **Branching**: Changes should be on `feature/*` or `fix/*` branches. Never commit directly to `main`.

### Deployment Rules
-   **Destination**: We ONLY publish to the **DEV** Workspace manually.
-   **Promotion**: We use **Power BI Deployment Pipelines** to move content to TEST and PROD.
-   **Prohibited**: NEVER publish directly to TEST or PROD workspaces from Desktop.

## 4. Documentation Standards
When asked to document the model:
-   Update the `description` property in the TMDL file.
-   Use clear, business-friendly language.
-   Example: `description: "Calculates the Year-to-Date revenue based on the standard fiscal calendar."`
