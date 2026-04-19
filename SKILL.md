---
name: self-improving-agent
description: "Captures learnings, errors, and corrections for Hermes. Triggers: /learn, user corrections, command failures, missing features. Also: review pending learnings at session start, promote recurring patterns to memory files. Hermes-specific: /opt/data/.learnings/ dir, github repo: Cnnnnnn/hermes-self-improving-agent."
hermes-adapted: true
learnings-dir: /opt/data/.learnings/
promotion-targets:
  - /opt/data/SOUL.md
  - /opt/data/memories/MEMORY.md
  - /opt/data/AGENTS.md
  - /opt/data/TOOLS.md
github-repo: https://github.com/Cnnnnnn/hermes-self-improving-agent
---

# Self-Improving Agent (Hermes Adaptation)

Log learnings and errors to markdown files for continuous improvement. Important learnings get promoted to permanent memory files that persist across sessions.

## Hermes Setup

**Learnings directory**: `/opt/data/.learnings/`
**Log files**:
- `LEARNINGS.md` — corrections, knowledge gaps, best practices
- `ERRORS.md` — command failures, exceptions
- `FEATURE_REQUESTS.md` — user-requested capabilities

**Promotion targets**:
| Learning Type | Promote To |
|---------------|------------|
| Behavioral patterns, principles | `/opt/data/SOUL.md` |
| Workflows, delegation rules | `/opt/data/AGENTS.md` |
| Tool capabilities, integration gotchas | `/opt/data/TOOLS.md` |
| Environment facts, persistent preferences | `/opt/data/memories/MEMORY.md` |

**Weekly cron review** (Monday 9:35, avoiding 9:00 batch and 9:30 trading broadcast):
```
35 9 * * 1 python3 /opt/data/scripts/review_learnings.py
```

## Quick Reference

| Situation | Action |
|-----------|--------|
| Command/operation fails | Log to `/opt/data/.learnings/ERRORS.md` |
| User corrects you | Log to `/opt/data/.learnings/LEARNINGS.md` with category `correction` |
| User wants missing feature | Log to `/opt/data/.learnings/FEATURE_REQUESTS.md` |
| API/external tool fails | Log to `/opt/data/.learnings/ERRORS.md` with integration details |
| Knowledge was outdated | Log to `/opt/data/.learnings/LEARNINGS.md` with category `knowledge_gap` |
| Found better approach | Log to `/opt/data/.learnings/LEARNINGS.md` with category `best_practice` |
| Recurring pattern detected | Track with `Pattern-Key`, bump `Recurrence-Count` |
| Broadly applicable learning | Promote to appropriate target above |

## Detection Triggers

Automatically log when:

**Corrections** (→ learning with `correction`):
- "No, that's not right..."
- "Actually, it should be..."
- "You're wrong about..."
- "That's outdated..."

**Feature Requests** (→ feature request):
- "Can you also..."
- "I wish you could..."
- "Is there a way to..."

**Knowledge Gaps** (→ learning with `knowledge_gap`):
- User provides information you didn't know
- Documentation you referenced is outdated
- API behavior differs from your understanding

**Errors** (→ error entry):
- Command returns non-zero exit code
- Exception or stack trace
- Unexpected output or behavior
- Timeout or connection failure

## Logging Format

### Learning Entry

Append to `/opt/data/.learnings/LEARNINGS.md`:

```markdown
## [LRN-YYYYMMDD-XXX] category

**Logged**: ISO-8601 timestamp
**Priority**: low | medium | high | critical
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Summary
One-line description of what was learned

### Details
Full context: what happened, what was wrong, what's correct

### Suggested Action
Specific fix or improvement to make

### Metadata
- Source: conversation | error | user_feedback
- Related Files: path/to/file.ext
- Tags: tag1, tag2
- See Also: LRN-20250110-001 (if related to existing entry)
- Pattern-Key: simplify.dead_code | harden.input_validation (optional)
- Recurrence-Count: 1 (optional)
- First-Seen: 2025-01-15 (optional)
- Last-Seen: 2025-01-15 (optional)

---
```

### Error Entry

Append to `/opt/data/.learnings/ERRORS.md`:

```markdown
## [ERR-YYYYMMDD-XXX] skill_or_command_name

**Logged**: ISO-8601 timestamp
**Priority**: high
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Summary
Brief description of what failed

### Error
```
Actual error message or output
```

### Context
- Command/operation attempted
- Input or parameters used
- Environment details if relevant

### Suggested Fix
If identifiable, what might resolve this

### Metadata
- Reproducible: yes | no | unknown
- Related Files: path/to/file.ext
- See Also: ERR-20250110-001 (if recurring)

---
```

### Feature Request Entry

Append to `/opt/data/.learnings/FEATURE_REQUESTS.md`:

