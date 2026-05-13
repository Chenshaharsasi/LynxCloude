---
title: Superpowers Plugin
tags:
  - skills
  - third-party
source: obra/superpowers
---

# Superpowers Plugin

## Overview

14 skills שהותקנו מ-`obra/superpowers` ([github](https://github.com/obra/superpowers)) @ commit `f2cbfbe`. ההתקנה נעשתה ידנית (clone + cp) כי מערכת `/plugin` לא היתה זמינה. הסקילים יושבים ב-`.claude/skills/<name>/SKILL.md`. אלה סקילים גנריים לפיתוח: brainstorming, planning, debugging, code review, TDD, וכו'.

## Owner / Belongs to

כרגע — **ראובן** (היחיד שיכול להפעיל סקילים בסשנים נוכחיים). בעתיד ייתכן שחלקם ינותבו ל-sub-agents (למשל TDD ל-coding agent).

## Skills

| Skill | תיאור קצר |
|---|---|
| `brainstorming` | סשני brainstorming מובנים |
| `dispatching-parallel-agents` | הפעלת sub-agents במקביל |
| `executing-plans` | ביצוע תוכנית מסודרת |
| `finishing-a-development-branch` | סגירה נכונה של ענף פיתוח |
| `receiving-code-review` | קבלת ביקורת קוד |
| `requesting-code-review` | בקשת ביקורת קוד |
| `subagent-driven-development` | פיתוח מבוסס sub-agents |
| `systematic-debugging` | debugging שיטתי |
| `test-driven-development` | TDD |
| `using-git-worktrees` | עבודה עם worktrees |
| `using-superpowers` | meta-skill לשימוש בכל ה-skills |
| `verification-before-completion` | אימות לפני הצהרה על "סיום" |
| `writing-plans` | כתיבת תוכניות |
| `writing-skills` | כתיבת סקילים חדשים |

## Open Questions

- האם להוסיף hook ש-`using-superpowers` ייטען אוטומטית בכל סשן?
- האם להתאים את `requesting-code-review` למודל של "מעצב מבקש מהכותב" וכו' בתוך הצוות?

## Session Log

### 2026-05-13 — installed and documented [shipped]
- **What was done:** clone + cp של 14 הסקילים מ-`obra/superpowers` (`f2cbfbe`) ל-`.claude/skills/`. כתיבת מסמך זה.
- **Decisions:** התקנה ידנית בלבד (לא `/plugin`), בלי דריסת קבצים קיימים (`cp -Rn`).
- **Notes / Caveats:** ה-upstream לא מכיל `commands/` או `agents/` — רק `skills/`.
- **Related:** [[claude-directory-layout]], [[vault-bootstrap]]
