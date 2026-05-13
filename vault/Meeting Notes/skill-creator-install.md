---
title: skill-creator Install
tags:
  - skills
  - plugins
  - third-party
source: anthropics/skills
---

# skill-creator Install

## Overview

הותקן `example-skills` ב-`scope: project` דרך Claude Code CLI — חבילה רשמית של Anthropic ([anthropics/skills](https://github.com/anthropics/skills)) שמכילה 12 skills, ביניהם **`skill-creator`** (היעד שביקש המשתמש). `skill-creator` לא קיים כ-plugin עצמאי ב-marketplace; הוא רק skill בתוך `example-skills`. ההתקנה ב-project scope נרשמת ב-`.claude/settings.json` (קובץ ב-git) — לכן ההגדרה משותפת לכל מי שמשכפל את ה-repo.

## Owner / Belongs to

כרגע — **ראובן**. בעתיד `skill-creator` יוכל לשמש כל sub-agent שיצטרך לבנות סקיל חדש (למשל אם יעל תרצה template לסקיל כתיבה ייעודי).

## Skills שהתווספו דרך `example-skills`

| Skill | רלוונטיות לצוות |
|---|---|
| `skill-creator` | **היעד** — יצירת skills חדשים |
| `brand-guidelines` | רלוונטי ליובל / כללי המותג |
| `frontend-design` | – |
| `canvas-design` | – |
| `algorithmic-art` | – |
| `mcp-builder` | אם נרצה MCP מותאם |
| `doc-coauthoring` | רלוונטי ליעל |
| `internal-comms` | רלוונטי ליעל |
| `slack-gif-creator` | – |
| `theme-factory` | – |
| `web-artifacts-builder` | – |
| `webapp-testing` | – |

## Configuration

נוסף ל-`.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "anthropic-agent-skills": {
      "source": { "source": "github", "repo": "anthropics/skills" }
    }
  },
  "enabledPlugins": {
    "example-skills@anthropic-agent-skills": true
  }
}
```

ה-marketplace רשום כ-`anthropic-agent-skills` (השם מ-`marketplace.json` של ה-repo, לא מה-GitHub slug).

## Open Questions

- האם להשבית skills שלא רלוונטיים לצוות (slack-gif-creator, algorithmic-art, theme-factory) דרך `skillOverrides`?
- האם להתקין גם את `document-skills` (pptx/docx/pdf/xlsx) ב-project scope? כרגע הם זמינים ברמת המשתמש בלבד.

## Session Log

### 2026-05-13 — installed via plugin system [shipped]
- **What was done:** ניסיון 1 (`skill-creator@claude-plugins-official`) נכשל — לא ב-marketplace. ניסיון 2 דילגנו (ה-marketplace כבר רשום). ניסיון 3: `marketplace add anthropics/skills` (נרשם בשם `anthropic-agent-skills`) ואז `plugin install skill-creator@anthropic-agent-skills` נכשל ("not found"). בדיקת `marketplace.json` הראתה ש-skill-creator הוא skill בתוך plugin `example-skills`, לא plugin עצמאי. ההתקנה הסופית: `plugin install example-skills@anthropic-agent-skills --scope project`.
- **Decisions:** הותקנה החבילה המלאה (12 skills) ולא העתקה נקודתית של `skill-creator`, כדי להישאר במסלול ה-plugin הרשמי וכדי שניתן יהיה לעדכן דרך `claude plugin update`. גם הותקן Claude Code CLI (`@anthropic-ai/claude-code` v2.1.140) דרך `sudo npm install -g`.
- **Notes / Caveats:** ה-CLI הותקן ע"י המשתמש בטרמינל נפרד (sudo דרש סיסמה אינטראקטיבית). `claude plugin list` מאשר scope=project, status=enabled.
- **Related:** [[superpowers-plugin]], [[claude-directory-layout]]
