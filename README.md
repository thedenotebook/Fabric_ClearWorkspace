# Microsoft Fabric – Workspace Clear Tool 💣🦺

A safety-first Python utility for clearing a Microsoft Fabric workspace during POCs, migrations, and non-production resets.
This script deletes all Fabric items and folders in a workspace (except the running notebook), with multiple guardrails to reduce the risk of accidental data loss.

⚠️ This tool is destructive.
Use only in non-production environments and only if you fully understand the impact.

🔍 Why this exists

Manually clearing a Fabric workspace is:
  - slow
  - repetitive
  - error-prone
  - easy to get wrong (especially with folders)

This tool was built to support:
  - Microsoft Fabric migrations
  - Platform Ops workflows
  - POC resets
  - Controlled environment rebuilds (e.g. collation changes)

🚨 WARNING – READ FIRST
🔴 This script permanently deletes Fabric items.

Before running:
✅ Confirm you are in the correct workspace
✅ Ensure the workspace is NOT production
✅ You have Contributor or higher permissions
✅ You understand that deletion is not reversible

The author is not responsible for data loss caused by misuse.

✨ Key features

🦺 Safety-first design
  - Exact workspace name verification
  - Optional dry-run mode
  - Countdown before deletion begins

🧾 Clear logging
  - Grouped by artifact type
  - Success / skipped / failure summaries

🧹 Correct folder deletion

  - Deletes leaf folders first
  - Deletes root folders only when empty
  - Avoids infinite loops and FolderNotEmpty errors

⚙️ Fabric-native
  - Uses sempy.fabric
  - Designed to run inside a Fabric Python notebook

🧩 Supported & skipped artifacts

Deleted
  - Lakehouses
  - Warehouses
  - Notebooks (except the running one)
  - Pipelines
  - Dataflows Gen2
  - Other standard Fabric artifacts

Skipped intentionally
  - Dashboards (API deletion not supported)
  - SQL Endpoints (deleted with parent lakehouse/warehouse)
** These rules can be extended if new APIs become available.

🧑‍💻 How to run

1️⃣ Open a Fabric Python notebook
  - The notebook must live in the workspace you want to clear
  - The script intentionally avoids deleting the running notebook

2️⃣ Configure the script
  - confirmWorkspaceName = "Workspace_XXX"
  - dryRun = True
  - countDown = 10

Setting	                   | Description
confirmWorkspaceName	     - Must exactly match the workspace name
dryRun	                   - True = print actions only (recommended first)
countDown	                 - Seconds to wait before deletion

3️⃣ Run a dry run (recommended)
clear_workspace(confirmWorkspaceName, dryRun=True, countDown=10)

Review the output carefully before proceeding.

4️⃣ Execute for real
clear_workspace(confirmWorkspaceName, dryRun=False, countDown=10)

When complete:
  - The workspace will be empty
  - The running notebook will remain
  - Delete the notebook manually if required

🧠 Git integration gotcha (important)

Microsoft Fabric uses soft delete.

If your workspace is connected to Git:
  - Deleted items may remain cached briefly
  - Recreating items with the same names too quickly can cause Git sync conflicts

Recommendation:

  - Wait a short period before syncing an empty workspace
  - Especially if rebuilding with the same structure

🧠 Final note

This tool is designed for engineers who treat Fabric as a platform, not a playground.

If you don’t fully understand what it does —
do not run it.
