---
name: identify-agent-traffic
description: Identify AI agents, crawlers and bots from a User-Agent string, an IP address, or web server access logs, and judge whether an identity claim is genuine or spoofed. Use when the user asks what bot made a request, whether traffic is really from GPTBot, ClaudeBot or another AI crawler, or wants to know which AI agents are hitting their site.
---

# Identify agent traffic

Use the AgentCheck connector tools.

## One request

Call `identify_agent` with the full User-Agent. Pass the source `ip` whenever the user has it: without an IP the result is only a claim.

State the verdict in these terms, without upgrading it:

| Verdict | Say |
|---|---|
| `ip_range_confirmed` | Confirmed: the IP is in the operator's published ranges |
| `rdns_confirmed` | Confirmed by reverse DNS |
| `ua_match_only` | Claims to be this agent; not checked against an IP |
| `unverifiable` | Claims to be this agent; the operator publishes nothing to check against |
| `mismatch_suspected_spoof` | Likely impersonation: the IP does not belong to the claimed operator |
| `unknown_agent` | Not in the catalog; it may still be automated |

Never call a User-Agent match "verified". The IP must be the client address seen at the network edge, not a private or load-balancer address.

## Logs

Call `analyze_access_log` with the raw lines (nginx or Apache combined format, up to 2,000 lines or 200 KB per call). For larger logs, ask for a representative sample or a filtered extract, or run several calls and add up the per-bot counts.

The log summary identifies bots by User-Agent only. For the bots that matter, or any with odd behaviour (high volume, many 403/429 responses, sensitive paths), pick a few of their source IPs from the log and check each with `identify_agent`.

Report per bot: operator, purpose (training, search, user-directed, agentic), request count, what it fetched, and how it was answered. Call out impersonation separately from genuine crawler activity.

## Context

`list_known_agents` gives the catalog, including which agents can be verified and how. Use it when the user asks what a bot is for or which bots exist.

If the user then wants to block or allow agents, the `harden-site-against-ai-crawlers` skill applies. For a visual summary, use `agent-activity-dashboard`.
