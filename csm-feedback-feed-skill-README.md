# CSM Feedback Feed

A Claude skill that gives Customer Success Managers a daily product feedback feed — bugs, feature requests, and (optionally) customer sentiment — pulled from their actual tools and scoped to their accounts.

It works across tool stacks. You bring whatever CRM, email, call recorder, and chat platform you already use, and the skill connects to them, figures out which accounts are yours, and starts scanning.

## What it does

Every day (or on demand), the feed scans four sources for product signal:

- **Call recordings** — Extracts bugs, feature requests, and sentiment from customer meeting summaries
- **Email** — Scans threads with customer contacts for product feedback
- **Team chat** — Reads both external customer channels *and* internal channels where teammates discuss customer issues
- **CRM** — Scopes everything to the CSM's active accounts so the feed isn't noisy

It deduplicates across sources (a bug mentioned on a call and then emailed about becomes one item), groups everything by customer, and delivers a structured summary to Slack, email, or in-chat.

## Supported tools

| Category | Supported |
|---|---|
| CRM | HubSpot, Salesforce, Attio |
| Email | Gmail / Google Workspace, Outlook / Microsoft 365 |
| Call recordings | Fathom, Gong, Chorus, Fireflies |
| Team chat | Slack, Microsoft Teams, Google Chat |

The feed works with whatever subset you have connected. Missing a source? It skips it gracefully and notes it in the output.

## Setup

On first run, the skill walks you through an interactive setup:

1. **Tool stack** — Which CRM, email, call recorder, and chat platform you use
2. **Account scoping** — Auto-detects your CRM's ownership fields and active customer filters, then validates the count. If you only own a few accounts but manage the whole book, you can track all active customers instead
3. **Channel naming pattern** — How your external customer channels are named (e.g., `acme-external`, `ext-acme`). Validates the pattern against real channels and corrects if needed
4. **Delivery** — Where the feed goes: a Slack channel, email, or in-chat
5. **Categorization** — By type (🐛 💡 💬), by priority (P1–P4), both, or keep it simple
6. **Feedback template** — If your product team has a preferred format for receiving feedback (a Linear template, a Coda form, a specific Slack format), the feed fills it out for each item automatically. Fields the feed can't populate get a bracketed placeholder so you can finish it quickly
7. **Sentiment** — If your template doesn't already include it, the skill asks whether you want customer sentiment tracked alongside bugs and feature requests
8. **Connection check** — Walks you through connecting any tools that aren't set up yet

Say "reconfigure my feed" anytime to change your preferences.

## Running the feed

**On demand:** Say "run my feedback feed" or "what feedback came in"

**As a scheduled task:** Set it up as a Claude scheduled task for weekday mornings at 8 AM. On Mondays it looks back 72 hours to cover the weekend; other days it looks back 24 hours.

## Example output

With a product team template configured, each bug and feature request comes out filing-ready:

```
📣 Product Feedback Feed — July 30, 2026
7 items from 6 customers

---

Brellium

Title: Support different billing frequencies for different contract phases

What were they trying to do? What went wrong or was missing?:
Customer has a 39-month deal where Year 1 should be billed annually
and Years 2–3 quarterly. The default billing schedule applies the same
frequency to all phases, so they couldn't configure this without help.

What did the customer specifically ask for?:
Ability to set different payment frequencies per phase within a single
contract.

What business goal was the customer trying to achieve?:
Closing a complex multi-year deal with accurate billing terms.

How critical is this to the customer?:
Active deal — they need this to send the order form. Workaround exists
but requires CSM help every time.

customer-feedback-priority: high-impact

Source: #acme-external + Linear FBK-3703
```

Without a template, the feed uses a simpler grouped format:

```
📣 Product Feedback Feed — July 30, 2026
3 items from 2 customers

---

🏢 Acme Corp

🐛 Bug: Invoice export times out on large accounts
   Source: Fathom call on Jul 30

💡 Feature Request: Bulk export for monthly reconciliation
   Source: #acme-external

---

🏢 Beta Inc.

💬 Sentiment: Positive — very happy with the new dashboard rollout
   Source: Fathom call on Jul 30
```

## How it handles edge cases

- **No CRM connected** — Runs unscoped across all sources and notes it
- **A source goes down** — Skips it, notes it in the feed, delivers the rest
- **Zero feedback found** — Says so clearly: "✅ No new product feedback from your accounts today"
- **Channel pattern mismatch** — If the pattern you entered doesn't match real channels, the skill finds the actual pattern and asks if you want to use it instead
- **Same feedback in multiple sources** — Deduplicates and merges into one item with both sources cited

## Installation

Save the `.skill` file to your Claude skills directory, or click **Save skill** when the file card appears in Claude.

## Trigger phrases

The skill activates on phrases like:

- "set up my feedback feed"
- "run my feedback feed"
- "what feedback came in"
- "what are my customers saying"
- "any bugs or feature requests"
- "product feedback digest"
- "reconfigure my feed"
