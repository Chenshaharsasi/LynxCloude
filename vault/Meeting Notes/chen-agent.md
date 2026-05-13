---
title: Chen — Web Researcher Agent
tags:
  - agents
  - research
  - chen
  - pipeline
---

# Chen — Web Researcher Agent

## Overview

חן היא ה-sub-agent השלישי של ראובן ומשלימה את הצוות (אחרי [[yael-agent]] ו-[[yuval-agent]]). תפקידה: למצוא חומר גלם איכותי ברשת ולהכין אותו כקלט אפשרי ליעל. היא היחידה בצוות עם גישה לאינטרנט (`WebSearch` + `WebFetch`) — כלומר היא היחידה שמביאה מידע עכשווי וממוקד לפרויקט, במקום להישען על ידע פנימי של ה-LLM שיכול להיות ישן או מומצא.

נקודה ארכיטקטונית מפתח: **חן לא מפעילה את יעל ישירות**. היא רק שומרת ב-`Content/` ומדווחת לראובן. ההחלטה מה לעשות הלאה (לעצור / להמשיך) היא של ראובן בלבד.

## Owner / Belongs to

**ראובן** — sub-agent שלישי. עם הוספתה, ה-Pipeline המלא (תרחיש A) מאפשר זרימה אוטומטית של "מצא + שכתב + תמונות" משימוש אחד.

## Components

| רכיב | קובץ / תיקייה | תפקיד |
|---|---|---|
| הגדרת הסוכנת | `.claude/agents/chen.md` | frontmatter + system prompt; tools = WebSearch, WebFetch, Read, Write, Edit, Glob, Grep |
| זיכרון חיפושים | `chen/Memory/searches.md` | append-only log; `Grep` עליו לפני כל חיפוש; window של 30 ימים |
| יעד שמירה | `Content/<YYYY-MM-DD>-<slug>.md` | פורמט עם frontmatter (`source`, `fetched`, `title`) ואז גוף המאמר |
| pipeline integration | `CLAUDE.md` | סקציית "תהליך תוכן" עודכנה עם 4 תרחישים — A (full), B (research only), C (rewrite only), D (image only) |

## Memory protocol

- **לפני כל חיפוש**: `Grep -B 5` על מילות המפתח ב-`chen/Memory/searches.md`. מאתר entries קיימים יחד עם ה-`## YYYY-MM-DD HH:MM | ...` שמעליהם.
- **window**: 30 ימים. אם entry < 30 יום וגם הנושא לא דינמי → מחזירה את הקיים ושואלת.
- **נושאים דינמיים** (תמיד re-search גם בתוך החלון): חדשות, מחירים, סטטיסטיקות עדכניות, ביצועי מודלים, מספרי משתמשים.
- **אחרי כל חיפוש**: append entry בפורמט הקבוע (תאריך+שעה, מילות מפתח, שאילתות, מקורות עם דירוג ⭐, הנבחר ולמה, שם הקובץ ב-Content).

## Quality criteria

| Pass ✅ | Reject ❌ |
|---|---|
| מקור ראשוני (מחקר / אתר רשמי / בלוג חברה) | אגרגטורים, listicle sites |
| פרסום מקצועי (Anthropic, OpenAI, TechCrunch, וכד') | פורומים (אלא כ-primary source) |
| 12 חודשים אחרונים (אלא אם evergreen) | clickbait, headlines מטעות |
| שפה תואמת לקהל (עברית אם רלוונטי) | AI-generated content גנרי |

אם אף מועמד לא עובר — חן לא ממציאה. מחזירה לראובן ש-לא נמצא מקור איכותי ומציעה rephrasing.

## Open Questions

- ה-window של 30 ימים hardcoded. האם להוסיף configurable threshold (ב-`.env` או ב-`settings.json`)?
- מה לעשות עם paywall content? כרגע: לדלג ולחפש אלטרנטיבה. אולי בעתיד — לרשום ב-Memory עם פירוט "Pay-walled, אבל הנה ה-abstract הציבורי".
- האם להוסיף rate-limit awareness ל-WebFetch? כרגע נשענים על שגיאות runtime.
- הסקציה `## הערה` ב-CLAUDE.md outdated (מבטיחה דברים שכבר נעשו) — האם למחוק / להחליף ב-"## TODO" עם open items?
- כשחן תתחיל לרוץ הרבה, האם `searches.md` יגדל מדי? אולי כדאי rotation לפי שנה (`searches-2026.md`, `searches-2027.md`).

## Session Log

### 2026-05-13 — chen shipped, 4-scenario pipeline live [shipped]
- **What was done:** נוצר `.claude/agents/chen.md` עם frontmatter תקין (tools: WebSearch, WebFetch, Read, Write, Edit, Glob, Grep) ו-system prompt בעברית: זהות, workflow בן 7 שלבים, פרוטוקול memory מפורט, קריטריוני איכות, וגבולות. נוצר `chen/Memory/searches.md` כ-stub עם header. עודכן `CLAUDE.md` ב-4 עריכות: triggers ליד חן ב-`## הצוות שלי`, routing block חדש ב-`## ניתוב`, החלפת `## תהליך מאמר + תמונות` בסקציה רחבה יותר `## תהליך תוכן` עם 4 תרחישים (A=full, B=research-only, C=rewrite-only, D=image-only), הוספת `chen/Memory/` ל-`## מבנה התיקיות`.
- **Decisions:** חן **לא** מפעילה את יעל ישירות — דבק בעיקרון ש-orchestration היא של ראובן בלבד. ה-window הוא 30 ימים hardcoded לעת עתה. נושאים דינמיים (חדשות/מחירים/סטטיסטיקות) תמיד re-search גם בתוך החלון. slug של קבצי `Content/` באנגלית (filesystem-safe) — הגוף בשפת המקור. הסקציה הישנה `## תהליך מאמר + תמונות` הוחלפה ב-`## תהליך תוכן` כדי לתת מקום ל-4 תרחישים, בלי לאבד את ה-yael→yuval flow (הוא תרחיש C כעת).
- **Notes / Caveats:** אותו הגבל כמו yael/yuval — ה-Agent tool של ההארנס לא רואה sub-agents מ-`.claude/agents/`. הפעלת חן בצ'אט הזה תיעשה דרך `general-purpose` fallback עם ה-system prompt שלה מוטמע. CLI מקומי יזהה אוטומטית מה-`description`.
- **Related:** [[yael-agent]], [[yuval-agent]], [[claude-directory-layout]], [[project-scaffolding]]
