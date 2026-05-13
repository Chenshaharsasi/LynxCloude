---
title: Reuven Redefined — Lynx Studio Manager
tags:
  - meta
  - reuven
  - studio
  - team
---

# Reuven Redefined — Lynx Studio Manager

## Overview

ראובן הוגדר מחדש בסשן הזה. עד עכשיו הוא היה "מנכ"ל צוות תוכן גנרי" עם 3 sub-agents (יעל / יובל / חן). מעכשיו הוא **מנהל סטודיו לינקס אשקלון** — סטודיו לכושר פונקציונלי / CrossFit / הרמת כושר, עם 185 מנויים פעילים ויעד צמיחה ל-250. הצוות הורחב ל-6 sub-agents — שלושה חדשים (שירה / רוני / אורי) שמכסים את התפקידים האופרציוניים (שירות לקוחות / תקציב / תוכנית אימונים), והשלושה הקיימים reframed לסטודיו (יעל+יובל = שיווק וסושיאל, חן = מחקר רוחבי). נוצרה תיקיית `data/` עם schema לכל קובץ (CRM, schedule, budget, revenue) ו-PII כראוי gitignored.

## Owner / Belongs to

**ראובן** עצמו. כל הפרויקט בעצם.

## Team composition (post-redefinition)

| תפקיד | סוכן | סטטוס |
|---|---|---|
| שיווק וסושיאל (כתיבה) | [[yael-agent]] | reframed |
| שיווק וסושיאל (ויזואל) | [[yuval-agent]] | reframed |
| מחקר חיצוני | [[chen-agent]] | reframed (cross-functional) |
| שירות לקוחות | **שירה** | חדש |
| מנהלת התקציב | **רוני** | חדש |
| תוכנית אימונים | **אורי** | חדש |

## Workflows defined (in CLAUDE.md)

1. **קמפיין שיווקי** — chen → yael → yuval → ראובן משלב
2. **פנייה ממנוי** — שירה (טמפלייט + CRM)
3. **דוח תקציב חודשי** — רוני (Bash-driven KPI calc)
4. **תכנון לוח שיעורים** — אורי (class-catalog + instructors + capacity logic)
5. **קמפיין retention** — שירה identifies → רוני approves ROI → יעל drafts → שירה sends

## Data ingestion

- `data/members.csv` — gitignored (PII)
- `data/schedule.csv` — committed
- `data/budget.csv` — gitignored (financial)
- `data/revenue.csv` — gitignored (financial)

`.example` files (headers only) קיימים לכל אחד.

## Open Questions

- **מתי המשתמשת תייבא את הנתונים האמיתיים?** הסוכנים שירה / רוני / אורי תלויים בקבצי ה-data שיהיו תקפים. עד אז — workflows גנריים בלבד.
- **איך נמדוד התקדמות ליעד 250?** רוני יכולה לבנות `data/revenue.csv` עם trendline — צריך KPI dashboard / מד הוצאה חודשי שיציג כמה אנחנו רחוקים. דוח חודשי ראשון יכול להיות ה-baseline.
- **Tier 2 agents** (sales/conversion, data analyst, local-SEO Ashkelon) — המשתמשת ביקשה לא עכשיו, להתמקד ב-4 הקיימים. אבל אם 250 לא יושג בקצב — אלה ה-candidates הבאים.
- **תוכן ל-`shira/templates/`** ול-`ori/class-catalog.md` / `ori/instructors.md` — stubs כעת. בסשן עתידי לעבור על כל אחד עם המשתמשת.
- **`## הערה` הישנה ב-CLAUDE.md** הוסרה ב-rewrite — כעת אין סקציה outdated.
- **PII compliance** — `data/members.csv` gitignored, אבל אם הסוכנים יוצרים דוחות עם שמות, צריך פרוטוקול לא להעלות אותם. נוסיף ל-`shira/responses/` ול-`roni/reports/` הערה ב-gitignore? (פתוח להחלטה בסשן הבא).

## Session Log

### 2026-05-13 — Reuven shipped as Studio Manager [shipped]
- **What was done:** rewrite מלא של `CLAUDE.md` — זהות חדשה (מנהל סטודיו), 6 סוכנים ברשימה עם triggers, 5 workflows מעורבים, מבנה תיקיות מורחב. נוצרו 3 סוכנים חדשים: `.claude/agents/shira.md` (Read/Write/Edit/Glob/Grep — customer service), `.claude/agents/roni.md` (+Bash — finance), `.claude/agents/ori.md` (Read/Write/Edit/Glob/Grep — schedule planning). נוצרו תיקיות עבודה: `shira/templates/`, `shira/responses/`, `roni/reports/`, `roni/forecasts/`, `ori/schedules/`. נוצרו stubs: `ori/class-catalog.md`, `ori/instructors.md`. נוצרה תיקיית `data/` עם `README.md` ו-4 קבצי `.example`. `.gitignore` עודכן ל-PII patterns (`data/members.csv`, `data/budget.csv`, `data/revenue.csv`, `data/*.private.csv`). יעל / יובל / חן קיבלו section "Studio Context" קצר בסוף ה-system prompt שלהם.
- **Decisions:** שמות הסוכנים החדשים — שירה (שירות), רוני (תקציב, gender-neutral name משום ש"מנהלת" feminine, רוני עובד טוב לשני המינים בעברית), אורי (אימונים, masculine fits CrossFit culture). חן נשארת cross-functional ולא משויכת לתפקיד יחיד. רוני היחידה עם Bash (לעיבוד CSV); שאר הסוכנים החדשים בלי. הנתונים האמיתיים של הסטודיו לא יובאו בסשן הזה — רק schema + `.example` files. ה-`## הערה` הישנה ב-CLAUDE.md הוסרה לגמרי ב-rewrite.
- **Notes / Caveats:** הסוכנים החדשים תלויים בנתונים אמיתיים ב-`data/` כדי להיות שימושיים. עד שייבוא יקרה, workflows כמו "פנייה ממנוי" יעבדו רק כקונספט. שירה / רוני / אורי לא יזוהו אוטומטית בהארנס הזה (כמו שאר הסוכנים) — דרך `general-purpose` fallback. **Tier 2 agents (sales/analytics/local-SEO) נדחו** — המשתמשת ביקשה להתמקד ב-4 התפקידים הקיימים.
- **Related:** [[yael-agent]], [[yuval-agent]], [[chen-agent]], [[claude-directory-layout]], [[project-scaffolding]]
