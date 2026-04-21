---
name: always-ask-next
description: Always ask the user what to do next before finishing all tasks. Use AskUserQuestion with 3 dynamically generated relevant suggestions". ⚠️ This is a MANDATORY rule — must always be followed without exception.
metadata:
  {
    "openclaw":
      {
        "emoji": "➡️",
      },
  }
---

# Always Ask Next

## 此 Skill 適用於

在 Agent 完成所有任務之前，強制詢問使用者接下來想做什麼，避免 Agent 自行假設結束點或遺漏後續行動。

---

## 規則

**Before finishing all tasks, always use one AskUserQuestion to ask the user what to do next.**

**Question — What would you like to do next?** (header: "Next Action"):
Dynamically generate and suggest 3 most relevant next actions based on the current context.

---

## 執行方式

1. 在完成當前所有任務後、宣告完成前，呼叫 `AskUserQuestion`
2. `header` 固定為：`"Next Action"`
3. 根據當前的執行脈絡，動態推斷並生成 3 個最相關的後續行動選項
4. 等待使用者回應後再繼續

---

## 範例

```
AskUserQuestion(
  header: "Next Action",
  question: "What would you like to do next?",
  options: [
    "<動態生成選項 1，基於當前任務脈絡>",
    "<動態生成選項 2，基於當前任務脈絡>",
    "<動態生成選項 3，基於當前任務脈絡>",
  ]
)
```
