---
name: harden-site-against-ai-crawlers
description: Decide on and write an AI-crawler policy for a website, including a robots.txt that blocks or allows specific AI bots. Use when the user wants to block AI training crawlers, stop AI bots scraping their site, allow AI search while blocking training, or review and fix their robots.txt for AI agents.
---

# Set an AI-crawler policy

Use the AgentCheck connector tools.

## 1. See where the site stands

Call `check_ai_agent_policy` for the site. Note which bots are blocked, allowed and unmentioned.

## 2. Settle the goal

Blocking has costs, so confirm what the user wants before generating anything:

- **Training crawlers** (GPTBot, ClaudeBot, CCBot, Google-Extended): blocking keeps content out of future model training.
- **Search and answer crawlers** (OAI-SearchBot, PerplexityBot): blocking removes the site from AI search answers and the referral traffic they send.
- **User-directed agents** (ChatGPT-User): these fetch a page because a person asked. Blocking them breaks that person's request.

`list_known_agents` with a `purpose` filter shows which bots fall in each group.

## 3. Generate

Call `generate_ai_crawler_policy`:

- `block_training`: blocks training crawlers, leaves search and user-directed agents alone. The usual choice.
- `block_all_ai`: blocks every cataloged AI bot.
- `custom`: pass `blocked_bots` and `allowed_bots`.

The tool returns text only. It changes nothing on the site.

## 4. Hand over

- Merge the generated rules into the site's existing robots.txt; do not replace rules for other crawlers.
- robots.txt is a request. Well-behaved crawlers follow it; others do not. For enforcement, the user needs rules at the server, CDN or WAF. To avoid blocking impersonators' victims or missing real crawlers, match on verified IP ranges rather than User-Agent alone.
- Suggest re-running `check_ai_agent_policy` after deploying to confirm the rules parse as intended.
