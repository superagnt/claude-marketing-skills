# claude-marketing-skills

Seven superagnt skills make Claude Code your marketing assistant, with live data for leads, competitor research, SEO and social listening. Also in ChatGPT and Cursor.

A skill on its own is instructions. These run on live data out of the box: LinkedIn, X, the Meta Ad Library, people and companies with verified work emails, Google rankings and keywords, and a database that keeps what each run finds. Connect superagnt's hosted MCP server once and every skill here has it. Super SEO also reads your own Search Console once you connect it.

## Install

**1. Connect the server.** **Server URL:** `https://mcp.superagnt.com/mcp` (Streamable HTTP, OAuth). Your client opens superagnt's consent screen in your browser, and approving it creates your superagnt account if you don't have one. There's no API key to copy. The setup guide for every client: https://www.superagnt.com/install

- **Claude Code:** `claude mcp add --scope user --transport http superagnt https://mcp.superagnt.com/mcp`, then `claude mcp login superagnt`. For one project only, put [`config/claude-code.mcp.json`](config/claude-code.mcp.json) in it as `.mcp.json`.
- **Claude** (claude.ai, the desktop app and Cowork): Customize, then Connectors, then Add custom connector. Name it superagnt, paste the server URL, add it and approve. One connector covers Claude on the web, the desktop app, mobile, Cowork and Claude Code signed in with your Claude account.
- **ChatGPT:** In ChatGPT on the web, go to Plugins, select the plus button, then Add custom MCP server. Name it superagnt, enter the server URL under Connection, and set Authentication to OAuth. Tick "I understand and want to continue", then select Create as a plugin. Install the plugin, then start a new chat, type @ and select superagnt. ChatGPT takes custom MCP servers on the web only, and OpenAI lists them for Business, Enterprise/Edu and Pro plans. Business and Enterprise/Edu workspaces get every tool, write actions included. On Pro, connected servers are read and fetch only, so the skills that save their runs need one of those plans in ChatGPT.
- **Cursor:** put [`config/cursor.mcp.json`](config/cursor.mcp.json) in your project as `.cursor/mcp.json`, then click Login for superagnt under Settings, then MCP.
- **Codex:** `codex mcp add superagnt --url https://mcp.superagnt.com/mcp`, then `codex mcp login superagnt`.
- **VS Code, Gemini CLI, Windsurf, Zed:** each client's file is in [`config/`](config/).

**2. Install the skills.**

- Any agent that reads SKILL.md files (Claude Code, Codex, Cursor and others): `npx skills add superagnt/claude-marketing-skills`, or one of them with `--skill <name>`. It installs the seven, and `lead-generation` and `task-agents`, the two base skills they use most.
- Claude Code, as plugins: `/plugin marketplace add superagnt/plugins`, then `/plugin install <name>@superagnt` for each skill you want. Each brings superagnt's base skills with it.

Each skill is plain markdown under [`skills/`](skills/gtm-prospecting-desk/SKILL.md). Read one before you install it.

## The skills

| Skill | What it does | Part |
|---|---|---|
| [Outbound Pipeline Engine](skills/gtm-prospecting-desk/SKILL.md) (`gtm-prospecting-desk`) | 10 scored prospects with verified emails and outreach drafts, every morning. | Leads |
| [LinkedIn Warm Outbound](skills/warm-outbound-engine/SKILL.md) (`warm-outbound-engine`) | This week's warm leads: the people engaging with posts about your problem, scored and staged with a first message each. | Leads |
| [Competitor Ad Research](skills/competitor-ad-research/SKILL.md) (`competitor-ad-research`) | Your competitors' running Facebook and Instagram ads, grouped by offer, with each landing page's price and what the offer's keywords cost per click. | Competitor research |
| [Competitor Tracker](skills/competitor-mindshare/SKILL.md) (`competitor-mindshare`) | A daily scoreboard of you vs your competitors on X, on a chart you can share. | Competitor research |
| [Super SEO](skills/super-seo/SKILL.md) (`super-seo`) | Every move your site could make this week, ranked on one scale, each with the Search Console number behind it. | SEO |
| [Content Ideas Engine](skills/audience-radar/SKILL.md) (`audience-radar`) | Wake up to 5 post ideas, each cited to a real conversation in your niche on X. | Social listening |
| [LinkedIn Post Radar](skills/linkedin-post-radar/SKILL.md) (`linkedin-post-radar`) | The post types and formats winning on LinkedIn in your market this month, and five posts to write in those shapes. | Social listening |

Two base skills come with them: [lead-generation](skills/lead-generation/SKILL.md), the lead workflow that Outbound Pipeline Engine and LinkedIn Warm Outbound build on, and [task-agents](skills/task-agents/SKILL.md), which the skills' optional scheduled runs follow. A few steps also point at superagnt's other base skills (workspace-db, automations, data-pipelines, dashboards, superagnt-setup), which the Claude Code plugins in `superagnt/plugins` install with each skill.

