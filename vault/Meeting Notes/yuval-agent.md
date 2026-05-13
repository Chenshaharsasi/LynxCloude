---
title: Yuval — Image Designer Agent
tags:
  - agents
  - images
  - yuval
  - pipeline
---

# Yuval — Image Designer Agent

## Overview

יובל הוא ה-sub-agent השני של ראובן. תפקידו: להפיק תמונות בסגנון עקבי לכל הפרויקט, באמצעות OpenAI Images API (model `gpt-image-2`). הוא מקבל בקשת תמונה — או מהמשתמש ישירות, או דרך ה-pipeline של ראובן שמעביר אליו placeholders מסוג `{{IMAGE_NEEDED:...}}` שיעל השאירה במאמר. הוא סורק את `yuval/reference/` לסגנון, מנסח prompt באנגלית, מפעיל את הסקיל `gpt-image-gen`, ושומר תוצר ב-`yuval/outputs/`.

## Owner / Belongs to

**ראובן** — sub-agent שני (אחרי [[yael-agent]]). יובל קורא לסקיל `gpt-image-gen` אבל לא מפעיל סוכנים אחרים.

## Components

| רכיב | קובץ / תיקייה | תפקיד |
|---|---|---|
| הגדרת הסוכן | `.claude/agents/yuval.md` | frontmatter + system prompt; tools = Read, Write, Bash, Glob |
| הסקיל שיובל קורא לו | `.claude/skills/gpt-image-gen/SKILL.md` | מעטפת ל-OpenAI Images API (`gpt-image-2`); כולל primary call (jq) + fallback (Python) |
| reference | `yuval/reference/` | תמונות השראה לסגנון (ריק כרגע — `.gitkeep` בלבד) |
| outputs | `yuval/outputs/` | תוצרים: `<YYYY-MM-DD>-<slug>.png` + sibling `.txt` עם ה-prompt |
| משתנה סביבה | `.env` / `.env.example` | `OPENAI_API_KEY` — קיים מאז [[project-scaffolding]]; הוסר מהקטגוריה "optional" |

## Pipeline יעל → יובל

תועד גם ב-`CLAUDE.md` תחת "תהליך מאמר + תמונות":

1. יעל כותבת מאמר ב-`Output/`. תוך כתיבה — משאירה `{{IMAGE_NEEDED: "..."}}` גם ב-MD וגם ב-HTML (אותו טקסט).
2. יעל מחזירה לראובן: סיכום + רשימה ממוספרת של ה-placeholders.
3. ראובן מפעיל את יובל פעם אחת לכל placeholder.
4. יובל מייצר את התמונה ב-`yuval/outputs/`, מחזיר נתיב.
5. ראובן מחליף את ה-placeholders בקבצי יעל — ב-MD ל-`![alt](path)`, ב-HTML ל-`<img src="..." alt="..." />`. שומר את הגרסה הסופית.

## Open Questions

- מתי יוסיף המשתמש תמונות `reference/` אמיתיות? בלעדיהן יובל מייצר בלי "עוגן סגנוני".
- האם להוסיף Caching ל-prompts זהים כדי לחסוך קריאות API?
- האם להריץ את יובל ברקע (`run_in_background`) כשיש N>1 placeholders, כדי שראובן יכול להתחיל לשלב את הראשונות במקביל?
- מתי תוגדר [[chen]] (החוקרת)?

## Session Log

