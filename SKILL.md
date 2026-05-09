---
name: vault-reporting-conventions
description: "Authoritative reference for writing recurring operational reports to the Deltek Obsidian vault. Use this skill whenever you need to know where a report file goes, what to name it, what frontmatter to write, how to link it from today's daily note, or which vault sections to scan for inputs. Covers Weekly Executive Briefing, Weekly Project Health Check, Action Tracker, Routine Digest, and LinkedIn Content Brief. Always invoke before writing any companion file or vault report so paths, naming, and frontmatter stay consistent across runs. Distinct from para-memory-files (which manages agent PARA memory) — this covers the shared Obsidian knowledge base used by Matt."
---

# Vault Reporting Conventions

This skill is the single source of truth for output conventions when writing recurring reports
to the Deltek vault. Use it at the start of any reporting routine.

---

## Vault Root & Key Folders

| Path (relative to vault root) | Purpose |
|-------------------------------|---------|
| `_Daily/YYYY-MM-DD.md` | Daily notes — one per calendar day |
| `_Daily/Companions/` | Companion reports linked from daily notes |
| `01_Projects/` | Project folders; each has `00 - Index.md` |
| `02_Reference/LinkedIn Posts/` | Published and draft LinkedIn posts |
| `03_Meetings/` | Non-project meeting notes |
| `00_Meta/Templates/` | Obsidian note templates |
| `_Inbox/` | Unprocessed items awaiting filing |
| `System/` | Agent config — do not modify |

**Vault root (absolute path):**
```
/Users/matt/Library/Mobile Documents/iCloud~md~obsidian/Documents/Deltek
```

Use `obsidian-cli` with `vault=Deltek` for all reads and writes (never write raw files directly when `obsidian-cli` is available — it handles sync and conflict resolution).

---

## Report Types: Naming, Path, Frontmatter, and Linking

### 1. Weekly Executive Briefing

**When:** Produced once per ISO week, typically on Sunday or Monday covering the prior Mon–Sun period.

**Companion path:**
```
_Daily/Companions/YYYY-Wnn Executive Briefing.md
```
Example: `_Daily/Companions/2026-W18 Executive Briefing.md`

**Frontmatter:**
```yaml
---
title: "Wnn Executive Briefing"
date: YYYY-MM-DD        # date compiled, not start of week
tags:
  - briefing
  - weekly
  - executive
status: active
created-by: claude
agent-role: operations-analyst
---
```

**Body opening (immediately after H1):**
```markdown
**Reporting period:** YYYY-MM-DD to YYYY-MM-DD
**Prepared:** YYYY-MM-DD
```

**Daily note section to add** (insert after the `# YYYY-MM-DD` heading and any callouts, before `## Open Actions from Meetings`):
```markdown
## Weekly Executive Briefing

[[_Daily/Companions/YYYY-Wnn Executive Briefing|Wnn Executive Briefing]] — compiled YYYY-MM-DD by Operations Analyst

---
```

**Input scan paths:**

| Source | What to read |
|--------|-------------|
| `01_Projects/*/00 - Index.md` | Project status, tier, active risks for each project |
| `03_Meetings/YYYY-MM-DD*.md` | All non-project meetings during the reporting period |
| `01_Projects/*/Meetings/YYYY-MM-DD*.md` | Project-specific meetings during the reporting period |
| `01_Projects/*/ADRs/` | Any new architecture decisions this week |
| `_Daily/YYYY-MM-DD.md` (Mon–Sun) | Daily context, actions, and priorities from the week |

---

### 2. Weekly Project Health Check

**When:** Produced once per ISO week alongside or just after the Executive Briefing.

**Companion path:**
```
_Daily/Companions/YYYY-Wnn Project Health Check.md
```
Example: `_Daily/Companions/2026-W18 Project Health Check.md`

**Frontmatter:**
```yaml
---
title: "Wnn Project Health Check"
date: YYYY-MM-DD
tags:
  - health-check
  - weekly
  - projects
status: active
created-by: claude
agent-role: operations-analyst
---
```

**Daily note section to add** (after Executive Briefing section if present, otherwise same insertion point):
```markdown
## Weekly Project Health Check

[[_Daily/Companions/YYYY-Wnn Project Health Check|Wnn Project Health Check]] — compiled YYYY-MM-DD by Operations Analyst

---
```

**Input scan paths:**

| Source | What to read |
|--------|-------------|
| `01_Projects/*/00 - Index.md` | Status, tier, last-updated date for every active project |
| `01_Projects/*/Meetings/YYYY-MM-DD*.md` | Any new meetings or decisions this week per project |
| `01_Projects/*/ADRs/` | New or recently changed ADRs |
| Prior week's Project Health Check companion | Carry forward unresolved risks and blockers |

---

### 3. Action Tracker

