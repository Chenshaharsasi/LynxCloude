---
title: .claude Directory Layout
tags:
  - infrastructure
  - claude-config
---

# .claude/ Directory Layout

## Overview

תחת `.claude/` יושב הקונפיג של Claude Code לפרויקט. שלוש תיקיות עיקריות: `agents/`, `commands/`, `skills/`. כרגע `agents/` ו-`commands/` ריקים (רק `.gitkeep`); `skills/` מאוכלס ב-17 סקילים (14 מ-Superpowers + 3 Obsidian).

## Owner / Belongs to

- **`agents/`** → ראובן — ה-sub-agents שלו (יעל / יובל / חן) יחיו פה כשיוגדרו.
- **`commands/`** → ראובן — פקודות workflow מותאמות.
- **`skills/`** → משאב משותף — כרגע נצרך ע"י ראובן (אין עדיין sub-agents).
- **`settings.local.json`** → מקומי למחשב, gitignored.
- **`settings.json`** → תצורת hook לפרויקט (משותף, ב-git).

## Subdirs

| תיקייה | מצב | תיאור |
|---|---|---|
| `agents/` | ריקה (`.gitkeep`) | יוגדרו כאן: [[yael]], [[yuval]], [[chen]] (בהמשך הסדנה) |
| `commands/` | ריקה (`.gitkeep`) | פקודות workflow ייווצרו בהמשך |
| `skills/` | 17 סקילים | ראה [[superpowers-plugin]] + [[obsidian-skills]] |

## Open Questions

- מתי יוגדרו יעל / יובל / חן כ-agents? איזה מבנה (frontmatter + system prompt)?
- האם חלק מהסקילים שמיועדים ל-Superpowers צריכים להישאר רק לראובן או להיות נגישים לכל sub-agent?

## Session Log

### 2026-05-13 — initial layout documented [shipped]
- **What was done:** תיעוד מבנה `.claude/` — `agents/` (ריק), `commands/` (ריק), `skills/` (17 סקילים).
- **Decisions:** התשתית עומדת לפני שהסוכנים מוגדרים — מכוון, לפי תוכנית הסדנה.
- **Notes / Caveats:** `.gitkeep` נשמר בכל תיקייה ריקה כדי שgit ישמור אותה.
- **Related:** [[project-scaffolding]], [[superpowers-plugin]], [[obsidian-skills]]
