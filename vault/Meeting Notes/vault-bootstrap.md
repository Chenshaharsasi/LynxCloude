---
title: Vault Bootstrap
tags:
  - vault
  - meta
---

# Vault Bootstrap

## Overview

הקמת ה-`vault/` הראשונית של הפרויקט, לפי הסקיל `obsidian-vault-workflow`. ה-vault הוא הזיכרון ארוך-הטווח של Claude Code לפרויקט. נוצרו 4 תיקיות (Meeting Notes / Content Briefs / Publishing Log / Brand Guidelines), כל אחת עם `_index.md`, ו-5 topic files ראשונים שמתעדים את הקבצים הקיימים.

## Owner / Belongs to

**ראובן** — האחראי לתחזק את ה-vault, לקרוא לפני כל משימה ולכתוב אחרי.

## Bootstrap content

קבצים שנוצרו בסשן זה:

- `vault/Meeting Notes/_index.md`
- `vault/Meeting Notes/project-scaffolding.md`
- `vault/Meeting Notes/claude-directory-layout.md`
- `vault/Meeting Notes/superpowers-plugin.md`
- `vault/Meeting Notes/obsidian-skills.md`
- `vault/Meeting Notes/vault-bootstrap.md` (קובץ זה)
- `vault/Content Briefs/_index.md` (placeholder)
- `vault/Publishing Log/_index.md` (placeholder)
- `vault/Brand Guidelines/_index.md` (placeholder)

## Open Questions

- מתי יתחילו לזרום Content Briefs ו-Publishing Log? תלוי מתי יעל / יובל יוגדרו.
- האם להגדיר Brand Guidelines ראשוניות עכשיו, או להמתין שיהיה תוכן מוחשי?
- `volt/` הריקה שבשורש — האם תאוחד עם `vault/` בהמשך או תישמר נפרדת?

## Session Log

### 2026-05-13 — vault initialized [shipped]
- **What was done:** יצירת מבנה `vault/` (4 תיקיות + indexes), 5 topic files ב-Meeting Notes שמתעדים את הקבצים הקיימים. הגדרת SessionStart hook ל-`obsidian-vault-workflow`. הוספת `.obsidian/` ל-`.gitignore`.
- **Decisions:** רמת מפורטיות "קבצים ראשיים בלבד" — לא קובץ-פר-SKILL.md, אלא topic file לקבוצה לוגית. `volt/` הריקה לא נגעתי בה לפי בקשת המשתמש.
- **Notes / Caveats:** ה-vault נמצא ב-`vault/` (כמו שהסקיל מגדיר), לא ב-`volt/`. `.obsidian/` (הקונפיג של Obsidian) gitignored.
- **Related:** [[project-scaffolding]], [[claude-directory-layout]], [[superpowers-plugin]], [[obsidian-skills]]
