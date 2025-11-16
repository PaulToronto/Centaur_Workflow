# Centaur Workflow — Sync & Tooling Quick Reference (v1.6)

Location of tools:

```text
~/Projects/_workflow_tools/
```

All commands below assume you run them from **any shell** (they use full paths), and that:

- Local projects live in `~/Projects/<ProjectName>/`
- Drive mirror lives under your Google Drive “Projects” hierarchy
- Git is initialized in the project root

---

## 1. Project Creation

### `create_centaur_project.sh`

**Purpose:**  
Create a new Centaur-style project (local structure + git init + basic docs stubs).

**Usage:**

```bash
~/Projects/_workflow_tools/create_centaur_project.sh <ProjectName>
```

**What it does:**

- Creates `~/Projects/<ProjectName>/` with:
  - `processing/`, `outputs/`, `reflection/`, `.git/`
- Initializes Git repo (if needed).
- Adds starter docs/logs as needed.

---

## 2. Drive Sync

### `sync_to_drive.sh`

**Purpose:**  
Sync key project folders between local and Google Drive (one-way: local → Drive).

**Usage:**

```bash
~/Projects/_workflow_tools/sync_to_drive.sh <ProjectName>
```

**Syncs:**

- `reflection/` → Drive `.../<ProjectName>/reflection/`
- `outputs/` → Drive `.../<ProjectName>/outputs/`
- `notebooks/` → Drive `.../<ProjectName>/notebooks/` (if it exists)

**Notes:**

- Writes a line to `reflection/logs/sync_log.txt`
- Honors your current Google Drive mount at:  
  `~/Library/CloudStorage/GoogleDrive-.../My Drive/Colab Notebooks/Data Science/Projects/`

---

## 3. Verification & Audits

### `verify_workflow.sh`

**Purpose:**  
Check project structure, tool presence, Git status, Drive mirror, and audits for links/notebooks.

**Basic usage:**

```bash
cd ~/Projects/<ProjectName>
./verify_workflow.sh <ProjectName>
```

**Modes:**

- **Normal (default)**  
  ```bash
  ./verify_workflow.sh <ProjectName>
  ```  
  Checks structure, tools count, Git clean/dirty, and Drive mirror presence.

- **Audit Links:**  
  ```bash
  ./verify_workflow.sh <ProjectName> --audit-links
  ```  
  Generates/overwrites `reflection/logs/link_audit_report.md`.

- **Audit Notebooks:**  
  ```bash
  ./verify_workflow.sh <ProjectName> --audit-nb
  ```  
  Generates/overwrites `reflection/logs/notebook_audit_report.md`.

- **Full Audit (links + notebooks) + verification stamp:**  
  ```bash
  ./verify_workflow.sh <ProjectName> --audit
  ```  
  - Runs both link + notebook audits.  
  - Appends a “✅ Verified” timestamp to `reflection/centaur_log.md`.

**Logs:**

- Main result: `reflection/logs/verify_log.md`  
- Link audit: `reflection/logs/link_audit_report.md`  
- Notebook audit: `reflection/logs/notebook_audit_report.md`  
- Verification stamp: `reflection/centaur_log.md`

---

## 4. Watcher (Auto Sync)

### `watch_workflow.sh`

**Purpose:**  
Watch key folders for changes and auto-trigger sync + optional verification.

**Usage:**

```bash
~/Projects/_workflow_tools/watch_workflow.sh <ProjectName> [--drive-only | --git-only]
```

- If no mode flag: performs both Drive sync and Git push (when configured).
- `--drive-only`: only calls `sync_to_drive.sh` on changes.
- `--git-only`: only calls `push_to_github.sh` on changes.

**What it watches:**

- `reflection/`
- `outputs/`
- `processing/`
- `_workflow_tools/` (so tool updates also cause syncs)

**Notes:**

- Uses `fswatch` (installed via `brew install fswatch`).  
- Logs to `reflection/logs/watch_log.txt`.  
- Includes debouncing and basic deduplication to avoid infinite loops.

Press **Ctrl+C** to stop.

---

## 5. ChatGPT Export Helper

### `export_from_chatgpt.sh`