**When:** Produced as needed (often daily or after heavy meeting days) by the Operations Analyst's daily heartbeat routine.

**Companion path:**
```
_Daily/Companions/YYYY-MM-DD - Action Tracker.md
```
Example: `_Daily/Companions/2026-04-27 - Action Tracker.md`

**Frontmatter:**
```yaml
---
title: "Action Tracker YYYY-MM-DD"
date: YYYY-MM-DD
tags:
  - actions
  - tracker
status: active
created-by: claude
agent-role: operations-analyst
---
```

**Daily note inline reference** (within the Open Actions section summary line):
```markdown
Full cross-team tracker: [[_Daily/Companions/YYYY-MM-DD - Action Tracker]]
```

**Input scan paths:**

| Source | What to read |
|--------|-------------|
| `03_Meetings/YYYY-MM-DD*.md` | Actions extracted from recent meetings |
| `01_Projects/*/Meetings/YYYY-MM-DD*.md` | Project-specific meeting actions |
| Prior Action Tracker companion | Carry-forward open actions |

---

### 4. Routine Digest

**When:** Produced by the daily heartbeat routine to summarise what the agent ran during the day.

**Companion path:**
```
_Daily/Companions/YYYY-MM-DD - Routine Digest.md
```
Example: `_Daily/Companions/2026-04-10 - Routine Digest.md`

**Frontmatter:**
```yaml
---
title: "Routine Digest YYYY-MM-DD"
date: YYYY-MM-DD
tags:
  - digest
  - routines
status: active
created-by: claude
agent-role: operations-analyst
---
```

**Daily note inline reference:**
```markdown
Routine digest: [[_Daily/Companions/YYYY-MM-DD - Routine Digest]]
```

---

### 5. LinkedIn Content Brief

**When:** Produced weekly (Sundays) by the Document Writer running the vault-linkedin-brief skill.

> **Note:** The full authoring workflow is in the `vault-linkedin-brief` skill. Use that skill for the full pipeline. The conventions below apply when you only need to know the save path.

**Save path (not a Companion — goes to Reference):**
```
02_Reference/LinkedIn Posts/YYYY-MM-DD - Short Topic.md
```
Example: `02_Reference/LinkedIn Posts/2026-05-11 - AI Programmes Need Two Owners.md`

**Frontmatter:**
```yaml
---
tags:
  - linkedin
  - content-pillar/<pillar-slug>
status: draft
date: YYYY-MM-DD     # the Sunday post_date
created-by: agent/document-writer
---
```

**No daily note section is added** for LinkedIn briefs — they are standalone drafts filed in Reference.

---

## Daily Note Update Protocol

When you need to add a companion link to today's daily note:

1. Read the current daily note: `_Daily/YYYY-MM-DD.md`
2. Locate the insertion point:
   - For briefing companions: after the `# YYYY-MM-DD` heading (and any `> [!info]` callout below it), before the first `---` separator
   - For inline tracker/digest references: within the relevant section's summary paragraph (not as a separate section)
3. Add the section block for the report type (see per-report templates above)
4. Write back using `obsidian-cli` with `overwrite` to avoid creating a duplicate

If today's daily note does not yet exist, create it using the template from `00_Meta/Templates/Daily Note.md` before inserting.

---

## ISO Week Derivation

When you need the ISO week number and the Monday–Sunday range for any date:

```python
from datetime import date, timedelta

def iso_week_bounds(d: date):
    iso_year, iso_week, _ = d.isocalendar()
    monday = d - timedelta(days=d.weekday())
    sunday = monday + timedelta(days=6)
    return iso_year, iso_week, monday, sunday
```

Example for 2026-05-04 (Sunday):
- ISO year: 2026, week: 18
- Monday: 2026-04-27, Sunday: 2026-05-03  ← reporting period
- Filename prefix: `2026-W18`
- `post_date` (for LinkedIn): 2026-05-04

**Note:** The Executive Briefing date in the filename is the ISO week number of the *reporting period*, not the compilation date. If you compile on Monday 2026-05-04 covering the prior Mon–Sun (2026-04-27–2026-05-03), that is W17's content — file it as `2026-W17 Executive Briefing.md`.

---

## Frontmatter Quick Reference

All companion files must include at minimum: `title`, `date`, `tags`, `status`, `created-by`, `agent-role`.

| Field | Value for agent-produced files |
|-------|-------------------------------|
| `created-by` | `claude` |
| `agent-role` | `operations-analyst` (or the producing agent's role) |
| `status` | `active` (in-use report) or `complete` (archived) |

Existing files already in the vault use slightly different tags (`weekly-briefing` vs `briefing`, `created-by: operations-analyst` vs `claude`). Prefer the newer conventions shown above for new files; do not rewrite existing frontmatter unless asked.

---

*TEC Custom Skill — maintained by the Deltek Technical Services Engineering team.*
