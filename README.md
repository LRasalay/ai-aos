# AI OS Template for Claude Code & Windsurf

This repository provides a **ready-to-use AI Operating System** template for managing **prompts, tasks, and tool adapters** across multiple repositories.  
It is designed for teams of 100+ engineers using both **Claude Code** and **Windsurf**.

The template includes:

- **Centralized AI prompts** (`ai/`) with core rules, task templates, and tool-specific profiles  
- **Tool wrappers** (`.claude/` and `.windsurf/`) with symlinks to avoid duplication  
- **`ai-cli` script** to standardize task execution  
- **Compose orchestrator** for consistent AI outputs  
- Pre-filled examples for immediate use

---

## 📁 Folder Structure

```text
ai-os-template/
├─ ai/                     ← central AI prompts (can be a submodule)
│  ├─ core/                ← company rules & coding standards
│  ├─ tasks/               ← reusable task templates
│  ├─ profiles/            ← tool-specific behaviors (Claude, Windsurf)
│  └─ compose.md           ← orchestrates rules + task + profile
├─ .claude/                ← Claude Code wrapper
│  └─ commands/run.md
├─ .windsurf/              ← Windsurf wrapper
│  └─ commands/run.md
├─ ai-cli                  ← CLI for running AI tasks
└─ AI_OS_Setup.md          ← instructions
```mermaid
graph LR
A[Developer runs ai-cli] --> B[Tool Wrapper (.claude/.windsurf)]
B --> C[Compose.md orchestrator]
C --> D[Core Rules & Standards (ai/core)]
C --> E[Task Templates (ai/tasks)]
C --> F[Tool Profile (ai/profiles)]
D & E & F --> G[AI Tool executes task]
G --> H[Output returned to developer]
```
⚡ Quick Setup

Clone repository

git clone <your_repo_url> my-repo
cd my-repo

Add central AI prompts submodule

git submodule add git@internal-git:ai-prompts.git ai
git commit -m "Add central AI prompts submodule"

Create symlinks for tool adapters

cd .claude
ln -s ../ai prompts
cd ../.windsurf
ln -s ../ai prompts
cd ..

Make ai-cli executable

chmod +x ai-cli

Run AI tasks

# Architecture plan with Claude
./ai-cli run architecture claude "Add role-based access control"

# Code review with Windsurf
./ai-cli run review windsurf "Refactor authentication module"
📌 Prompt Flow Diagram
```mermaid
flowchart TD
subgraph AI_Prompts
CR[Core Rules] --> COMP[Compose.md]
TP[Task Templates] --> COMP
PR[Profile (Claude/Windsurf)] --> COMP
end
COMP --> TOOL[AI Tool Execution]
TOOL --> DEV[Developer Output]
```
📝 AI Prompt Examples

Core Rules (ai/core/rules.md)

# Core Rules
- Follow repository coding standards
- Preserve module boundaries
- Write unit tests for new logic
- No new dependencies without approval

Example Task Template (ai/tasks/architecture.md)

# Architecture Task
Task: {{TASK}}
Output:
1. Summary
2. Relevant Modules
3. Implementation Steps
4. Tests
5. Risks / Considerations

Claude Profile (ai/profiles/claude.md)

# Claude Profile
You are in deep analysis mode.
Scan the full repository before answering.
Prioritize correctness over brevity.

Windsurf Profile (ai/profiles/windsurf.md)

# Windsurf Profile
You are in inline execution mode.
Focus on currently open files and summarize repo context if needed.
⚙️ AI CLI Script
#!/bin/bash
COMMAND=$1
TASK=$2
TOOL=$3
FEATURE="$4"

if [[ "$COMMAND" != "run" ]]; then
    echo "Usage: ./ai-cli run <task> <tool> \"<feature description>\""
    exit 1
fi

if [[ "$TOOL" == "claude" ]]; then
    claude run --file .claude/commands/run.md --task "$TASK" --feature "$FEATURE"
elif [[ "$TOOL" == "windsurf" ]]; then
    windsurf run --file .windsurf/commands/run.md --task "$TASK" --feature "$FEATURE"
else
    echo "Unknown tool $TOOL"
    exit 1
fi
🔄 Updating AI Prompts
cd ai
git fetch origin
git checkout main
git pull origin main
cd ..
git add ai
git commit -m "Update AI prompts submodule"
git push
🛠 Features

Centralized, versioned prompts for all repos

Symlinks for .claude and .windsurf to avoid duplication

Standardized ai-cli execution

Pre-filled prompt templates for immediate workflow

Scalable for 100+ repos and teams

🎯 Recommended Workflow

Developers run tasks via ai-cli

Tool wrappers reference compose.md

Compose orchestrator combines rules, task templates, and profiles

AI tools produce outputs consistent with company standards

Updates propagate across repos via ai-prompts submodule

📌 Notes

Keep .claude/prompts and .windsurf/prompts symlinked to ai/

Pin submodule commits in production for stability

Customize tasks/*.md and profiles/*.md for your workflow

📌 References

Claude Code

Windsurf IDE