**Purpose:**  
Placeholder / helper for moving exports from ChatGPT/Atlas into the project tree in a consistent way.

*(Current implementation in your environment is minimal; you can evolve it later.)*

---

## 6. Quickref Consistency Check

### `check_quickrefs.sh`

**Purpose:**  
Scan `_workflow_tools/` for scripts and check that each has a corresponding Quick Reference card entry.

**Usage:**

```bash
~/Projects/_workflow_tools/check_quickrefs.sh
```

**Output:**

- Reports scripts that are missing quickref entries.
- Helps you keep `sync_tools_master_quickref.md` in sync with actual tools.

---

## 7. Colab / Drive Notebook Sync

These scripts assume your Drive layout:

```text
My Drive/
  Colab Notebooks/
    Data Science/
      Projects/
        <ProjectName>/
          notebooks/
```

### `sync_colab_notebooks.sh`

**Purpose:**  
High-level wrapper to keep local and Drive notebooks aligned.

**Usage:**

```bash
~/Projects/_workflow_tools/sync_colab_notebooks.sh <ProjectName>
```

**Behavior:**

- Currently wired as a simple “pull from Drive → local `notebooks/`” for your test project.
- Can be extended into a two-way or more complex sync later.

### `pull_colab_notebooks.sh`

**Purpose:**  
Pull notebooks from Drive into local project’s `notebooks/` folder.

**Usage:**

```bash
~/Projects/_workflow_tools/pull_colab_notebooks.sh <ProjectName>
```

**Result:**

- Populates `~/Projects/<ProjectName>/notebooks/` from Drive’s `notebooks/` folder.
- Logs to `reflection/logs/notebook_pull_log.txt` (and/or related logs).

### `add_nb_links.sh`

**Purpose:**  
Add/update NBViewer links (and, in future, helper info for Colab/Drive URIs) for project notebooks.

**Usage:**

```bash
~/Projects/_workflow_tools/add_nb_links.sh <ProjectName>
```

**Result:**

- Scans `notebooks/` for `.ipynb` files.
- Updates/creates link references according to your chosen conventions.
- Works in tandem with `verify_workflow.sh --audit` and documentation in `documentation_map.md`.

*(Current behavior has been refined to respect your preference: Colab launch links documented in text/README/docs, NBViewer links optionally in notebooks.)*

---

## 8. GitHub Sync

### `push_to_github.sh`

**Purpose:**  
Stage, commit, and push project changes to GitHub with minimal typing.

**Usage:**

```bash
~/Projects/_workflow_tools/push_to_github.sh <ProjectName>
```

**Behavior:**

1. Ensures you’re in `~/Projects/<ProjectName>/`.
2. Stages changes.
3. Creates a timestamped commit message:  
   `Centaur sync: YYYY-MM-DD HH:MM:SS`
4. Pushes to `origin` on the `main` branch.

**First-time remote setup:**

- If no `origin` remote exists:
  - Prompts for GitHub username/org.
  - Proposes `https://github.com/<UserOrOrg>/<ProjectName>.git` as remote.
  - Reminds you to create the empty GitHub repo if needed.
- Uses macOS Keychain (`credential.helper=osxkeychain`) to remember your PAT credentials once set up.

**Logs:**

- Writes to `reflection/logs/git_sync_log.txt`  
  (useful for diagnosing any future Git/GitHub issues).

---

## 9. Typical Daily Commands

For `Centaur_Workflow` (or any future project `<ProjectName>`):

```bash
# 1. Verify everything is healthy
cd ~/Projects/Centaur_Workflow
./verify_workflow.sh Centaur_Workflow --audit

# 2. Sync local → Drive
~/Projects/_workflow_tools/sync_to_drive.sh Centaur_Workflow

# 3. Pull latest Colab notebooks (if you edited them in Colab)
~/Projects/_workflow_tools/pull_colab_notebooks.sh Centaur_Workflow

# 4. Optional: push to GitHub
~/Projects/_workflow_tools/push_to_github.sh Centaur_Workflow
```

This quickref is meant to be **glanceable**. For deeper design rationale, see:

- `reflection/documentation_map.md`
- `reflection/changelog.md`
- `reflection/collaboration_rules.md`
