---
name: audit-site-for-ai-agents
description: Audit a website for third-party trackers, its AI-crawler policy (robots.txt), and agent-facing files such as llms.txt. Use when the user asks who tracks visitors on a site, how a site treats AI bots or crawlers, whether a site is ready for AI agents, or wants a privacy or AI-readiness review of a domain.
---

# Audit a site for trackers and AI-agent signals

Use the AgentCheck connector tools.

## Pick the right depth

- **Policy only** (how the site treats AI crawlers, which agent files it publishes): call `check_ai_agent_policy`. It takes seconds and stores nothing.
- **Full audit** (trackers too): call `scan_site`. It loads the page in a headless browser, takes 15-30 seconds, and **creates a public report page** on agentcheck.com for that domain. If the site is not clearly the user's own or a well-known public site, say that a public report will be created before scanning.

If `scan_site` returns `status: "running"`, call `get_scan_report` with the `scan_id` after a short wait.

## Read the results honestly

- `scan_status` other than `complete` (`partial_network`, `partial_static`, `blocked`, `error`) means the scan saw only part of the page, usually because of bot protection. Say so, and treat tracker counts as a lower bound.
- `tracker_score` is 0-100; higher means more third-party exposure. `null` means it could not be computed, not zero.
- robots.txt rules are what the site **declares**. They do not prove a crawler is blocked in practice.
- A bot under `not_mentioned` has no rule. Do not describe it as allowed.
- `basis: "wildcard"` means the bot inherits a blanket `User-agent: *` rule rather than being named.

## Report

Lead with the answer to what was asked. Then cover, as relevant:

1. Tracker exposure: score, the companies involved, and the categories that matter most (session replay, advertising, data platforms).
2. AI-crawler posture: which bots are blocked, allowed or unmentioned, and whether that looks deliberate.
3. Agent-facing files found or missing.
4. Two or three concrete next steps. For policy changes, the `harden-site-against-ai-crawlers` skill applies.

Include the `report_url` when there is one.
