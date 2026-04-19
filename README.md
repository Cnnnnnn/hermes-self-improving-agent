# hermes-self-improving-agent

Hermes-adapted version of [self-improving-agent](https://github.com/peterskoett/self-improving-agent) for the [Hermes Agent](https://github.com/your-org/hermes-agent).

## What is this

A skill that logs learnings, errors, and corrections to `/opt/data/.learnings/` and promotes broadly applicable patterns to persistent memory files.

## Key differences from upstream

| Aspect | Upstream (OpenClaw) | This version (Hermes) |
|--------|---------------------|----------------------|
| Learnings dir | `~/.openclaw/workspace/.learnings/` | `/opt/data/.learnings/` |
| Promotion targets | OpenClaw workspace files | `/opt/data/SOUL.md`, `/opt/data/memories/MEMORY.md`, `/opt/data/AGENTS.md`, `/opt/data/TOOLS.md` |
| Auto-reminder | Hook (`activator.sh`) | Cron (Monday 9:35) |
| Review commands | grep `.learnings/*.md` | grep `/opt/data/.learnings/*.md` |

## Setup

Learnings directory and log files are created automatically on first use. The skill also creates a weekly cron job (Monday 9:35) to review pending items.

## Log files

- `LEARNINGS.md` — corrections, knowledge gaps, best practices
- `ERRORS.md` — command failures, exceptions
- `FEATURE_REQUESTS.md` — user-requested capabilities

## Promotion targets

| Learning type | File |
|---------------|------|
| Behavioral patterns | `/opt/data/SOUL.md` |
| Workflows | `/opt/data/AGENTS.md` |
| Tool gotchas | `/opt/data/TOOLS.md` |
| Environment facts | `/opt/data/memories/MEMORY.md` |

## Detection triggers

Automatically log when:
- User corrects you ("No, that's wrong...", "Actually...")
- A command/API fails unexpectedly
- You discover a better approach
- User requests a missing capability

## License

MIT
