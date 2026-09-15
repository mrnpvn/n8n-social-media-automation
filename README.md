# N8N Social Media Automation Workflows

Ready-to-import **n8n workflows** that automate your social media publishing. Import the JSON, add your credentials, press activate. No code.

## What you get

The repo contains one **free workflow** (RSS → Telegram digest) plus documentation of the two full production workflows available as a paid pack:

| Workflow | Nodes | What it does | Availability |
|---|---|---|---|
| Social Content Factory | 100 | AI generates posts from your prompts, emails you an approval with preview images, you approve with one click, it publishes to X/Twitter, Instagram, Facebook, LinkedIn, Threads and archives everything to Google Drive | [Paid pack](https://mrnpvn.gumroad.com/l/n8n-automation-workflow-pack) |
| Social Media AI Agent | 26 | Crawls Hacker News + GitHub trending daily, AI writes a tweet + LinkedIn post, you approve via Telegram, it publishes. Airtable dedup. | [Paid pack](https://mrnpvn.gumroad.com/l/n8n-automation-workflow-pack) |
| RSS → Telegram Digest | 3 | Free: reads any RSS feed, deduplicates and forwards the latest items to a Telegram chat. Fully working. | [Free in this repo](workflows/rss-to-telegram-digest.json) |

## Free workflow: RSS → Telegram Digest

A minimal, complete workflow that forwards the latest 5 items from an RSS feed to a Telegram chat every hour.

1. Import `workflows/rss-to-telegram-digest.json` in n8n (Workflows → Import from File).
2. Open the **Telegram** node, connect your bot (`Botfather` → `/newbot`) and set the chat ID.
3. Change the RSS URL in the first node if you want a different feed.
4. Activate the workflow.

Uses only free services. No paid API keys required.

## Paid pack: full details

`docs/social-content-factory.md` and `docs/social-media-ai-agent.md` walk through every node of both production workflows — the architecture, the approval gates, the dedup logic and the failure handling.

- Store page: https://mrnpvn.gumroad.com/l/n8n-automation-workflow-pack
- **20% off** with code `N8N20` (first 100 buyers)

What you get with the pack:
- Both workflow JSONs (100 + 26 nodes)
- A step-by-step setup guide with screenshots
- Cloud vs self-hosted comparison
- Pro tips on credentials, quotas and error paths
- Support

## Stack used

- [n8n](https://n8n.io) — workflow automation (free, self-hosted or cloud)
- Google Docs, Gmail, Google Drive
- Telegram (free approval gate)
- Airtable (free dedup + archive)
- Pollinations.ai (free AI images)
- X / Twitter, LinkedIn, Facebook Graph API, Instagram

Everything runs on free tiers. £0/month running cost.

## Why these workflows exist

Most "automation" products are thin tutorials. These are production workflows that run daily: approval gates keep a human in the loop (AI generates, you approve), Airtable prevents duplicates, and error nodes log failures instead of silently dropping posts.

## License

The free workflow (`workflows/`) is MIT. The two premium workflows + documentation in `docs/` are not released here — they are sold via the Gumroad pack above.

## More products

This is part of a small catalog of digital products. See the rest: https://mrnpvn.gumroad.com