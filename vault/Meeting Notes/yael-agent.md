---
title: Yael — Content Writer Agent
tags:
  - agents
  - content
  - yael
---

# Yael — Content Writer Agent

## Overview

יעל היא ה-sub-agent הראשון של ראובן שמוגדר בפועל תחת `.claude/agents/`. תפקידה: לקחת מאמרי גלם מ-`Content/`, לקרוא את `yael/style-guide.md` ואת הדוגמאות ב-`yael/reference/`, ולהפיק שני תוצרים ל-`Output/` — גרסת Markdown וגרסת HTML self-contained עם inline CSS מינימלי. ראובן מנתב אליה כשהבקשה מכילה trigger keywords כמו "שכתב", "ערוך", "מאמר" (עברית) או "rewrite", "edit", "summarize" (אנגלית).

## Owner / Belongs to

**ראובן** — יעל היא sub-agent שלו, מופעלת אוטומטית דרך מנגנון ה-sub-agents של Claude Code על בסיס ה-`description` ב-frontmatter.

## Files

| קובץ / תיקייה | תפקיד |
|---|---|
| `.claude/agents/yael.md` | הגדרת הסוכנת — frontmatter + system prompt. הכלים: Read, Write, Edit, Glob, Grep. |
| `yael/style-guide.md` | מדריך הסגנון של הסטודיו. כרגע stub עם TODO — המשתמש ימלא בנפרד. |
| `yael/reference/` | דוגמאות לטקסטים בסגנון שלנו. כרגע ריק (`.gitkeep` בלבד). |
| `Content/` | מאמרי גלם נכנסים שממתינים לשכתוב. כרגע ריק (`.gitkeep` בלבד). |
| `Output/` | תוצרים משוכתבים — `<name>.md` + `<name>.html` לכל מאמר. כרגע ריק (`.gitkeep`). |
| `CLAUDE.md` | עודכן — נוספה סקציית "ניתוב" עם trigger keywords של יעל. |

## כללים מחייבים (system prompt)

- מסירה קישורים, CTAs והפניות חיצוניות של המחבר המקורי.
- שומרת מותגים שהם חלק מהסיפור (למשל "אני משתמש ב-Notion").
- לא מוסיפה קישורים חדשים, לא מקצרת ללא בקשה, לא ממציאה עובדות (במקום זה: `(להשלים)`).
- לא נוגעת ב-`yael/style-guide.md`, `yael/reference/`, או בקבצי `Content/` המקוריים.

## Open Questions

- מתי המשתמש ימלא את `yael/style-guide.md` והאם הוא יוסיף דוגמאות ל-`yael/reference/`?
- האם בעתיד נחליף את ה-inline CSS המינימלי בתבנית `yael/template.html` חיצונית?
- מתי יוגדרו [[yuval]] (מעצב התמונות) ו-[[chen]] (החוקרת)?

## Session Log

### 2026-05-13 — agent and scaffolding shipped [shipped]
- **What was done:** נוצר `.claude/agents/yael.md` עם frontmatter תקין (name=yael, description עם trigger keywords עבריים+אנגליים, tools = Read/Write/Edit/Glob/Grep) ו-system prompt מלא בעברית שמגדיר workflow בן 5 שלבים, כללי תפקיד, וגבולות. נוצרו תיקיות עבודה: `yael/style-guide.md` (stub), `yael/reference/` (`.gitkeep`), `Content/` (`.gitkeep`), `Output/` (`.gitkeep`). עודכן `CLAUDE.md` עם סקציית "ניתוב" שמתעדת את ה-trigger keywords של יעל.
- **Decisions:** style-guide.md נוצר כ-stub ולא מולא בתוכן — המשתמש יעשה את זה בנפרד. ה-HTML יהיה self-contained עם inline CSS (max-width 720px, RTL, system-ui), בלי תבנית חיצונית. ה-trigger keywords הוטמעו ב-`description` של ה-agent (המנגנון הסטנדרטי של Claude Code) וגם תועדו ב-CLAUDE.md לשקיפות.
- **Notes / Caveats:** יעל לא תוכל לפעול בפועל עד שיהיה תוכן ב-`Content/` ומדריך אמיתי ב-`style-guide.md`. ה-stub יחזיר אזהרה כשתקרא אותו (לפי הוראה ב-system prompt: "ציין לראובן והמשך").
- **Related:** [[claude-directory-layout]], [[project-scaffolding]]

### 2026-05-13 — trigger keywords surfaced in team list [shipped]
- **What was done:** ה-trigger keywords של יעל הוצמדו ישירות לשורה שלה ב-`## הצוות שלי` ב-CLAUDE.md, בנוסף להופעה הקיימת ב-`## ניתוב`. שתי שורות sub-bullet — אחת עברית, אחת אנגלית.
- **Decisions:** הכפילות בין `הצוות שלי` ל-`ניתוב` מכוונת — האחת נקראת כ-quick reference card, השנייה מסבירה איך המנגנון עובד. לא הוסר כלום.
- **Notes / Caveats:** המקור המחייב ל-routing נשאר ה-`description` ב-`.claude/agents/yael.md`; ה-CLAUDE.md הוא תיעוד אנושי.
- **Related:** [[claude-directory-layout]]
