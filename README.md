# AgentCheck plugin for Claude

AgentCheck tells you which AI agents and crawlers are visiting a site, whether they are who they claim to be, and how exposed a site is to trackers and AI crawlers. This plugin connects Claude to the AgentCheck service and adds skills for the common jobs.

## What you can ask

- "Is this request really from GPTBot?" (paste the User-Agent and source IP)
- "Which AI bots are in this access log, and what are they fetching?"
- "Audit example.com: who tracks its visitors and which AI crawlers does it block?"
- "Write a robots.txt that blocks AI training crawlers but keeps search."
- "Show me a dashboard of AI-agent traffic from these logs."
- "Does this web page contain prompt-injection text?"

## What is included

**Skills**

| Skill | Use it to |
|---|---|
| `audit-site-for-ai-agents` | Audit a website's trackers, AI-crawler rules and agent-facing files |
| `identify-agent-traffic` | Identify AI agents in requests or logs and test their identity claims |
| `harden-site-against-ai-crawlers` | Decide on and generate an AI-crawler policy |
| `agent-activity-dashboard` | Turn results into a visual dashboard |

**Connector** (remote MCP server at `https://agentcheck.com/api/mcp`, no sign-in)

| Tool | What it does |
|---|---|
| `scan_site` | Loads a public page in a headless browser; reports trackers, a 0-100 exposure score and AI-crawler rules |
| `get_scan_report` | Returns a scan result or an existing public report |
| `check_ai_agent_policy` | Reads robots.txt and agent files (llms.txt, agents.json) without a browser |
| `identify_agent` | Matches a User-Agent to a known agent and tests it against IP ranges and reverse DNS |
| `analyze_access_log` | Summarizes AI-agent traffic in pasted log lines |
| `list_known_agents` | Lists cataloged agents by operator, type and purpose |
| `get_ai_bot_leaderboard` | Shows how well-known sites treat AI crawlers |
| `lookup_trackers` | Names the company behind third-party domains |
| `generate_ai_crawler_policy` | Generates robots.txt text |
| `scan_text_for_prompt_injection` | Flags known prompt-injection phrasings in text |

## Install

In Claude Code:

```text
/plugin marketplace add <owner>/<this-repo>
/plugin install agentcheck@agentcheck-plugins
```

On claude.ai or in Cowork, add the plugin from **Customize > Plugins**, then connect AgentCheck on the plugin's **Connectors** tab.

## Privacy

What you pass to a tool is sent to agentcheck.com to produce the answer.

- `scan_site` stores the result and creates a **public report page** for the scanned domain. Only scan sites you are comfortable having a public report for.
- Log lines, User-Agent strings, IP addresses and text sent to the other tools are processed to answer the request and are not stored.
- The connector needs no account and receives no data from your Claude conversations beyond tool inputs.

Privacy policy: https://agentcheck.com/privacy

## Limits

- A User-Agent match is a claim. Identity is only confirmed when the source IP is in the operator's published ranges or reverse DNS matches; many operators publish neither.
- robots.txt results show what a site declares, not what it enforces.
- Prompt-injection scanning is pattern-based and can miss novel attacks.
- New browser scans share an hourly limit across all users.

## Support

support@agentcheck.com · https://agentcheck.com/docs

## License

MIT. The license covers this plugin's files, not the AgentCheck service.
