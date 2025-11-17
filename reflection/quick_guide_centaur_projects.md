# 🚀 Quick Guide: Creating New Centaur Workflow Projects

This guide walks you through the exact steps to create, initialize, and fully sync a **new Centaur Workflow project** across:
- Local filesystem
- Google Drive mirror
- GitHub repository (optional but recommended)
- ChatGPT Project Workspace

---

## 1. 📁 Create a New Project Locally

Use your master tool:

```bash
~/Projects/_workflow_tools/create_centaur_project.sh <ProjectName>
```

Example:
```bash
~/Projects/_workflow_tools/create_centaur_project.sh ML_Practice
```

This creates:

```
~/Projects/ML_Practice/
    reflection/
    processing/
    outputs/
    notebooks/     ← only added once you use Colab tools
```

It also:
- initializes Git locally  
- creates `.gitignore`  
- logs setup  
- prepares Drive mirror path  

---

## 2. 🔁 Sync Project to Google Drive

Once created:

```bash
~/Projects/_workflow_tools/sync_to_drive.sh <ProjectName>
```

This ensures:

```
Google Drive/
    My Drive/
        .../Centaur_Workflow/<ProjectName>/
```

contains an exact mirror.

---

## 3. 🔍 Verify the Project Structure

Run:

```bash
./verify_workflow.sh <ProjectName>
```

Optional audits:

```bash
./verify_workflow.sh <ProjectName> --audit
./verify_workflow.sh <ProjectName> --audit-links
./verify_workflow.sh <ProjectName> --audit-nb
```

---

## 4. ☁ Add Google Colab Notebooks (Optional)

To pull notebooks from Drive → Local:

```bash
~/Projects/_workflow_tools/pull_colab_notebooks.sh <ProjectName>
```

To push notebooks Local → Drive:

```bash
~/Projects/_workflow_tools/push_colab_notebooks.sh <ProjectName>
```

To add NBViewer links into each `.ipynb`:

```bash
~/Projects/_workflow_tools/add_nb_links.sh <ProjectName>
```

---

## 5. 🧭 (Optional) Push to GitHub

Once your project is complete:

```bash
./push_to_github.sh <ProjectName>
```

If the repo exists → pushes normally.  
If not → script can auto-create the repo (your chosen mode).

---

## 6. 🧠 Use ChatGPT as Project “Brain”

Inside ChatGPT:
- Set your active project
- Store docs in the project
- When you request a document (e.g., `README.md`, report.md, etc.)  
  **The Doc Sync Sentinel will auto-generate a downloadable file.**

---

## 7. 🔄 Full Sync Cycle

When you update anything locally:

```bash
~/Projects/_workflow_tools/watch_workflow.sh <ProjectName>
```

Automatically syncs:
- Reflection logs  
- Outputs  
- Notebooks  
- Git (optional)  
- Drive copy  

Uses smart-dedup + debounce to avoid infinite loops.

---

# 🎉 You're Ready to Start Any New Project

This Quick Guide lives in:

```
reflection/quick_guide_centaur_projects.md
```

and will always sync everywhere automatically.

