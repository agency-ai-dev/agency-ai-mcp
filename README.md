<div align="center">
<img src="./assets/logo.png" height="128px" alt="Agency AI Logo"/> 

# Agency AI MCP Server

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-brightgreen)](https://modelcontextprotocol.io)
[![Claude Ready](https://img.shields.io/badge/Claude-Ready-blue?logo=anthropic)](https://claude.ai)
[![ChatGPT Ready](https://img.shields.io/badge/ChatGPT-Ready-green?logo=openai)](https://chatgpt.com)

**Meta + Google ads analytics, strategy, and execution for Shopify stores**

🎯 AI-powered ad management for Claude and ChatGPT | 📊 Real-time performance insights | 💰 Budget optimization

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [What You Can Do](#-what-you-can-do)
- [Tool Reference](#-tool-reference)
- [Executions](#-executions-write-tools)
- [Requirements](#-requirements)
- [Quick Start](#-quick-start)
- [Example Prompts](#-example-prompts)
- [Links](#-links)

---

## 🎯 Overview

Agency AI MCP connects your Meta and Google ad accounts for Shopify stores to Claude and ChatGPT. Manage your ad operation conversationally:

✨ **Performance Analysis** — Campaign data, breakdowns, NCROAS and advanced KPIs
🎨 **Creative Recommendations** — Optimization suggestions and scaling plans
⚡ **Real-time Execution** — Create ads and update, pause, or activate campaigns, ad sets, and ads ([opt-in](#-executions-write-tools))
💬 **Natural Language** — Ask questions in plain English, get actionable insights
⏱️ **Fast & Efficient** — Faster responses with less token spend than direct-to-ad-platform MCPs

---

## 🎨 What You Can Do

### 📊 **Analyst**
Get campaign data, performance breakdowns, NCROAS and advanced KPIs across Meta and Google. Understand what's working and what's not.

```
💡 Example: "Compare my top 5 campaigns by ROAS last month"
```

### 🎯 **Strategist**
Receive budget guidance, creative recommendations, scaling plans, and cross-channel campaign planning strategies.

```
💡 Example: "Which campaigns should I scale and why?"
```

### ⚙️ **Execution**
Create new ads, pause underperforming campaigns, and adjust budgets across Meta and Google in seconds — without leaving the chat. Executions are opt-in per store; see [Executions](#-executions-write-tools).

```
💡 Example: "Pause all campaigns with ROAS below 2:1"
```

---

## 🧰 Tool Reference

Every store gets the **read** tools. The **write** tools appear only for stores with executions enabled.

### 📖 Read tools (always available)

| Tool | What it returns |
|------|-----------------|
| `get_brand_info` | Brand profile and economics — what the store sells, its audience, and breakeven ROAS |
| `get_campaigns` | Meta + Google campaigns with spend, revenue, ROAS, impressions, clicks, conversions, objective |
| `get_adsets` | Meta ad sets (with daily budget and performance) or Google asset groups |
| `get_ads` | Meta ads with performance for the date range, ranked by spend — also the source-ad picker for `create_ad` |
| `get_ad_creative` | One Meta ad's creative: type, primary text, headline, description, CTA, landing page, media URLs |
| `get_performance_timeseries` | Daily spend, revenue, and ROAS per connected platform |
| `get_pacing` | Month-to-date spend vs the monthly budget you set, per platform |
| `get_anomalies` | Statistically unusual spend/ROAS days over the trailing 30 days |
| `get_recommendations` | AI optimization recommendations with reasoning and confidence |
| `get_operation_status` | Progress, result, or error of a queued ad creation |

### ✍️ Write tools (executions-enabled stores)

| Tool | What it changes |
|------|-----------------|
| `create_ad` | Creates a new Meta campaign, ad set, or ad by reusing an existing ad's creative |
| `update_campaign` | Campaign name, daily budget, and/or status — Meta and Google |
| `update_adset` | Meta ad set name, daily budget, and/or status; Google asset group name and/or status |
| `update_ad` | Meta ad name and/or status |
| `set_campaign_status` | Starts or pauses up to 25 campaigns in one call, across both platforms |

---

## ⚡ Executions (write tools)

By default the connector is **read-only** — it can analyze your account but never change it. Executions add five write tools that let Claude or ChatGPT act on your ad account directly.

### 🔓 Turning executions on

Executions are enabled **per store** by Agency AI — email [hello@agencyai.app](mailto:hello@agencyai.app) to have them switched on for yours. Until then the write tools are not published to your assistant at all: they never appear in its tool list, and a write attempt comes back as *"Tool not found"* rather than a silent failure. Nothing else about your connection changes, and you don't need to reconnect after the switch — just start a new chat.

### 🛠️ What each write tool does

**`create_ad`** — Creates a new Meta campaign, ad set, or ad by duplicating an existing ad's creative. `placement` decides how much structure is created:

| `placement` | What gets created |
|-------------|-------------------|
| `new-campaign` | Campaign + ad set + ad (needs a daily budget) |
| `new-adset` | Ad set + ad inside an existing campaign |
| `existing-adset` | Ad only, inside an existing ad set |

Chat can't upload media, so a source ad always supplies the creative — build brand-new creative on the Create Ad page in the dashboard. Creation runs in the background and returns an `operation_id`; ask your assistant to check on it and it will call `get_operation_status`. The source ad set's schedule is *not* inherited: the new ad set runs with no end date unless you give a future one.

**`update_campaign`** — Renames a campaign, changes its daily budget, and/or starts/pauses it, on Meta or Google. Each field reports its own outcome, so a partial result ("renamed, but the budget was rejected") comes back honestly rather than as a blanket failure.

**`update_adset`** — Same three fields for a Meta ad set. Google asset groups support name and status only; their budget lives on the campaign.

**`update_ad`** — Renames or starts/pauses a Meta ad. Meta only — Google Performance Max has no separate ad level.

**`set_campaign_status`** — Bulk start/pause for up to 25 campaigns in a single call, Meta and Google mixed. Each campaign succeeds or fails independently and the result lists every outcome, so one bad id never blocks the rest.

### 🛡️ Guardrails

- 🔐 **Your account only** — the store is resolved from your verified connection, never from what the assistant passes in. Ids that don't belong to your connected Meta or Google account are rejected before anything is written.
- ✅ **Confirmation first** — write tools are declared to Claude and ChatGPT as destructive (`readOnlyHint: false`, `destructiveHint: true`), so your assistant asks before running one and the client can show its own approval prompt. The server also instructs the model to confirm with you before any write.
- 💰 **Budget ceiling** — Meta daily budgets are clamped to **$1–$5,000/day**, in chat exactly as in the dashboard.
- 📋 **Same pipeline as the dashboard** — writes go through the same services and ownership checks the Agency AI dashboard uses, and every change lands in your activity log.
- 📊 **Metered** — every tool call is recorded for your store (tool, duration, response size, errors), so what your assistant did is auditable.
- ↩️ **Reversible** — anything paused can be re-activated the same way. A queued ad creation can be checked with `get_operation_status`; pause the result if you change your mind.

---

## ✅ Requirements

- ✔️ An **Agency AI account** ([Create one here](https://agencyai.app))
- ✔️ **Claude Max** subscription OR **ChatGPT** plan supporting custom connectors
- ✔️ Connected Meta and Google ad accounts in Agency AI
- ✔️ For write tools: **executions enabled** for your store ([how](#-executions-write-tools)) — analysis needs nothing extra

---

## 🚀 Quick Start

### Endpoint URL

```
https://s.agencyai.app/mcp
```

Keep this URL handy — you'll need it for both Claude and ChatGPT.

### 📱 Claude Setup

1. Open **Claude** (claude.ai)
2. Click your profile → **Settings** → **Connectors**
3. Click **Add custom connector**
4. Paste: `https://s.agencyai.app/mcp`
5. Sign in with your Agency AI account when prompted
6. ✅ Done! Start asking Claude about your ads

### 💬 ChatGPT Setup

1. Open **ChatGPT** (chatgpt.com)
2. Click your profile → **Settings** → **Connectors**
3. Click **Add connector**
4. Paste: `https://s.agencyai.app/mcp`
5. Sign in with your Agency AI account when prompted
6. ✅ Done! Start asking ChatGPT about your ads

📖 **Need detailed steps?** See [llms-install.md](./llms-install.md) for a complete walkthrough with screenshots.

---

## 💬 Example Prompts

Try these questions with your connected Claude or ChatGPT:

```
📊 Analysis
"How did my Meta campaigns perform last 30 days vs prior 30?"
"Show me my top 10 best-performing ads by ROAS"
"Which campaigns are underperforming and why?"

🎯 Strategy
"Which ads should I iterate on and which should I cut?"
"Where should I shift budget this week?"
"Give me creative recommendations for my top 5 performers"

⚡ Execution (executions-enabled stores)
"Pause all campaigns with ROAS below 2:1"
"Raise Spring Drop's daily budget to $150"
"Duplicate my best-performing ad into a new campaign at $50/day"
"Rename these ad sets so the naming is consistent"
"Did that ad creation finish?"
```

---

## 🔗 Links
| Link | Purpose |
|------|----------|
| 🌐 [Website](https://agencyai.app) | Learn more about Agency AI |
| 🛍️ [Shopify App](https://apps.shopify.com/agency-ai) | Install from Shopify App Store |
| 📧 [Support](mailto:hello@agencyai.app) | Get help or send feedback |

## 📄 License
This project is licensed under the Apache License 2.0 - see the [LICENSE](./LICENSE) file for details.
