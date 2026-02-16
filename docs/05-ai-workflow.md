# AI-Assisted Power BI Development

This guide explores the three main ways to use AI in Power BI development, from standard code generation to full-blown agentic control.

## The Three Approaches

### 1. Code-First (VS Code Standard)
Using GitHub Copilot Chat (including `@workspace` agents). This method treats your Power BI project as a collection of text files. It can answer questions about your implementation by reading the code (TMDL/DAX) and can write directly to your files, but it does not interact with the *running* data model.

### 2. Model-Connected (VS Code + MCP)
Using the **Model Context Protocol (MCP)** to give AI direct access to your running model's metadata service. It connects to the Analysis Services engine (via Power BI Desktop) to understand deeper relationships, validate DAX against the schema, and perform architectural changes that text analysis alone might miss.

### 3. Native Copilot
Using the built-in Microsoft Copilot features in Power BI Service and Desktop. Best for quick visuals and business-user questions.

---

## Strategy 1: Code-First (The "Text-Based Agent")

**Tooling**: VS Code + GitHub Copilot (Agnet).
**Best for**: Writing measures, DAX patterns, and documenting code based on file context.

### Core Workflows

### 1. Generating DAX & TMDL
Instead of typing out the full measure definition, use the Chat view or inline chat (`Ctrl+I` / `Cmd+I`) to describe what you want. You can then accept the changes directly into your file.

**Prompt:**
> "Create a measure for 'Year over Year Sales Growth' using the 'Total Sales' measure."

**Output (TMDL):**
```tmdl
measure 'YoY Sales Growth' = 
    DIVIDE(
        [Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date])),
        CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
    )
    formatString: "0.00%"
```

### 2. Bulk Refactoring
You can open multiple TMDL files and ask Copilot to make structural changes.

**Prompt:**
> "Rename 'Client' to 'Customer' in all open files and update all DAX references."

*Note: Always verify the changes in the "Source Control" tab before committing!*

### 3. Documentation & Explanations
Copilot excels at writing human-readable documentation for your data model.

**Prompt:**
> "Add a description to this measure explaining that it excludes returned items."

**Output:**
```tmdl
measure 'Net Sales' = ...
    description: "Calculates total sales minus returns. Used for executive reporting."
```

## Strategy 2: Agentic (VS Code + MCP)

**Tooling**: VS Code + Claude/Copilot + Power BI MCP Server.
**Best for**: Deep model refactoring, semantic understanding, and "conversation with your data model".

### How it differs
Unlike the standard Copilot (which just guesses text), an MCP-enabled agent can:
-   **Analyze the Model**: "List all measures that use the CALCULATE function."
-   **Trace Dependencies**: "Show me what visuals will break if I delete the [Total Sales] measure."
-   **Execute Changes**: "Rename 'Client' to 'Customer' everywhere and update the properties."

*Note: This feature is rapidly evolving. Ensure you have the necessary MCP servers configured.*

## Strategy 3: Native Copilot

**Tooling**: Power BI Desktop / Service (Fabric Capacity required).
**Best for**: Business users, rapid visual generation, and "explain this page".

### Common Use Cases
-   **"Summarize this report page"**: Generates a text narrative of the data.
-   **"Create a page about Sales"**: automatically builds visuals.
-   **DAX Query Generation**: In the DAX query view, ask it to write queries to inspect data.

## The "AI + DevOps" Workflow

It is tempting to trust AI output blindly, but in a professional DevOps environment, AI is just a **proposer**. You are the **approver**.

### The Flow
1.  **AI Generation**: You ask Copilot to draft a complex DAX measure.
2.  **Local Test**: You save the file, switch to Power BI Desktop (which reloads instantly), and drag the measure into a visual to verify numbers.
3.  **Commit & Push**: You push the changes to a feature branch.
4.  **Pull Request**: A human peer reviews the code. *Copilot can be wrong.*
5.  **CI/CD Pipeline**: Your automated tests (e.g., Tabular Editor Best Practice Analyzer) run against the code.

> **Crucial Rule**: Never push AI-generated code to production without local testing. AI can hallucinate DAX functions that don't exist or logic that looks correct but filters data wrongly.

## Best Practices
- **Be Specific**: "Create a measure" is weak. "Create a measure called 'Profit Margin' that divides 'Total Profit' by 'Total Sales', formatted as a percentage" is strong.
- **Context Matters**: Keep relevant files open. Copilot reads your open tabs to understand your data model's schema (table names, relationships).
- **Iterate**: If the output isn't quite right, follow up: *"Make it handle divide-by-zero errors"* or *"Use the KEEPFILTERS function"*.
