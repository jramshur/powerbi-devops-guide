# Git & OneDrive: Risks and Best Practices

Using Git inside a OneDrive-synced folder is **highly discouraged**. The fundamental issue is that Git and OneDrive are both version control systems (Git for code, OneDrive for files) that operate in conflicting ways. When combined, they often lead to performance degradation, file locking issues, and in severe cases, repository corruption.

For Power BI projects (`.pbip`), this risk is amplified because a single Power BI project decomposes into hundreds of small text and JSON files, increasing the likelihood of sync conflicts.

## Key Issues

1.  **Race Conditions & File Locking**
    *   **The Problem:** Git operations (like `git status`, `git commit`, `git checkout`) create and modify many temporary files instantly. OneDrive's sync engine detects these changes and attempts to lock the files to upload them to the cloud.
    *   **The Result:** Git may fail with "permission denied" or "resource busy" errors because OneDrive has locked a file that Git needs to write to.

2.  **Performance Degradation**
    *   **The Problem:** A Git repository, especially with history, contains thousands of tiny files in the hidden `.git` folder.
    *   **The Result:** OneDrive tries to sync every single one of these change events. This consumes massive CPU/RAM and network bandwidth, slowing down both your computer and your Git operations.

3.  **Repository Corruption**
    *   **The Problem:** If a sync conflict occurs inside the `.git` folder (where Git stores its history database), the repository structure can become invalid.
    *   **The Result:** You may encounter "fatal: not a git repository" errors or lose commit history. Recovering from this is difficult and often requires a fresh clone.

4.  **"Files on Demand" Conflicts**
    *   **The Problem:** OneDrive's "Files on Demand" feature keeps placeholders on your disk and downloads files only when accessed.
    *   **The Result:** Git expects all files to be physically present to calculate checksums (hashes). If files are "online-only," Git operations will trigger mass downloads, causing extreme slowness or timeouts.

## Specific Impact on Power BI Projects

Modern Power BI projects (`.pbip` format) save reports as a folder structure containing:
-   `report.json`
-   `model.bim`
-   Various metadata files

Because a single `.pbix` file is exploded into many small text files, the number of files OneDrive needs to track increases significantly, multiplying the risks mentioned above.

## Recommendations

### 1. The Best Practice: Move Repo Outside OneDrive
The safest and most reliable solution is to move your Git repositories to a local folder that is **not** synced by OneDrive.

*   **Recommended Path:** `C:\Dev\` or `C:\Users\YourName\Repos\`
*   **Workflow:**
    1.  Develop in the local `C:\Dev\MyProject` folder.
    2.  Use Git to push your changes to a remote repository (like GitHub, Azure DevOps, or Bitbucket) for backup and collaboration.
    3.  Treat the remote repository (GitHub/ADO) as your "cloud backup," not OneDrive.

### 2. The "If You Must" Workaround: Git-Worktree (Advanced)
If you absolutely must have your working files in OneDrive (e.g., for non-Git collaboration), you can theoretically store the `.git` directory outside of OneDrive while keeping the working tree inside. However, this is complex to set up and maintain, and still prone to file-locking issues with the working files themselves. **This is not recommended for most users.**

### 3. Compromise: Copying for Publication
If you need the final `.pbix` file in OneDrive for sharing:
1.  Work in a local, non-synced folder (`C:\Dev`).
2.  Use Git for version control.
3.  When ready to share, manually copy/export the `.pbix` file to the OneDrive folder.

## Conclusion
To ensure data integrity and a smooth workflow, **move your active development folders out of OneDrive**. Rely on your Git remote (Azure DevOps / GitHub) for backing up your code history.
