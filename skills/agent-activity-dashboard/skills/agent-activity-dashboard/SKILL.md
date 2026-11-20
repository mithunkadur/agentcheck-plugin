---
name: agent-activity-dashboard
description: Build a visual dashboard or report of AI-agent activity, site tracker exposure, or AI-crawler posture from AgentCheck results. Use when the user asks for a dashboard, chart, monitor view, visual summary or shareable report about AI bots, agent traffic, trackers, or how sites treat AI crawlers.
---

# AgentCheck dashboards

Gather the data with the AgentCheck connector tools, then present it visually using whatever this surface offers for visual output (an artifact or an HTML file). If neither is available, use compact tables.

## Which data for which dashboard

| Dashboard | Tools |
|---|---|
| Agent traffic on my site | `analyze_access_log` on the user's log lines, then `identify_agent` for the top source IPs |
| Site exposure | `scan_site` (creates a public report page; say so first), or `check_ai_agent_policy` for policy only |
| Comparing sites | `check_ai_agent_policy` per site, or `get_ai_bot_leaderboard` for well-known sites |
| Who is who | `list_known_agents` |

## Layout

Put the numbers a reviewer needs at the top, detail below:

1. **Headline tiles**: total AI-agent requests and share of all requests; number of distinct agents; suspected impersonation count; for a site, tracker score and posture.
2. **Per-agent breakdown**: requests by agent, grouped or coloured by purpose (training, search, user-directed, agentic).
3. **Trust**: each agent's verification verdict, using the tool's wording. Show confirmed, unverifiable and suspected-spoof as distinct states.
4. **Behaviour**: status codes per agent (403/429 show blocking already in effect) and top paths.
5. **Policy**: blocked, allowed and unmentioned bots.

## Rules

- Chart only values the tools returned. Do not estimate or fill gaps.
- Label scope: the number of log lines analysed and the time span they cover, or the scan date.
- Log analysis identifies agents by User-Agent. Mark anything not IP-verified as claimed.
- Keep the caveats the tools return visible on the dashboard, in a footnote.

A dashboard here is a snapshot of the data provided. For ongoing monitoring, the user re-runs it with fresh logs.
