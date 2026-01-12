# Power BI Desktop + Local Git Documentation

Welcome! This documentation helps Power BI developers adopt professional version control practices using **Power BI Projects (PBIP)** and **Local Git**.

No Azure DevOps or GitHub Enterprise required—just you, VS Code, and Power BI Desktop.

## Key Documents

### 1. [The Guide (`powerbi-git-guide.md`)](./powerbi-git-guide.md)
**Start here.** A comprehensive narrative on:
- How PBIP changes the game.
- Setting up your repo and `.gitignore`.
- The "Three Edit Loops" (Desktop, Code, AI).
- Why `git worktree` is essential for safe Power BI workflows.
- Merging and Promotion strategies.

### 2. [Git Basics Deep Dive (`git-basics-deep-dive.md`)](./git-basics-deep-dive.md)
**New to Git?** Start here.
- Plain English explanations of Commit, Branch, Merge.
- Why we recommend "Feature Branching" for Power BI.
- How to use `git worktree` to work on **parallel** branches avoiding file locking issues.

### 3. [The Cheat Sheet (`powerbi-git-cheatsheet.md`)](./powerbi-git-cheatsheet.md)
**Keep this open.** Quick reference for:
- Golden Rules of PBIP + Git.
- Essential copy-paste Git commands.
- Checklists for Committing and Deploying.
- Rapid troubleshooting table.

## Quick Start Summary
1.  Enable **Power BI Project (.pbip)** save option in Desktop preview features.
2.  Initialize a local git repo (`git init`).
3.  Add the recommended **.gitignore**.
4.  Save your report as a **Project (PBIP)**.
5.  Use **VS Code** to manage commits and branches.
