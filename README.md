# phronesis-skills

Agent Skills for **Phronesis — the decision-assurance layer for AI agents.**

Before an autonomous agent takes a consequential action — moving money, changing
access, altering security/infra config, deploying, exporting data, accepting terms,
or binding on a user's behalf — it should be able to get a **defensible, auditable
action boundary** and a **Decision Receipt**. These skills teach an agent *when* and
*how* to call Phronesis for exactly that.

**Phronesis evaluates; it never executes.** The calling agent (or its rails) retains
the action. Outputs are decision-assurance, not advice, ratings, or guarantees;
calibration is reported honest-empty until outcomes resolve.

## The skills

| Skill | The moment it covers |
|---|---|
| `phronesis-action-boundary` | the general call — before any consequential action |
| `phronesis-decision-receipt` | the proof — record and verify the auditable receipt |
| `phronesis-mcp-control-path` | the input — pass verified control-path; *access ≠ assurance* |
| `phronesis-agentic-money` | money moves — *can-pay ≠ should-pay*; Phronesis never moves it |
| `phronesis-change-assurance` | a config/deploy/migration change — *generated ≠ safe*; never applies it |

## Install / load

These follow the **Agent Skills** standard (`SKILL.md` with `name` + `description`
frontmatter). They load in any agent that reads skills from a directory or repo
(Claude Code, Codex, Cursor, OpenCode, and compatible runtimes).

- **Whole set:** clone this repo into your agent's skills directory.
- **One skill:** copy the relevant `*/SKILL.md` into your skills directory.

The agent loads each skill's `description` and decides when it applies; no
configuration required to read them.

## Calling Phronesis

The skills call the **`phronesis-hermes`** MCP server.

- **MCP Registry:** `com.phronesisintel/phronesis-hermes` (Anthropic MCP Registry)
- **Endpoint:** `https://api.phronesisintel.com/mcp`
- **Public, read-only (no key):** `health_check`, `bench_query`
- **Core call (principal-scoped):** `phronesis_score_action_boundary` —
  free diligence-tier JWT via `https://api.phronesisintel.com/api/v1/signup`

## Working directory (`.phronesis/`)

Each skill keeps a per-task assurance trail in a `.phronesis/<task-slug>/` directory —
requirement → evidence notes → boundary request → receipt → review log → outcome
follow-up — so the task can be audited afterward. Agent-side only; receipts stay
verifiable against Phronesis either way. Convention:
[phronesis-working-directory.md](phronesis-working-directory.md).

## Honest scope

Phronesis is the **decision-assurance layer**: it returns a defensible action
boundary and a verifiable receipt, and scores outcomes over time. It is **not**
advice, a rating, a recommendation, a guarantee, or a creditworthiness assessment;
it **never executes** the action and **never moves money or applies changes**.

## Learn more

- Website: https://phronesisintel.com
- For agents (machine-readable): https://phronesisintel.com/for-agents
- Contact: support@phronesisintel.com

_Operated by Sustainable Finance Partners, LLC._
