# Setup & Configuration

Before you start version controlling your Power BI projects, you need to set up your environment correctly. This ensures you only commit the "source code" (definitions) and not the heavy data caches.

## 1. Prerequisites

### Essential Tools
- **Power BI Desktop**: Ensure you have the latest version.
- **Git**: [Download Git for Windows](https://git-scm.com/download/win).
- **VS Code**: [Download Visual Studio Code](https://code.visualstudio.com/). We recommend this for managing Git and editing `.tmdl` files.

### Optional but Recommended
- **GitHub Desktop**: If you prefer a GUI for Git operations.
- **Tabular Editor 3 (or 2)**: For advanced model editing.

## 2. Power BI Desktop Configuration

You must enable the **Power BI Project (.pbip)** save option.

1.  Open Power BI Desktop.
2.  Go to **File** > **Options and settings** > **Options**.
3.  Under **Preview features**, check **Power BI Project (.pbip) save option**.
4.  Restart Power BI Desktop.

Now, when you "Save As", you will see the `.pbip` option.

## 3. The Critical `.gitignore`

Power BI generates many local cache files when working with projects. Committing these bloats your repository and causes merge conflicts.

### Repository Structure
Stop thinking "one file." Start thinking "Repository."

```text
/my-analytics-repo
├── .gitignore              <-- The most important file
├── README.md               <-- Documentation
└── src/
   ├── Sales.Report/        <-- PBIP Folder
   └── Sales.Dataset/       <-- PBIP Folder
```

### What to Ignore
| File Type | Why Ignore it? |
| :--- | :--- |
| `*.abf` | **Analysis Services Backup File**. This is the data cache. It can be huge. |
| `*.mbr` | **Model Backup**. Internal backup used by the engine. |
| `*.cache` | Temporary cache files. |
| `localSettings.json` | Your specific user settings (e.g., "Last Page Viewed"). |
| `unappliedChanges.json`| **Power Query Lock**. If you save while "Apply Changes" is pending. |

### Copy-Paste `.gitignore`
Create a file named `.gitignore` at the root of your repo and add this content:

```gitignore
# Power BI temporary files
*.abf
*.mbr
*.cache
*.pbix

# User-specific settings
localSettings.json
unappliedChanges.json
.DS_Store
Thumbs.db
```

## 4. Troubleshooting

### "My Repo is huge!"
- **Cause**: You likely committed the data cache (`.abf`) or an accidental `.pbix` backup.
- **Fix**: Check `.gitignore`. Add `*.abf`. Run `git rm --cached *.abf` to stop tracking it, then commit.

### "I edited the TMDL file but don't see changes in Desktop"
- **Fact**: Desktop loads the definition into memory on open. It does **NOT** watch for file changes in real-time.
- **Fix**: You must **close** and **re-open** the `.pbip` file in Desktop to load external changes.

### "Merge Conflicts in `report.json`"
- **Pain**: Two people moved the same visual, or changed the same bookmark.
- **Real Talk**: JSON merging for reports is difficult.
- **Fix**: Pick "Their Version" or "My Version" completely (don't try to merge lines). Then manually re-do the missing small change in Desktop.

## 5. Further Reading

For more external resources, articles, and videos, see **[Useful Resources](./resources.md)**.
