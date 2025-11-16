# Centaur Workflow — Changelog

A concise, human-readable record of significant changes to tools, structure, and documentation for the `Centaur_Workflow` project.

This log is **project-specific** (for `Centaur_Workflow`) but many entries reflect improvements to the shared `_workflow_tools` scripts that will benefit future projects as well.

---

## v1.6 — GitHub Integration & Notebook Flow Refinements
**Date:** 2025-11-16

- Added `push_to_github.sh`:
  - Automates staging, committing, and pushing project changes.
  - Handles first-time remote setup (prompts for GitHub username/org and remote URL).
  - Integrates with macOS Keychain via `credential.helper=osxkeychain` to avoid repeated PAT prompts.
  - Logs all pushes to `reflection/logs/git_sync_log.txt`.

- Finalized **local ↔ Drive ↔ Colab** notebook workflow:
  - Confirmed `notebooks/` as the local home for Colab notebooks.
  - Ensured `sync_to_drive.sh` syncs `notebooks/` to the Drive-side project folder.
  - Tightened the interaction between `pull_colab_notebooks.sh`, `sync_colab_notebooks.sh`, and `verify_workflow.sh --audit-nb`.

- Consolidated documentation:
  - Replaced versioned changelog fragments with a single `reflection/changelog.md`.
  - Updated `reflection/documentation_map.md` to reflect the current structure and notebook flow.
  - Updated `reflection/sync_tools_master_quickref.md` to describe all current tools.

---

## v1.5 — Central Documentation & Master Quickref
**Date:** 2025-11-15

- Introduced `reflection/documentation_map.md`:
  - A “you are here” map explaining the role of `processing/`, `outputs/`, `reflection/`, `notebooks/`, and `_workflow_tools/`.
  - Documents where logs, audits, and core docs live.

- Created `reflection/sync_tools_master_quickref.md`:
  - Single quick reference for all tools in `~/Projects/_workflow_tools/`.
  - Includes brief purpose and usage for each script.

- Refined `reflection/collaboration_rules.md`:
  - Clarified norms for:
    - Tangents and how we resume steps.
    - “Yes by default” behavior for safe enhancements.
    - Expectations around estimates, versions, and documentation updates.

- Retired versioned changelog filenames in favor of the single `changelog.md` you’re reading now.

---

## v1.4 — Notebook Audit Integration & Drive Mirror Detection
**Date:** 2025-11-14

- Enhanced `verify_workflow.sh` to **remember** the Drive mirror path:
  - Detects it once (after a successful sync) and reuses it on subsequent runs.
  - Avoids repeated “Drive mirror not verified yet” messages once properly configured.

- Added **notebook audit mode**:
  - `./verify_workflow.sh <ProjectName> --audit-nb`:
    - Compares local `notebooks/` contents with expected Drive layout.
    - Writes results to `reflection/logs/notebook_audit_report.md`.

- Ensured `sync_to_drive.sh` correctly handles your current Google Drive mount:
  - Uses the `Library/CloudStorage/GoogleDrive-…` convention.
  - Confirmed correct path under `My Drive/Colab Notebooks/Data Science/Projects/`.

---

## v1.3 — Link Audits (NBViewer & Colab Awareness)
**Date:** 2025-11-13

- Extended `verify_workflow.sh` with **link auditing**:

  - `./verify_workflow.sh <ProjectName> --audit-links`
  - And combined mode: `--audit` (runs both link + notebook audits where applicable).

- Link audit behavior:
  - Scans notebooks and key markdown files for NBViewer and Colab-related links.
  - Records findings in `reflection/logs/link_audit_report.md`.
  - Helps ensure that documentation and notebooks don’t drift away from actual URLs.

- Laid groundwork for the later refinement that **Colab links should live in docs/README** rather than inside the notebook body itself.

---

## v1.2 — Drive Mirror Awareness & Centaur Verification Log
**Date:** 2025-11-12

- Improved `verify_workflow.sh`:

  - Added explicit Drive mirror check, with clearer messaging:
    - “Drive mirror not verified yet” vs. confirmed path.
  - Began capturing “✅ Verified” timestamps for full audits in:
    - `reflection/centaur_log.md`

- Purpose of `centaur_log.md`:
  - Acts as a simple, append-only record that the project has passed:
    - Structure checks
    - Tool presence checks
    - Drive mirror / audit checks
  - Gives you confidence the project was in a good state at specific moments.

---

## v1.1 — Watcher Improvements & Debounced Auto-Sync
**Date:** 2025-11-11

- Upgraded `watch_workflow.sh` from a simple `fswatch` loop to a more robust watcher:

  - Added **debounce logic** to avoid rapid-fire loops when logs change.
  - Ignored changes inside log files that are written as a result of the sync itself (preventing “storm” behavior).
  - Provided modes:
    - `--drive-only` (only trigger Drive sync)
    - `--git-only` (in future, only trigger Git pushes)
    - default: combined behavior where appropriate.

- Directed watcher logs to:
  - `reflection/logs/watch_log.txt`

- Smoothed integration with `sync_to_drive.sh` and `verify_workflow.sh` for a more “hands-off” workflow during active editing sessions.

---

## v1.0 — Initial Centaur Workflow Scaffolding
**Date:** 2025-11-02 (initialization window)

Initial creation of the Centaur Workflow project pattern and shared tooling, including:

- **Local project structure** for `Centaur_Workflow`:

  - `processing/`  
  - `outputs/` (`blog_drafts/`, `exports/`, `figures/`)  
  - `reflection/` (`logs/`, `reviews/` initially)  
  - `.git/` (Git repository root)

- **Baseline scripts in `~/Projects/_workflow_tools/`:**

  - `create_centaur_project.sh`
  - `sync_to_drive.sh`
  - `verify_workflow.sh` (initial, simpler version)
  - Early prototype of `export_from_chatgpt.sh`

- **Google Drive integration concept:**

  - Decided that for data and notebooks, Google Drive should be treated as the primary mirror for sharing and backup, while GitHub focuses on code and light-weight artifacts.

---

## How to Use This Changelog

- When you modify tools, paths, or semantics in a non-trivial way:
  1. Add a new version section at the **top** of this file.
  2. Briefly describe:
     - What changed
     - Why it changed
     - Any impact on existing projects or future workflows
  3. Optionally run:
     ```bash
     ./verify_workflow.sh <ProjectName> --audit
     ```
     to capture a new verification stamp in `reflection/centaur_log.md`.

This keeps your Centaur Workflow **transparent and traceable**, even as it evolves.
