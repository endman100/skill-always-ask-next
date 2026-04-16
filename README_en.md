# always-ask-next

> **⚠️ Mandatory Rule** — This skill defines the "always ask next" enforcement pattern. It cannot be skipped or bypassed under any circumstances.

## Overview

`always-ask-next` requires the Agent to call `AskUserQuestion` before declaring completion, offering the user 3 dynamically generated follow-up options. This prevents the Agent from unilaterally assuming the session is over or missing subsequent actions.

---

## Installation (Enforcement Required)

Simply placing this skill in a knowledge base does **not** enforce it automatically. Follow the steps below to ensure it applies to every conversation.

---

### GitHub Copilot (VS Code) — Recommended, Global Scope

Create `always-ask-next.instructions.md` in the VS Code User Prompts folder with `applyTo: "**"` to apply it to all workspaces.

**Windows:**
```powershell
$dest = "$env:APPDATA\Code\User\prompts"
New-Item -ItemType Directory -Path $dest -Force | Out-Null
```

**macOS / Linux:**
```bash
mkdir -p "$HOME/Library/Application Support/Code/User/prompts"
# Linux:
# mkdir -p "$HOME/.config/Code/User/prompts"
```

Create `always-ask-next.instructions.md` inside that folder with the following content:

````markdown
---
applyTo: "**"
---

# Mandatory Rule: Always Ask Next After Completing Tasks (always-ask-next)

After **completing all current tasks, before declaring done**, follow these steps:

1. Call `AskUserQuestion` (or equivalent interaction tool)
2. Set `header` to: `"Next Action"`
3. Set `question` to: `"What would you like to do next?"`
4. **Dynamically generate 3 most relevant follow-up options** based on current context
5. Fix the 4th option as: `"Other (please specify)"`

⚠️ This is a mandatory rule. It cannot be skipped, and you must not assume the user does not need a follow-up.

## Skill Path

`<YOUR_SKILL_DIR>/always-ask-next/SKILL.md`
````

> Replace `<YOUR_SKILL_DIR>` with the actual path to your skill knowledge base.

---

### Claude (claude.ai / API system prompt)

Insert the following at the beginning of your system prompt:

```
Additional rule:
After completing all tasks, before declaring done, you MUST call AskUserQuestion with:
- header: "Next Action"
- question: "What would you like to do next?"
- 3 dynamically generated relevant next action options based on current context
- 4th option fixed as: "Other (please specify)"
This rule is mandatory. No exceptions.
```

---

### Cursor / Windsurf / Cline — Rules Files

**Cursor:** `.cursor/rules/agent-next-action.mdc` or `.cursorrules`  
**Windsurf:** `.windsurfrules`  
**Cline:** `.clinerules`

```
# Agent Next Action — Mandatory Rule

After completing all tasks, before declaring done, always ask:
- header: "Next Action"
- question: "What would you like to do next?"
- 3 dynamically generated relevant options
- 4th option: "Other (please specify)"
No exceptions.
```

---

## Quick Install Prompt

Paste the following into any Agent (Copilot, Claude, Cursor, etc.) to auto-install:

```
Please read the following skill's README.md and complete the installation:
<YOUR_SKILL_DIR>/always-ask-next/README.md

Tell me which file was created and where it was placed.
```

> Replace `<YOUR_SKILL_DIR>` with the actual path, e.g. `c:\Users\user\.copilot\skills`.

---

## Directory Structure

```
always-ask-next/
├── SKILL.md    ← Skill definition (Agent execution logic)
└── README.md   ← Main documentation (installation guide, 繁體中文)
└── README_en.md ← This file (English version)
```
