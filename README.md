# 🤖 AI-Powered Cold Outreach Automation System
### Built with n8n · OpenAI GPT-4 · Gmail · Google Sheets

---

## 📌 Overview

A fully automated B2B cold outreach system that handles the entire lifecycle of a cold email campaign — from lead selection to multi-step follow-up sequences — with AI-powered reply detection and real-time CRM updates.

No manual work required after setup. The system runs on a schedule, respects business hours, randomizes send timing to avoid spam filters, and intelligently routes each lead based on their response behaviour.

---

## ⚙️ How It Works

```
Google Sheets (Lead CRM)
        ↓
Lead Selection & Validation
        ↓
Random Delay (human-like timing)
        ↓
Condition Check (If node)
        ↓
Initial Email Sent via Gmail
        ↓
CRM Updated → Wait 3 Days
        ↓
Sitemap & Website Analysis (HTTP)
        ↓
AI Reply Detection (OpenAI GPT-4)
        ↓
Switch: Real Reply / No Reply / Auto-Reply
        ↓
┌─────────────────┬──────────────────┬─────────────────┐
│  Follow-up 1    │  Follow-up 2     │  Follow-up 3    │
│  (No Reply)     │  (Still No Reply)│  (Final Touch)  │
└─────────────────┴──────────────────┴─────────────────┘
        ↓
CRM Updated at Every Stage
```

---

## 🧠 Key Features

- **Smart Lead Routing** — Reads from Google Sheets, filters only uncontacted & research-complete leads
- **Human-like Timing** — Random 8–20 minute delays between sends to mimic human behaviour
- **Business Hours Enforcement** — Only runs Monday–Friday, 8am–5pm via Cron scheduling
- **Multi-step Follow-up Sequence** — 4-email sequence with 3-day gaps between each touchpoint
- **AI Reply Classification** — GPT-4 reads thread responses and classifies as:
  - `1` = Real human reply → stop sequence
  - `2` = No reply → send follow-up
  - `3` = Auto-reply / OOO → treat as no reply
- **Auto-Reply Detection** — Header-based + keyword-based detection for vacation responders
- **CRM Auto-Update** — Google Sheets updated at every stage (sent, replied, follow-up count, thread ID)
- **Thread-based Follow-ups** — All follow-ups reply within the same Gmail thread for context continuity
- **Deliverability Optimized** — No spam trigger words, no attachments, plain-text style HTML

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** (self-hosted) | Workflow orchestration |
| **OpenAI GPT-4** | Reply classification & AI logic |
| **Gmail API (OAuth2)** | Email sending & thread management |
| **Google Sheets API** | Lead CRM & status tracking |
| **HTTP Request nodes** | Website analysis & data enrichment |
| **JavaScript (Code nodes)** | Custom logic, reply parsing, timing |

---

## 📊 Workflow Architecture

### Nodes Overview
- `Get row(s) in sheet` — Pulls next uncontacted lead
- `Code in JavaScript` — Random delay generator (8–20 min)
- `Code in JavaScript1` — Page inference from lead data
- `HTTP Request` — Website/data enrichment
- `Code in JavaScript2` — Lead data normalization
- `If` — Validates lead before sending
- `Edit Fields` — Prepares email variables
- `Wait` — 8–20 min human delay before send
- `Send a message` — Initial outreach email (Gmail)
- `Update row in sheet` — Marks lead as contacted
- `Wait1/2/3` — 3-day gaps between follow-ups
- `Sitemap / HTTP Request2` — Additional enrichment
- `Code in JavaScript3` — Reply detection logic
- `Switch` — Routes based on reply type (1/2/3)
- `Reply to a message (×3)` — Follow-up emails in same thread
- `Update row in sheet2/3/4` — CRM updates per stage

---

## 📅 Send Schedule

```
Trigger:    Every 15 minutes
Days:       Monday – Friday only
Hours:      8:00 AM – 5:00 PM
Delay:      8–20 minutes random between contacts
Sequence:   Day 1 → Day 4 → Day 7 → Day 10
```

---

## 🔐 Setup Requirements

To replicate this workflow you will need:

- n8n instance (self-hosted or cloud)
- OpenAI API key (GPT-4 access)
- Gmail OAuth2 credential configured in n8n
- Google Sheets OAuth2 credential configured in n8n
- Lead database in Google Sheets with required columns

---

## 📈 Results Tracking

Every lead's journey is tracked in Google Sheets:

| Column | Tracks |
|--------|--------|
| `done` | Initial email sent |
| `thread_ID` | Gmail thread for follow-up replies |
| `message_ID` | Specific message ID for threading |
| `follow_up_1/2/3` | Follow-up send status |
| `Checked reply` | Whether reply was detected |
| `issues` | Reply classification result |
| `STOP-30` | Lead removed from sequence |

---

## 👤 Author

**Yassine Haigane**
AI Automation Specialist | n8n · OpenAI · Python · REST APIs
🌐 [yassine-automations.com](https://yassine-automations.com)

---

*Built and deployed on Railway — runs 24/7 without local machine dependency.*
