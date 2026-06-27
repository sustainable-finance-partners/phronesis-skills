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
  executes. Phronesis never executes the action. *(Scoring tool forthcoming — see MCP endpoint below.)*
- **[`phronesis-decision-receipt`](phronesis-decision-receipt/SKILL.md)** — obtain, record,
  and verify a **Decision Receipt**: the durable, auditable record of a consequential
  decision (the action boundary, the basis it rested on, and a reference for later outcome
  scoring). A Decision Receipt records the decision basis — it is not a guarantee of outcome.
- **[`phronesis-mcp-control-path`](phronesis-mcp-control-path/SKILL.md)** — when an agent
  operates through an MCP server, MCP portal, or identity provider, collect the
  **control-path context** (verified identity, MCP/portal + tool, policy result, credential
  scope, gateway/logs) to pass into the action-boundary call. Phronesis *consumes* this
  context — it does not authenticate or authorize. *Valid access is not approval to act.*

## About Phronesis

The neutral substrate an agent calls **before it acts** — turning intent, evidence, and a
calibrated forecast into an auditable Decision Asset with an action boundary and a
verifiable Decision Receipt. It is advisory/assurance: it never executes, binds, moves
money, or modifies identity; the calling agent and its principal retain the action.

- **For agents:** https://phronesisintel.com/for-agents
- **MCP discovery manifest:** https://api.phronesisintel.com/.well-known/mcp.json
- **MCP endpoint:** `https://api.phronesisintel.com/mcp`. Callable today (public,
  read-only): `health_check`, `bench_query`. Action-boundary scoring
  (`phronesis_score_action_boundary`, principal-scoped via a free diligence-tier JWT at
  `/api/v1/signup`) is **forthcoming**.

Operated by Sustainable Finance Partners, LLC. Not investment, legal, or other
professional advice; no ratings or guarantees.
