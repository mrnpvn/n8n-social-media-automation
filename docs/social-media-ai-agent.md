# Social Media AI Agent — architecture walkthrough

The **Social Media AI Agent** is a 26-node n8n workflow that curates the web every day, writes your posts from what it finds, and asks you to approve via Telegram before anything goes live.

## The pipeline

```
ScheduleTrigger (daily)
   → Crawl Hacker News + GitHub trending
   → Filter + rank items (dedup via Airtable)
   → AI writes tweet + LinkedIn post
   → Markdown formatting
   → Telegram notification with the draft
        ↓
   You approve / skip in Telegram (5 min window)
   → Wait node (approval window)
   → Publish to Twitter + LinkedIn
   → Mark as used in Airtable
```

## Why this design works

### 1. Curation beats generation from scratch
Instead of asking the AI "what should I post today", the workflow **starts from real sources** (Hacker News, GitHub trending). The AI's job is to summarise and repackage — not invent. Result: posts that reference actual news, more credible than generic AI content.

### 2. Airtable is the memory
Every candidate post is logged in Airtable with a hash. Before publishing, the workflow checks the hash — **the same link can never be posted twice**. This single trick is what keeps a daily auto-poster from becoming a spam account.

### 3. Telegram approval with a time window
The draft arrives as a Telegram message with two buttons: **Approve** / **Skip**. A `Wait` node gives you roughly 5 minutes; if you don't answer, it skips (fail-closed — nothing posts without consent). Telegram is faster to check than email, which is why this agent uses it instead of Gmail.

### 4. Human stays in the loop at the exact right point
Curation and writing are automated. The only decision left to you is "does this post match my voice?" — the one thing AI still can't do for you. Everything after approval is deterministic.

## Failure handling

- Source crawl fails → workflow reports to a Telegram admin chat, no posts attempted
- Approval timeout → item skipped and marked, never posted
- Publish API error → logged, retried once, then reported

No silent failures.

## What it costs to run

| Service | Tier | Cost/month |
|---|---|---|
| n8n | self-hosted / community cloud | £0 |
| Hacker News + GitHub APIs | free | £0 |
| Large language model | API per usage | ~£2–5 |
| Airtable | free | £0 |
| Telegram | free | £0 |

**£0–5/month** for a daily curated social presence you only spend 5 minutes a day on.

## Recreate vs buy

This workflow is part of the paid pack at:

- **Store**: https://mrnpvn.gumroad.com/l/n8n-automation-workflow-pack
- **20% off** with code `N8N20`

It ships as an importable JSON (26 nodes) inside the same pack as the Social Content Factory (100 nodes), plus the setup guide and support.

---

*Part of [N8N Social Media Automation Workflows](https://github.com/mrnpvn/n8n-social-media-automation) · free demo workflow in this repo: `workflows/rss-to-telegram-digest.json`.*