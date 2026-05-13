---
title: Project Scaffolding
tags:
  - infrastructure
  - root
---

# Project Scaffolding

## Overview

קבצי תשתית בשורש הפרויקט. כוללים את `CLAUDE.md` שמגדיר את **ראובן** המנכ"ל, קבצי משתני סביבה (`.env` ו-`.env.example`), ו-`.gitignore`. אלה הקבצים הראשונים שטוענים בכל סשן Claude Code, ולכן הם הבסיס לכל ההתנהגות של המערכת.

## Owner / Belongs to

- **`CLAUDE.md`** → ראובן (CEO) — זה ה"מוח" שלו, הקובץ הזה הוא הוא.
- **`.env`** → מקומי בלבד, לא בגיט. שייך לסביבת ההרצה של המשתמש.
- **`.env.example`** → תבנית משותפת לכל מי שמשכפל את ה-repo.
- **`.gitignore`** → תשתית, חוקי git לפרויקט.

## Files

| קובץ | תיאור | קשור ל |
|---|---|---|
| `CLAUDE.md` | זהותו של ראובן: הצגה עצמית, רשימת הצוות (יעל/יובל/חן), מבנה `.claude/`. | [[claude-directory-layout]] |
| `.env` | משתני סביבה אמיתיים. gitignored. | – |
| `.env.example` | תבנית עם placeholders: `PROJECT_NAME`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`. | – |
| `.gitignore` | מתעלם מ-`.DS_Store`, `.claude/settings.local.json`, `.env*`, `.obsidian/`. | – |

## Open Questions

- האם להחליף את `OPENAI_API_KEY` ב-API key אחר כשיובל (מעצב התמונות) יוגדר?
- האם יידרש משתנה סביבה נוסף לחן (החוקרת) — למשל מפתח חיפוש (Tavily / Brave)?

## Session Log

### 2026-05-13 — initial scaffolding documented [shipped]
- **What was done:** תועדו קבצי השורש שיצרנו בסשנים הקודמים — `CLAUDE.md` (ראובן), `.env` / `.env.example`, `.gitignore`.
- **Decisions:** `.env` נשמר מחוץ ל-git; רק `.env.example` מתפרסם.
- **Notes / Caveats:** `.env` עדיין ריק — צריך מילוי ערכים אמיתיים בהמשך.
- **Related:** [[claude-directory-layout]], [[vault-bootstrap]]