### 2026-05-13 — yuval, gpt-image-gen, and the yael→yuval pipeline shipped [shipped]
- **What was done:** נוצרו: `.claude/skills/gpt-image-gen/SKILL.md` (קריאה ל-`gpt-image-2`, primary curl+jq + fallback python), `.claude/agents/yuval.md` (workflow בן 7 שלבים, tools = Read/Write/Bash/Glob), `yuval/reference/` ו-`yuval/outputs/` (ריקות עם `.gitkeep`). עודכן `.claude/agents/yael.md` עם step 3.5 (image placeholders) ועם דרישה לרשימה ממוספרת ב-report. עודכן `CLAUDE.md` — triggers ליובל ברשימת הצוות, blocks routing ליובל, סקציה חדשה "תהליך מאמר + תמונות" עם 5 שלבי pipeline, ומבנה תיקיות מורחב. עודכן `.env.example` (OPENAI_API_KEY כבר לא "optional").
- **Decisions:** המודל `gpt-image-2` נשמר כפי שהמשתמש דרש — לא הוצעו אלטרנטיבות. ה-SKILL כולל גם fallback Python כי `jq` לא תמיד מותקן (Git Bash). placeholders של יעל הולכים גם ל-MD וגם ל-HTML (לא רק ל-MD כפי שהיה אפשר לקרוא בספציפיקציה הראשונית) — כדי שראובן יוכל לבצע substitution בטוח בשני הקבצים. שלב 5 ב-pipeline (substitution) הוא תמיד באחריות ראובן, לא של יובל או יעל.
- **Notes / Caveats:** יובל מוגדר אבל לא מופעל אוטומטית בהארנס הזה (ה-Agent tool רואה רק 6 סוכנים מובנים; ראה [[yael-agent#2026-05-13 — first article rewrite]]). הפעלה דרך CLI מקומי של Claude Code תזהה את הסוכן מה-`description`. עד שיהיו `reference/` תמונות אמיתיות, יובל יעבוד עם prompt ניטרלי.
- **Related:** [[yael-agent]], [[claude-directory-layout]], [[project-scaffolding]], [[obsidian-skills]]

### 2026-05-13 — first image generation [shipped]
- **What was done:** המשתמש מילא `OPENAI_API_KEY` ב-`.env` וביקש "תמונה של כלב". יובל הופעל (דרך fallback `general-purpose` עם ה-system prompt שלו), זיהה ש-`yuval/reference/` ריקה, ניסח prompt מקצועי באנגלית (Golden Retriever, golden-hour lighting, 85mm f/2.0, photorealistic), קרא ל-`gpt-image-gen` והפיק `yuval/outputs/2026-05-13-friendly-dog-portrait.png` (1024×1024, ~1.6MB, RGB 8-bit) + sibling `.txt` עם ה-prompt המלא (714 bytes).
- **Decisions:** ה-API call עבר ללא שגיאה בניסיון הראשון — כלומר `gpt-image-2` אכן זמין בחשבון של המשתמש (אישור empirical שהמודל קיים, כפי שהמשתמש הצהיר). בהיעדר reference, יובל בחר ב-Golden Retriever כברירת מחדל "visually appealing" — הבחירה עצמה היא debt: אם המשתמש ירצה סגנון אחר, צריך לפתוח reference/ או לפרט יותר בבקשה.
- **Notes / Caveats:** ה-API key נשמר במחשב המשתמש ב-`.env` (gitignored — לא הולך ל-GitHub). יובל לא הדפיס/הדליף את ערך ה-key. ה-prompt באנגלית (כפי שה-system prompt מורה) — אם המשתמש יבקש בעברית בעתיד, יובל יתרגם אוטומטית.
- **Related:** [[yael-agent#2026-05-13 — first article rewrite]]

### 2026-05-13 — first pipeline integration (image inserted into article) [shipped]
- **What was done:** ראובן שילב את תמונת הכלב למאמר ה-CRM של יעל כ-hero. ב-MD: `![alt](../yuval/outputs/2026-05-13-friendly-dog-portrait.png)` אחרי ה-H1. ב-HTML: `<img>` עם inline style (`max-width: 100%`, `border-radius: 8px`, `margin: 0 auto 2rem`) באותו מקום. השתמש בנתיב יחסי `../yuval/outputs/...` (לא העתקה ל-`Output/`) — single source של התמונה נשמר ב-`yuval/outputs/`.
- **Decisions:** Demo-mode בלבד — התמונה לא קשורה תוכנית למאמר (כלב vs CRM). ה-alt-text מציין במפורש "אינה קשורה לתוכן המאמר" כדי שלא תיווצר אשליה שזה אינטגרציה semantic. בהפעלה אמיתית של ה-pipeline (יעל כותבת עם `{{IMAGE_NEEDED:...}}` → יובל מייצר ל-prompts → ראובן ממיר ל-`<img>`/`![]()`), התוכן והתמונות יהיו תואמים.
- **Notes / Caveats:** ה-HTML עדיין לא מכיל `img` בלוק CSS — השתמשתי ב-inline style במקום. אם יהיו תמונות נוספות בעתיד באותו מאמר, שווה לעקור את ה-style ל-CSS block.
- **Related:** [[yael-agent#2026-05-13 — first article rewrite]]
