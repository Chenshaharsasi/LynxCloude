---
title: Obsidian Skills
tags:
  - skills
  - obsidian
  - workflow
---

# Obsidian Skills

## Overview

3 סקילים שהמשתמש הוסיף תחת `.claude/skills/` כדי לכפות workflow של Obsidian vault — קריאת topic file לפני משימה, וכתיבת session log אחריה. הסקיל `obsidian-vault-workflow` הוא ה"חוק" המרכזי; השניים האחרים מספקים syntax helpers (markdown, bases).

## Owner / Belongs to

**ראובן** — הוא צריך להפעיל את `obsidian-vault-workflow` בתחילת ובסוף כל משימה. בעתיד גם כל sub-agent ירש את החובה הזאת.

## Skills

| Skill | תיאור | מתי משתמשים |
|---|---|---|
| `obsidian-vault-workflow` | מגדיר פרוטוקול ה-vault: קריאת topic + Meeting Notes + Brand Guidelines לפני המשימה; כתיבת session log עם status + Open Questions + Related wikilinks אחרי. | **כל משימה**, פרט לשאלות read-only טהורות. |
| `obsidian-markdown` | תחביר Obsidian: wikilinks, embeds, callouts, frontmatter, tags, footnotes, Mermaid. | בעבודה על קבצי `.md` ב-vault. |
| `obsidian-bases` | יצירת קבצי `.base` — תצוגות database-like (table / cards / list / map) עם filters ו-formulas. | אם נרצה תצוגות מסוננות של כל ה-topic files (למשל "כל ה-WIP"). |

## Open Questions

- האם להוסיף `.base` file לסיכום סטטוס כל ה-topics?
- האם להגדיר template מובנה ל-Content Briefs שיתאים לעבודה של יעל?

## Session Log

### 2026-05-13 — vault bootstrapped using this skill [shipped]
- **What was done:** המשתמש הוסיף את 3 הסקילים תחת `.claude/skills/`. בסשן זה השתמשתי בפעם הראשונה ב-`obsidian-vault-workflow` כדי לבנות את `vault/`.
- **Decisions:** ה-vault מתחיל עם 4 תיקיות (Meeting Notes, Content Briefs, Publishing Log, Brand Guidelines). תוכן ראשוני ב-Meeting Notes בלבד.
- **Notes / Caveats:** הוגדר hook ב-`.claude/settings.json` ל-SessionStart שיזכיר את הסקיל בכל סשן חדש.
- **Related:** [[vault-bootstrap]], [[claude-directory-layout]]