Nothing goes out (email, posts, DMs) unless you ask for it or approve it. The skills draft and stage, and campaigns stay stopped until you start them. Your agent can propose a plan upgrade when a limit is reached, but money only moves on a confirmation page you open yourself.

## What each tool does

Every tool the skills call, as superagnt's server lists them. First the data:

| Tool | What it does |
|---|---|
| `data_agnt_people` | Searches people and enriches a person's profile. |
| `data_agnt_people_find_mobile` | Finds a person's mobile number from their email or LinkedIn profile. |
| `data_agnt_companies` | Searches, discovers and enriches companies. |
| `data_agnt_contacts` | Finds a person's work email and verifies it. |
| `data_linkedin_search` | Searches LinkedIn posts, hashtags, people and companies. |
| `data_linkedin_posts` | Reads a LinkedIn post, its comments and its reactions. |
| `data_linkedin_activity` | Reads a profile's posts, comments and reactions. |
| `data_linkedin_profiles` | Reads a LinkedIn profile and its follower count. |
| `data_linkedin_companies` | Reads a LinkedIn company page and its posts. |
| `data_facebook_ads` | Searches the Meta Ad Library by keyword or advertiser page, and reads an ad's details. |
| `data_x_discover` | Searches X posts with X's own operators. |
| `data_seo_serp` | Google's results for a query, Google AI Mode's answer, and search intent. |
| `data_seo_keywords` | Keyword ideas, volume and cost per click, ranked keywords and keyword gaps. |
| `data_seo_domains` | A domain's overview, organic competitors, AI visibility and an on-page audit. |
| `data_seo_backlinks` | A domain's backlinks, referring domains and backlink gap. |
| `data_web_scrape` | Reads a page's text: a landing page's offer and price, or your own page. |
| `connection_google_search_console_analytics_read`, `connection_google_search_console_sites_read`, `connection_google_search_console_sitemaps_read`, `connection_google_search_console_url_inspection_read` | Your own Search Console, once you connect it: clicks and positions, your properties, your sitemaps and a URL's index status. |

Then your workspace, which keeps what the skills find and runs them on a schedule:

| Tool | What it does |
|---|---|
| `agnt_db_sql`, `agnt_db_read`, `agnt_db_write` | The tables in your workspace's database that keep what each run finds. |
| `agnt_dashboards_read`, `agnt_dashboards_write` | Shareable dashboards, such as the Competitor Tracker's chart. |
| `agnt_tools_list_enabled`, `agnt_tools_search`, `agnt_tools_write` | Checks which tools your workspace serves and turns on any a skill needs. |
| `agnt_usage_read` | Your credit balance, which Competitor Ad Research reads before and after a run to say what it cost. |
| `agnt_guidance_read` | superagnt's own guides, such as the LinkedIn outreach guide LinkedIn Warm Outbound reads before it stages leads into HeyReach. |
| `agnt_agents_write`, `agnt_schedules_write` | Optional scheduled runs: a small agent superagnt runs on a schedule, daily to monthly. |
| `agnt_agents_run`, `agnt_agents_read`, `agnt_schedules_list`, `agnt_observability_read` | How `task-agents` tests a scheduled run once and checks that later runs fired. |
| `agnt_data_job_write`, `agnt_data_job_lookup`, `agnt_typescript_skill_dry_run`, `agnt_webhooks_write`, `agnt_webhooks_read` | LinkedIn Warm Outbound's optional autopilot: a hosted job that works through each new lead, tested before it runs, and webhooks that bring replies back. |

With HeyReach, Instantly, AgentMail or Gmail connected to your workspace, LinkedIn Warm Outbound stages its leads and drafts there through their tools:

| Tools | What they do |
|---|---|
| `connection_heyreach_*` | HeyReach's LinkedIn campaigns, their leads and their inbox. |
| `connection_instantly_*` | Instantly's email campaigns, their leads and their emails. |
| `connection_agentmail_*` | An AgentMail inbox of its own, with drafts, messages and threads. |
| `connection_gmail_*` | Drafts in your own Gmail, for you to review and send. |

The data tools only read. Each call uses credits from your superagnt workspace: https://www.superagnt.com/pricing

## Prompts to try

- "Find 10 prospects for <what you sell> with verified emails, and draft a first message to each."
- "Who's engaging with LinkedIn posts about <your problem> this week? Score them and stage a first message."
- "What Facebook and Instagram ads are <competitor> and <competitor> running in the US?"
- "Track me against <competitor> on X every day and give me a chart I can share."
- "Run my SEO for <your domain>."
- "Give me five post ideas from what my niche is talking about on X this week."
- "What post formats are working on LinkedIn in <market> this month?"

Every skill's page, with what it does step by step and every file it installs: https://www.superagnt.com/skills

## About superagnt

superagnt is a marketing plugin for Claude and ChatGPT that helps businesses find customers through outbound, SEO and ads. These seven skills are a set of its skills for doing that from Claude Code; the rest are at https://www.superagnt.com/skills

## License

This repo's config and skills are MIT licensed. See [LICENSE](LICENSE). The server is superagnt's hosted service.
