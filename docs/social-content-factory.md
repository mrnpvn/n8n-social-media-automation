# Social Content Factory — architecture walkthrough

The **Social Content Factory** is a 100-node n8n workflow that turns a Google Doc of prompts into published, on-brand posts across X/Twitter, Instagram, Facebook, LinkedIn and Threads — with a human approval gate in the middle.

## The pipeline

```
Google Doc (prompts)
   → executeWorkflow trigger
   → AI generates post drafts + system prompt composition
   → pollinations.ai generates preview images
   → Email (Gmail) sent to you for approval
        ↓
   You click "approve" in the email
        ↓
   HTTP request marks the post approved
   → Publish nodes: Twitter · LinkedIn · Facebook · Instagram/Threads
   → Google Drive archive + audit log
```

## Why each block matters

### 1. Input: a Google Doc, not a form
All post ideas live in a document you already edit. The trigger runs the workflow on schedule, pulls the doc, and every row becomes a candidate post. Editing content means editing a Google Doc — no dashboard, no export.

### 2. AI generation with a "system prompt composition" step
Before generation, the workflow composes a system prompt from your brand context stored in sticky notes / a config node. This is what keeps the tone consistent: same voice, same language, same emoji policy on every platform. The AI writes the post AND a platform-specific variant.

### 3. Free images from pollinations.ai
Each draft gets a preview image generated at runtime (image URL → Gmail embed). Zero image stock costs. The human sees what the post will look like before approving.

### 4. The approval gate (Gmail → one click)
This is the core safety mechanism:

- Gmail arrives with the draft + preview image
- You reply or click a link to approve / reject
- The response is picked up (HTTP request polling), and only **approved** items continue downstream
- Rejected items stop silently with a log entry

AI generates, **you decide**. Every published post had a human sign-off.

### 5. Publishing nodes + retries
Each platform has its own node (Twitter, LinkedIn, Facebook Graph API, Instagram/Threads). Failures route through an error branch that reports to a Telegram admin chat instead of silently dropping a post.

### 6. Google Drive archive
After publishing, a copy of the final post + image + publish timestamp is saved to Google Drive. You get an audit trail without building a database.

## What it costs to run

| Service | Tier | Cost/month |
|---|---|---|
| n8n | self-hosted / community cloud | £0 |
| OpenAI / Anthropic | API per usage | ~£2–10 |
| pollinations.ai | free | £0 |
| Gmail + Google Drive | free | £0 |
| Telegram | free | £0 |

Roughly £0–10/month for a fully automated multi-platform publishing operation.

## Recreate vs buy

This workflow is part of the paid pack at:

- **Store**: https://mrnpvn.gumroad.com/l/n8n-automation-workflow-pack
- **20% off** with code `N8N20`

It ships as an importable JSON (100 nodes), with a step-by-step setup guide covering credentials, quota limits and the error paths.

---

*Part of [N8N Social Media Automation Workflows](https://github.com/mrnpvn/n8n-social-media-automation) · free demo workflow in this repo: `workflows/rss-to-telegram-digest.json`.*