```markdown
## [FEAT-YYYYMMDD-XXX] capability_name

**Logged**: ISO-8601 timestamp
**Priority**: medium
**Status**: pending
**Area**: frontend | backend | infra | tests | docs | config

### Requested Capability
What the user wanted to do

### User Context
Why they needed it, what problem they're solving

### Complexity Estimate
simple | medium | complex

### Suggested Implementation
How this could be built, what it might extend

### Metadata
- Frequency: first_time | recurring
- Related Features: existing_feature_name

---
```

## ID Generation

Format: `TYPE-YYYYMMDD-XXX`
- TYPE: `LRN` (learning), `ERR` (error), `FEAT` (feature)
- YYYYMMDD: Current date
- XXX: Sequential number or random 3 chars (e.g., `001`, `A7B`)

Examples: `LRN-20250115-001`, `ERR-20250115-A3F`, `FEAT-20250115-002`

## Resolving Entries

When an issue is fixed, update the entry:

1. Change `**Status**: pending` → `**Status**: resolved`
2. Add resolution block:

```markdown
### Resolution
- **Resolved**: 2025-01-16T09:00:00Z
- **Notes**: Brief description of what was done
```

Other status values:
- `in_progress` - Actively being worked on
- `wont_fix` - Decided not to address (add reason in Resolution notes)
- `promoted` - Elevated to SOUL.md, AGENTS.md, TOOLS.md, or MEMORY.md

## Promoting to Project Memory

When a learning is broadly applicable (not a one-off fix), promote it to permanent memory.

### When to Promote

- Learning applies across multiple situations
- Knowledge any session should know
- Prevents recurring mistakes
- Documents environment-specific conventions

### Promotion Targets

| Target | What Belongs There |
|--------|-------------------|
| `/opt/data/SOUL.md` | Behavioral patterns, communication style, principles |
| `/opt/data/AGENTS.md` | Workflows, delegation patterns, automation rules |
| `/opt/data/TOOLS.md` | Tool capabilities, usage patterns, integration gotchas |
| `/opt/data/memories/MEMORY.md` | Environment facts, persistent preferences, conventions |

### How to Promote

1. **Distill** the learning into a concise rule or fact (not a verbose incident write-up)
2. **Add** to appropriate section in target file (create file if needed)
3. **Update** original entry:
   - Change `**Status**: pending` → `**Status**: promoted`
   - Add `**Promoted**: /opt/data/SOUL.md` (or appropriate target)

### Promotion Examples

**Learning** (verbose):
> uv run failed because uv venv is incompatible with the system Python environment. The workaround is to call scripts directly with python3.

**In SOUL.md** (concise prevention rule):
```markdown
## Tool Invocation
- `uv run` fails with venv errors → use `python3` directly instead
- skill interface fails → find alternative tool → report issue → fix if有余力
```

**Learning** (verbose):
> The IMA API call in execute_code doesn't load shell environment variables from .env. Must read .env file directly with Python.

**In MEMORY.md** (concise fact):
```markdown
IMA API调用: execute_code(Python)环境中不会自动加载shell环境变量，需直接从/opt/data/.env读取
```

## Recurring Pattern Detection

If logging something similar to an existing entry:

1. **Search first**: `grep -r "keyword" /opt/data/.learnings/`
2. **Link entries**: Add `**See Also**: LRN-20250110-001` in Metadata
3. **Bump priority** if issue keeps recurring
4. **Consider systemic fix**: Recurring issues often indicate:
   - Missing documentation (→ promote to appropriate target)
   - Missing automation (→ add to AGENTS.md)
   - Architectural problem (→ create feature request)

### Promotion Rule

Promote recurring patterns when all are true:
- `Recurrence-Count >= 3`
- Seen across at least 2 different situations
- Within a 30-day window

Write promoted rules as **short prevention rules** (what to do before/while acting), not long incident write-ups.

## Periodic Review

Review `/opt/data/.learnings/` at natural breakpoints:
- Before starting a new major task
- After completing a complex task
- When working in an area with past learnings

### Quick Status Check

```bash
# Count pending items
grep -h "Status\*\*: pending" /opt/data/.learnings/*.md | wc -l

# List pending high-priority items
grep -B5 "Priority\*\*: high" /opt/data/.learnings/*.md | grep "^## \["

# Find learnings for a specific area
grep -l "Area\*\*: config" /opt/data/.learnings/*.md
```

### Review Actions
- Resolve fixed items
- Promote applicable learnings
- Link related entries
- Escalate recurring issues

## Implemented Improvements

