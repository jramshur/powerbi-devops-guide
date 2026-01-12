# Power BI + Git: The Cheat Sheet

## Golden Rules
**Need the basics?** Read our [Git Basics Deep Dive](./02-git-concepts.md).

1.  **Never commit cache.** Ensure `.gitignore` blocks `*.abf`, `*.cache`, `*.pbix`.
2.  **One Branch, One Desktop Instance.** Never switch Git branches in a folder while Power BI Desktop has that folder open.
3.  **Worktrees > Branch Switching.** Use `git worktree` to work on multiple branches in parallel folders.
4.  **Re-open after External Edits.** If you edit TMDL/JSON in VS Code, you MUST restart Power BI Desktop to see changes.
5.  **Save in Desktop after Agent Edits.** If an MCP agent updates your model, click SAVE in Desktop to write changes to disk before committing.

## Git Command Reference

### Basic Workflow
```powershell
# 1. Check status
git status

# 2. Stage all changes
git add .

# 3. Commit with message
git commit -m "feat: added YTD measures"

# 4. View history (graph)
git log --oneline --graph --all
```

### Worktree Management (The Safe Way)
*Run from your main repo folder*

```powershell
# Create a new folder (parallel branch) for a feature
git worktree add ../my-project-featureA feature/A

# List active worktrees
git worktree list

# Remove a worktree (delete folder & unlink)
git worktree remove ../my-project-featureA
```

### Merging
```powershell
# Standard Merge (Fast-Forward if possible)
git merge feature/A

# Feature Bubble (Recommended for History)
git merge --no-ff feature/A -m "Merge feature A"
```

## Checklists

### 📝 Pre-Commit
- [ ] **Save** all changes in Power BI Desktop.
- [ ] **Verify** no `unappliedChanges.json` exists (apply all PQ changes).
- [ ] **Check** `git status` - are you committing huge binary files by accident?
- [ ] **Review** diffs in VS Code - does the TMDL change look intentional?

### 🚀 Pre-Publish (DEV)
- [ ] **Merge** feature branch into `main`.
- [ ] **Open** `main` folder in Power BI Desktop.
- [ ] **Refresh Preview** to ensure visuals aren't broken.
- [ ] **Publish** to workspace "DEV".

### 📦 Pre-Deploy (TEST/PROD)
- [ ] **Verify** DEV deployment in browser.
- [ ] **Run Pipeline** in Power BI Service (DEV -> TEST).
- [ ] **Config Rules**: Check deployment rules (params) matched the target environment.
- [ ] **Refresh Dataset** in Service immediately after deployment.

## Troubleshooting Quick-Ref

| Symptom | Probable Cause | Fix |
| :--- | :--- | :--- |
| **"File in use" error in Git** | Desktop is open | Close Power BI Desktop or use Worktrees. |
| **New measure missing in Desktop** | Edited TMDL externally | Close and Re-open `.pbip` in Desktop. |
| **Repo size is 500MB+** | Committed cache/pbix | Check `.gitignore`, run `git rm --cached` on binaries. |
| **Visuals broken after merge** | Bad `report.json` merge | Revert merge, use "Take Theirs" strategy, or copy visual manually. |
