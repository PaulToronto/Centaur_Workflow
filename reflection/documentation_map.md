# Centaur Workflow — Documentation Map (v1.6)

This document explains **where everything lives** in the `Centaur_Workflow` project and how the main pieces fit together.

---

## 1. Project Layout (Local)

Local project root:

```text
~/Projects/Centaur_Workflow/
```

Key top-level folders:

- `processing/`  
  Working code, notebooks, experiments, drafts. This is where you *do* the work.

- `outputs/`  
  Stable, presentable artifacts to share:
  - `outputs/blog_drafts/`
  - `outputs/exports/`
  - `outputs/figures/`

- `reflection/`  
  Logs, meta-documentation, collaboration rules, and workflow notes.

- `notebooks/`  
  Local mirror of Colab notebooks pulled from Google Drive (via pull/sync scripts).

- `.git/`  
  Git repository metadata for this project.

There is also a **shared toolset** (outside this repo):

```text
~/Projects/_workflow_tools/
```

This contains reusable scripts that multiple Centaur projects can share.

---

## 2. Core Documentation Files (in `reflection/`)

All of these live in:

```text
~/Projects/Centaur_Workflow/reflection/
```

- `documentation_map.md` (**this file**)  
  - High-level map of the project’s structure and docs.

- `collaboration_rules.md`  
  - How you and ChatGPT collaborate on Centaur projects.
  - Conventions about prompts, tangents, “yes by default,” and how we resume steps.

- `changelog.md`  
  - Human-readable history of major changes to tools, docs, and structure.
  - Versioned entries (v1.0, v1.1, … v1.6).

- `sync_tools_master_quickref.md`  
  - One-page guide to all scripts in `~/Projects/_workflow_tools/`.
  - Commands, arguments, and when to use each tool.

---

## 3. Logs & Audit Trails (in `reflection/logs/`)

Logs folder:

```text
~/Projects/Centaur_Workflow/reflection/logs/
```

Main log files:

- `setup_log.txt`  
  - Milestones from initial project setup.

- `sync_log.txt`  
  - History of `sync_to_drive.sh` runs (Drive syncs).

- `watch_log.txt`  
  - Activity from `watch_workflow.sh` (auto-sync events).

- `verify_log.md`  
  - Human-readable record of `verify_workflow.sh` runs.
  - Summarizes structure checks, Drive mirror status, etc.

- `colab_sync_log.txt` / `notebook_pull_log.txt`  
  - Logs for notebook sync/pull operations between Drive and local `notebooks/`.

- `link_audit_report.md`  
  - Output of link audits (`--audit-links` / `--audit` in `verify_workflow.sh`).
  - NBViewer/Colab link presence/missing flags.

- `notebook_audit_report.md`  
  - Output of notebook consistency audits (`--audit-nb` / `--audit`).
  - Compares local `notebooks/` to Drive copies.

- `git_sync_log.txt`  
  - Output of `push_to_github.sh` runs.
  - Helpful when diagnosing Git/GitHub auth or remote issues.

---

## 4. Centaur Log (Verification Stamp)

File:

```text
~/Projects/Centaur_Workflow/reflection/centaur_log.md
```

Purpose:

- Simple append-only log of **successful verification runs** that used `--audit` or similar modes.
- Gives you a quick “trust indicator” that, as of certain timestamps, structure + Drive + notebooks passed checks.

You never edit this by hand; `verify_workflow.sh` maintains it.

---

## 5. Shared Workflow Tools (in `_workflow_tools/`)

Shared script location:

```text
~/Projects/_workflow_tools/
```

Key scripts (see `sync_tools_master_quickref.md` for details):

- `create_centaur_project.sh`  
- `sync_to_drive.sh`  
- `verify_workflow.sh`  
- `watch_workflow.sh`  
- `export_from_chatgpt.sh`  
- `check_quickrefs.sh`  
- `sync_colab_notebooks.sh`  
- `pull_colab_notebooks.sh`  
- `add_nb_links.sh`  
- `push_to_github.sh`  

These scripts *assume*:
- Local project: `~/Projects/<ProjectName>/`
- Drive mirror: `~/Library/CloudStorage/GoogleDrive-…/My Drive/Colab Notebooks/Data Science/Projects/<ProjectName>/`

`documentation_map.md` (this file) and `sync_tools_master_quickref.md` keep their behavior documented.

---

## 6. Notebook Flow (Colab ↔ Local ↔ GitHub)

High-level flow for notebooks:

1. **Author/edit in Colab** (linked to Drive).  
2. **Drive path** for project notebooks:  
   `My Drive/Colab Notebooks/Data Science/Projects/<ProjectName>/notebooks/`

3. **Pull notebooks to local**:

   ```bash
   ~/Projects/_workflow_tools/pull_colab_notebooks.sh <ProjectName>
   ```

   - Populates `~/Projects/<ProjectName>/notebooks/`

4. **Run audits** as needed:

   ```bash
   cd ~/Projects/<ProjectName>
   ./verify_workflow.sh <ProjectName> --audit-nb
   ./verify_workflow.sh <ProjectName> --audit
   ```

5. **Sync back to Drive** (if local edits are made):

   ```bash
   ~/Projects/_workflow_tools/sync_to_drive.sh <ProjectName>
   ```

6. **Optionally push to GitHub**:

   ```bash
   ~/Projects/_workflow_tools/push_to_github.sh <ProjectName>
   ```

   - First run will help you set up the remote and remember credentials.
   - After that, it’s usually a single command.

---

## 7. Versioning & How to Evolve This Map

- This file is currently **v1.6-aligned** with your tooling and structure.
- When tools or layout change:
  1. Update this map to reflect new structure or behavior.
  2. Add an entry to `changelog.md` (new version line).
  3. Optionally run `verify_workflow.sh <ProjectName> --audit` to log a fresh verification stamp.

If you ever feel “lost” in the project again, this is the **first doc to open**.