All 5 requested improvements (plus #7) are implemented:

1. ✅ `/learn` fast command — `scripts/hermes-learn.py` parses input, writes to correct file
2. ✅ Session-start pending display — `scripts/check_pending.py` runs at session start
3. ✅ cron failure → auto-write ERR entry — `scripts/cron-task-wrapper.py` logs failures
4. ✅ critical error → Feishu real-time notification — `cron-task-wrapper.py` sends Feishu msg for critical/high
5. ✅ Recurrence-Count ≥ 3 → skill extraction — `review_learnings.py` detects and reports candidates
6. ⏸️ cron heartbeat detection (not implemented)
7. ✅ Weekly structured md report — `review_learnings.py --report` → `weekly_report.md`

### cron-task-wrapper.py

All 8 script-based cron tasks are wrapped with:
```
python3 /opt/data/skills/skills/self-improving-agent/scripts/cron-task-wrapper.py <name> <priority> <cmd...>
```

Exit codes: 0=ok, 1=task failed (logs ERR), 2=critical (logs ERR + Feishu notify)

## Weekly Cron Review Script

`/opt/data/scripts/review_learnings.py` — updated with full features:
- `--report`: generate structured `weekly_report.md`
- Default: print pending summary + high-priority items
- Auto-detect Recurrence-Count ≥ 3 entries

## Automatic Skill Extraction

When a learning is valuable enough to become a reusable skill, extract it manually.

```python
#!/usr/bin/env python3
"""Review pending learnings and send summary to user."""
import subprocess
from pathlib import Path

learnings_dir = Path("/opt/data/.learnings/")
if not learnings_dir.exists():
    print("No learnings directory found.")
    return

pending = []
for f in learnings_dir.glob("*.md"):
    if f.name == ".gitkeep":
        continue
    content = f.read_text()
    entries = content.count("**Status**: pending")
    if entries > 0:
        pending.append((f.name, entries))

if pending:
    lines = ["📋 Pending Learnings:\n"]
    for name, count in pending:
        lines.append(f"  {name}: {count} pending")
    print("\n".join(lines))
else:
    print("✅ No pending learnings.")
```

## Automatic Skill Extraction

When a learning is valuable enough to become a reusable skill, extract it manually.

### Extraction Criteria

A learning qualifies for skill extraction when ANY of these apply:
- **Recurring**: Has `See Also` links to 2+ similar entries
- **Verified**: Status is `resolved` with working fix
- **Non-obvious**: Required actual debugging/investigation
- **Broadly applicable**: Not situation-specific; useful across contexts
- **User-flagged**: User says "save this as a skill"

### Manual Extraction

1. Create `/opt/data/skills/skills/<skill-name>/SKILL.md`
2. Use template from skill's `assets/SKILL-TEMPLATE.md`
3. Follow skill format: YAML frontmatter with `name` and `description`, markdown body
4. Update learning entry: set `**Status**: promoted_to_skill`, add `**Skill-Path**: /opt/data/skills/skills/<skill-name>/`

### Skill Quality Gates

Before extraction, verify:
- [ ] Solution is tested and working
- [ ] Description is clear without original context
- [ ] Code examples are self-contained
- [ ] No hardcoded values that won't transfer
- [ ] Follows naming conventions (lowercase, hyphens)

## Hermes Session Start

**Automatic check** — at session start, run this to check for pending high-priority learnings:

```bash
python3 /opt/data/skills/skills/self-improving-agent/scripts/check_pending.py
```

**If pending high/critical items exist**, display them to user and ask: "有 N 个高优先级 learnings 待处理，要先回顾吗？"

**If no pending high items**, proceed normally.

## /learn Quick Command

Low-friction learning logger. Use instead of manually writing entries.

**Command**: `/learn <type> <content>`

**Types**:
| Type | Target File | Example |
|------|-------------|---------|
| `correction` | LEARNINGS.md | `/learn correction "uv run failed → use python3 directly"` |
| `insight` | LEARNINGS.md | `/learn insight "gh api can replace git push when TLS fails"` |
| `knowledge_gap` | LEARNINGS.md | `/learn knowledge_gap "didn't know Dexh800 has no GITHUB_TOKEN"` |
| `best_practice` | LEARNINGS.md | `/learn best_practice "check cron times before creating new ones"` |
| `error` | ERRORS.md | `/learn error "qveris API returned 401"` |
| `feature` | FEATURE_REQUESTS.md | `/learn feature "want weekly summary report"` |

**Script**: `python3 /opt/data/skills/skills/self-improving-agent/scripts/hermes-learn.py <type> <content>`

**When to use**:
- User corrects you → `/learn correction "..."`
- You discover something → `/learn insight "..."`
- User requests missing feature → `/learn feature "..."`
- Any command/API fails → `/learn error "..."`
- You realize knowledge gap → `/learn knowledge_gap "..."`
- You find a better approach → `/learn best_practice "..."`

**Output**: `✅ [LRN-20260420-003] 已写入 LEARNINGS.md`
