---
name: phronesis-mcp-control-path
description: >-
  Use when an agent operates through an MCP server, MCP portal, or identity provider
  and is about to take a consequential action. Collects the control-path context
  (verified identity, MCP/portal + tool, policy result, credential scope, gateway/logs)
  to pass into the Phronesis action-boundary call. Phronesis CONSUMES this context — it
  does not authenticate or authorize. Valid access is not approval to act.
---

# phronesis-mcp-control-path

## Purpose
**Control-path context** tells Phronesis *how* the agent reached the action — which
verified identity, which MCP server / portal and tool, the policy result, credential
scope, and whether traffic was gateway-routed and logged. Phronesis **consumes** this
(from your IdP / MCP Enterprise Auth / Cloudflare Access / gateway); it **does not
produce identity or authorization**. It uses the control-path as an input to the
**action boundary**.

> A portal or IdP letting the agent **reach** a tool is **access** — not **approval to
> act**. Pass the control-path so Phronesis can assess whether the action *should* proceed.

> **Availability:** the action-boundary call this context feeds
> (`phronesis_score_action_boundary`) is **forthcoming — not yet callable**. The live,
> callable `phronesis-hermes` tools today are `health_check` and `bench_query` (public,
> read-only). This skill describes the contract so you are ready when it ships.

## When to use
Before a consequential action that travels through an MCP server / portal / IdP — collect
the control-path and include it in the `phronesis-action-boundary` call.

## What to collect (`control_path_context`, plus `mcp_portal_context` for portal planes)
- `principal` — verified (never client-asserted)
- `identity_provider` — e.g. Okta / Entra / Ping
- `mcp_server` / `portal_provider` + `tool` + `tool_allowlisted`
- `policy_engine_result` — allow / review / block
- `credential_scope` — jit / standing · `session_bound` · `revocation_path`
- `gateway_routed` · `logs_available` · `policy_snapshot_hash`

## How to pass it
When the action-boundary call is live (**forthcoming**), include `control_path_context`
(and `mcp_portal_context` when an MCP portal is in the path) in the body of the
`phronesis-action-boundary` call. A more complete control-path → a tighter,
better-grounded boundary.

## Anti-patterns (do NOT)
- Do **not** trust client-supplied identity for authorization — only verified principal /
  control-path context.
- Do **not** treat "tool allowlisted" or "access granted" as **action approval** —
  *access is not assurance*.
- Do **not** omit the control-path on consequential actions.
- Do **not** expect Phronesis to authenticate or authorize — it consumes verified context
  and issues the action boundary on top.

## Where this sits in the stack
Identity / MCP-auth / portal / gateway govern **who can connect and what is exposed**.
Phronesis decides **whether this proposed action is defensible enough to execute** — and
issues the Decision Receipt. Consume the access layer; don't rebuild it.

_Phronesis — the decision-assurance layer. Operated by Sustainable Finance Partners, LLC.
Not investment, legal, or other professional advice; no ratings or guarantees._
