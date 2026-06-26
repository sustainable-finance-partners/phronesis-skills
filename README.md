# phronesis-skills

Agent Skills for **Phronesis** — the decision-assurance layer for AI agents.

Each skill follows the **Agent Skills** standard: a `SKILL.md` with a `name` and a
`description` (the `description` is the trigger — the agent loads the skill when the
context matches). They work in Claude Code, Codex, Cursor, OpenCode, and other
Agent-Skills-compatible clients.

## Skills

- **[`phronesis-action-boundary`](phronesis-action-boundary/SKILL.md)** — before a
  consequential autonomous action (money · identity/access · security or infra config ·
  production deploy · data export · vendor selection · binding on a user's behalf), call
  Phronesis for an **auditable action boundary** (`approve / review / block /
  seek-evidence / limit / require-human`) plus a **Decision Receipt**, *before* the action
  executes. Phronesis never executes the action.

## About Phronesis

The neutral substrate an agent calls **before it acts** — turning intent, evidence, and a
calibrated forecast into an auditable Decision Asset with an action boundary and a
verifiable Decision Receipt. It is advisory/assurance: it never executes, binds, moves
money, or modifies identity; the calling agent and its principal retain the action.

- **For agents:** https://phronesisintel.com/for-agents
- **MCP discovery manifest:** https://api.phronesisintel.com/.well-known/mcp.json
- **MCP endpoint:** `https://api.phronesisintel.com/mcp` — principal-scoped; get a free
  diligence-tier JWT at `/api/v1/signup` and pass `Authorization: Bearer <JWT>`.

Operated by Sustainable Finance Partners, LLC. Not investment, legal, or other
professional advice; no ratings or guarantees.
