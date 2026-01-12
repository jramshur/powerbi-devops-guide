# Developer Workflow

This guide covers the daily mechanics of working with Power BI and Git. It answers the question: *"How do I actually make changes safely?"*

## The Three Edit Loops

Modern Power BI development isn't just "Clicking in Desktop" anymore. You have three ways to work.

### Loop A: Desktop-First (Standard)
*Best for: Visuals, Bookmarks, Power Query touches, and interactive design.*

**Concept**: You use the UI as intended.
1.  Open the `.pbip` folder in Power BI Desktop.
2.  Make changes (drag visuals, edit Power Query).
3.  **Save** (Writes from memory -> disk).
4.  **Commit** in Git.

### Loop B: Code-First (VS Code)
*Best for: Bulk measure edits, Search/Replace, copying logic between reports.*

**Workflow**:
1.  **Close Power BI Desktop** (Important!).
2.  Edit `.tmdl` files in VS Code.
3.  Save files.
4.  Open Power BI Desktop. Changes load on startup.

> **Warning**: Power BI Desktop does not "watch" files. If you edit a file while Desktop is open, your changes will be ignored (or overwritten) when you next save in Desktop.

### Loop C: AI-First (Copilot & Agents)
*Best for: "Create 10 measures", "Document my model", generating DAX patterns.*

**Workflow**:
1.  **Close Power BI Desktop**.
2.  In VS Code, ask Copilot: *"Create a measure for YTD Sales in the Sales table"*.
3.  Copilot writes the TMDL code directly to the file.
4.  **Open Power BI Desktop** to verify and test.

---

## Branching Strategies

**Rule #1**: Never work directly on `main`.

### Feature Branching
For every new task (e.g., "Add Sales Forecast"), create a new branch.

```bash
git checkout -b feature/sales-forecast
```

1.  Make your changes in the feature branch.
2.  Commit often.
3.  When done, merge back to `main`.

### The "Worktree" Power Move
**Scenario**: You are working on `feature/huge-update`. The boss demands a quick fix for `main` immediately.

**The Problem**: Switching branches in the same folder causes Power BI Desktop to lock files, creating conflicts or crashes.

**The Solution**: Use **Git Worktrees** to have multiple branches open in different folders simultaneously. (See [Git Concepts](./02-git-concepts.md#3-the-worktree-power-move) for details).

1.  **Create a folder for the hotfix**:
    ```bash
    git worktree add ../hotfix-branch main
    ```
    *This creates a new folder `../hotfix-branch` next to your current repo.*

2.  **Open the second folder** in a *new* instance of Power BI Desktop.
3.  Make your fix, save, and commit in that folder.
4.  **Cleanup**:
    ```bash
    git worktree remove ../hotfix-branch
    ```

**Result**: You worked on two tasks at once without closing your main project or risking file locking usage.
