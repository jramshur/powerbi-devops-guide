# Power BI DevOps Guide

> **A comprehensive guide to professional engineering workflows for Power BI.**
> *Source Control. CI/CD strategies. Developer Discipline.*

Welcome! This repository documents the modern "Code-First" approach to Power BI development, moving beyond the "Save As" chaos of single PBIX files.

## 📚 The Handbook

All documentation is located in the **[`docs/`](./docs/)** directory.

### 🏁 Getting Started
- **[Setup & Installation](./docs/01-setup.md)**: Prerequisites, VS Code setup, and the critical `.gitignore`.
- **[Git Concepts for Power BI](./docs/02-git-concepts.md)**: A primer on Commits, Branches, and why "Feature Branching" is safer than "Shared Folder" editing.

### 🛠️ Daily Workflow
- **[Developer Workflow](./docs/03-workflow.md)**: The "Three Edit Loops" (Desktop, Code, Copilot) and how to use key features like **Git Worktrees**.
- **[Cheat Sheet](./docs/cheatsheet.md)**: Quick reference commands and troubleshooting tips.
- **[AI-Assisted Development](./docs/05-ai-workflow.md)**: How to use Copilot and AI tools for faster, cleaner DAX/TMDL.

### 🚀 Deployment & Operations
- **[Deployment Strategy](./docs/04-deployment.md)**: How to safely promote code from DEV to PROD using Power BI Deployment Pipelines.

### 📖 Reference
- **[Useful Resources](./docs/resources.md)**: A collection of articles, videos, and official documentation.

## Why this exists?

Power BI has historically been a "low-code" tool, but managing it at scale requires "high-code" discipline. With the release of **PBIP (Power BI Project)** format, we can finally treat reports and datasets as true software projects.

**This guide aims to solve:**
- "Who changed the data model?"
- "I can't open the file because Dave has it open."
- " How do we roll back to last week's version?"
- "We broke production and don't know why."
