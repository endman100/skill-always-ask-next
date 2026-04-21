# always-ask-next

> **⚠️ 強制規則** — 此 skill 定義了「完成任務後必須詢問下一步」的強制模式，不可跳過、不可例外。

[English version → README_en.md](README_en.md)

## 說明

`always-ask-next` 要求 Agent 在每次完成所有任務、宣告結束之前，必須呼叫 `AskUserQuestion`，動態生成 3 個後續行動選項供使用者選擇，避免 Agent 自行假設結束點或遺漏後續行動。

---

## 安裝方式（強制執行）

若只是將此 skill 放在知識庫中，Agent **不會自動強制執行**。需要按照以下方式安裝，才能確保每次對話都生效。

---

### GitHub Copilot（VS Code）— 推薦，全域生效

在 VS Code User Prompts 資料夾建立 `always-ask-next.instructions.md`，設定 `applyTo: "**"` 使其套用到所有工作區。

**Windows：**
```powershell
$dest = "$env:APPDATA\Code\User\prompts"
New-Item -ItemType Directory -Path $dest -Force | Out-Null
```

**macOS / Linux：**
```bash
mkdir -p "$HOME/Library/Application Support/Code/User/prompts"
# Linux:
# mkdir -p "$HOME/.config/Code/User/prompts"
```

在該資料夾內建立 `always-ask-next.instructions.md`，內容如下：

````markdown
---
applyTo: "**"
---

# 強制規則：完成任務後必須詢問下一步（always-ask-next）

每次 **完成所有當前任務後、宣告結束前**，必須執行以下步驟：

1. 呼叫 `AskUserQuestion`（或等效互動工具）
2. `header` 固定為：`"Next Action"`
3. `question` 固定為：`"What would you like to do next?"`
4. 根據當前執行脈絡，**動態生成 3 個最相關的後續行動選項**

⚠️ 此規則為強制規則，不可跳過，不可假設使用者不需要後續行動。

## Skill 路徑

`<YOUR_SKILL_DIR>/always-ask-next/SKILL.md`
````

> 將 `<YOUR_SKILL_DIR>` 替換為你的 skill 知識庫實際路徑。

---

### Claude（claude.ai / API system prompt）

在 system prompt 最前面加入：

```
Additional rule:
After completing all tasks, before declaring done, you MUST call AskUserQuestion with:
- header: "Next Action"
- question: "What would you like to do next?"
- 3 dynamically generated relevant next action options based on current context
This rule is mandatory. No exceptions.
```

---

### Cursor / Windsurf / Cline — Rules 檔案

**Cursor：** `.cursor/rules/agent-next-action.mdc` 或 `.cursorrules`  
**Windsurf：** `.windsurfrules`  
**Cline：** `.clinerules`

```
# Agent Next Action — Mandatory Rule

After completing all tasks, before declaring done, always ask:
- header: "Next Action"
- question: "What would you like to do next?"
- 3 dynamically generated relevant options
No exceptions.
```

---

## 快速安裝提示詞

貼入任意 Agent（Copilot、Claude、Cursor 等）即可自動完成安裝：

```
請讀取以下 skill 的 README.md 並完成安裝：
<YOUR_SKILL_DIR>/always-ask-next/README.md

告訴我建立了哪個檔案以及放置的位置。
```

> 將 `<YOUR_SKILL_DIR>` 替換為實際路徑，例如 `c:\Users\user\.copilot\skills`。

---

## 目錄結構

```
always-ask-next/
├── SKILL.md    ← Skill 定義（Agent 執行邏輯）
└── README.md   ← 本文件（安裝說明）
```
