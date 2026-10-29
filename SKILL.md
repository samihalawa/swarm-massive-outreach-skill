---
name: swarm-massive-outreach-skill
description: Use when the user wants an autonomous but compliant cold-outreach agent that discovers targets, deduplicates against mailbox and Notion history, writes personalized short emails, sends through SMTP, and logs every action to Notion. Resolve current campaign configuration and account access before sends, and distinguish reply metrics from qualified opportunities.
metadata:
  author: samihalawa
  version: "1.0.0"
  license: MIT
---

# Swarm Massive Outreach Skill

## Reply History And Opportunity Evidence

For an inbox audit, pending-action review, or profit ranking, use `$inbox-forensic-action-operator-skill`; keep this skill responsible for its own campaign work. Reconstruct the full relevant inbound, Sent, Drafts, attachment and later provider history before deciding who owes action. A draft, old unanswered message, tracker label or positive reply alone does not prove a current obligation, send, selection or win. Verify the newest native state immediately before an authorized action and read back afterward; preserve existing sends and submissions.

For all/every reviews, exhaust the bounded message-ID population, hydrate full bodies in provider-sized batches, reconcile thread-only IDs and aliases, and record the exact accounts, timezone bounds, cutoff, deduplicated counts and unavailable slices. Do not substitute a giant truncated dump, capped thread or relevance-filtered sample for complete coverage. Use native mailbox connectors or SMTP/IMAP before browser access.

Keep early interest and campaign response metrics separate from qualified earning opportunities. Employment qualifies only at final selection, offer or contracting; other commercial work needs explicit progress beyond the first approach, such as a passed review, active negotiation, accepted terms, committed customer or paid transaction. Separate public tenders/applications from other commerce; verify public case status, documents and deadlines on the official platform. Generic alerts and advertised rates are not pipeline value.

Rank qualified work by realistic cash profit, proximity to closing/payment, urgency and preventable loss. For this owner's operating model, own labour and technical delivery are zero cash cost; include separately staffed roles only when binding requirements force them, plus actual acquisition, supplier, fee, shipping, required travel and other external costs. Recover existing costs/agreements from history before calling them unknown. Optional outsourcing is a scenario, not an automatic deduction. Distinguish revenue, fixed/contingent amounts, held/available funds, profit, shared fees and noncash credits; invent no win probabilities.

Use the current authorized tracker and scope rather than creating duplicate logs or broadening outreach during an inbox review. A one-off follow-up does not authorize a recurring automation; do not create or restart scheduled agent loops without an explicit request.

## Purpose

Run a high-volume outreach wave without placeholders, guessed credentials, or silent assumptions.

The agent must:

- recover relevant campaign facts and exact-account credentials before the affected action;
- discover qualified targets from public sources;
- deduplicate against mailbox history and Notion before each contact;
- write one specific message per recipient;
- send only through the configured SMTP account;
- log every candidate, send, failure, reply, and bounce to Notion.

## Hard Start Gate

Resolve the current campaign values below from the authorized task, existing tracker, runtime environment and saved exact-account configuration. Reuse existing global environment values, including `~/.env` when available; retrieve credentials through the runtime-supported vault route. Do not invent missing facts or secrets. An unavailable send dependency blocks that send, not independent discovery or read-only reply analysis. Do not request the same authority or context already supplied by the owner.

Required environment variables:

| Variable | Meaning |
|---|---|
| `OUTREACH_COMPANY` | Company name |
| `OUTREACH_ONE_LINER` | One sentence describing what the company does and for whom |
| `OUTREACH_METRIC_1` | Strongest current traction metric |
| `OUTREACH_METRIC_2` | Second current traction metric |
| `OUTREACH_ASK` | What the campaign is asking for, such as funding, pilots, partnerships, or customer meetings |
| `OUTREACH_LINK` | Website, deck, memo, demo, or other link every recipient should see |
| `OUTREACH_CONTACT` | Preferred reply or contact line, such as email, phone, calendar, or WhatsApp |
| `OUTREACH_TARGET_DESCRIPTION` | Exact target profile for this wave |
| `SMTP_HOST` | SMTP host |
| `SMTP_PORT` | SMTP port |
| `SMTP_USER` | Sender email |
| `SMTP_PASSWORD` | SMTP password or app password |
| `NOTION_TOKEN` | Notion integration token |
| `NOTION_DATABASE_ID` | Notion database ID for the outreach tracker |

Optional environment variables:

| Variable | Default | Meaning |
|---|---:|---|
| `OUTREACH_BATCH_SIZE` | `200` | Total eligible sends to complete |
| `OUTREACH_SENDS_PER_HOUR` | `15` | Maximum send rate |
| `OUTREACH_WORD_LIMIT` | `95` | Maximum words per email body |
| `OUTREACH_REPLY_RATE_TARGET` | `15` | Reply-rate target percentage |
| `OUTREACH_MEETING_TARGET` | `8` | Positive conversation or meeting target |
| `IMAP_HOST` | derived from SMTP provider when obvious | Mailbox host for duplicate and reply audits |
| `IMAP_PORT` | `993` | IMAP SSL port |

Credential rules:

- Never print secrets.
- Never write secrets to Notion, local logs, reports, email body, subject, or final output.
- Never invent credentials or use guessed defaults for secrets.
- Use STARTTLS for SMTP when supported by the configured port.
- Use IMAP or provider mailbox tooling for duplicate checks. If mailbox access is unavailable, stop before sending.

## Notion Tracker Contract

Use the configured Notion database as the durable audit trail. If the database does not already have matching properties, use the closest existing properties and add missing detail to the page body. Do not fail just because a property name differs.

Canonical fields:

- `Name`
- `Company`
- `Role`
- `Email`
- `Channel`
- `Source URL`
- `Personalization Evidence`
- `Status`
- `Last Contacted At`
- `Thread ID`
- `Subject`
- `Message Body`
- `Bounce Reason`
- `Reply Summary`
- `Notes`

Canonical statuses:

- `Candidate`
- `Skipped duplicate`
- `Skipped no public route`
- `Ready`
- `Sent`
- `Failed`
- `Bounced`
- `Replied`
- `Meeting booked`
- `Closed`

Every target must have a Notion record before sending. Every send must append the exact subject and plain-text body. Every failure, bounce, and reply must be appended with timestamp and evidence.

## Target Discovery

Find targets matching `OUTREACH_TARGET_DESCRIPTION`.

Use current public evidence only:

- official company, fund, team, portfolio, press, contact, or application pages;
- reputable directories with source links;
- public LinkedIn profile pages when available to the agent;
- public posts, interviews, podcasts, talks, or funding announcements.

Required candidate evidence:

- target name or organization;
- contact route: public email, apply form, contact form, or LinkedIn profile URL;
- source URL;
- one sentence explaining why the target matches the campaign;
- one specific personalization hook tied to public evidence.

Do not send to:

- guessed email patterns;
- purchased, scraped, or leaked personal data;
- role accounts that prohibit pitches;
- minors, sensitive personal categories, or private individuals unrelated to the target profile;
- anyone already contacted, unless the task is an explicit follow-up in the same thread.

## Duplicate Gate

Before each send, search both mailbox history and Notion using:

- recipient email;
- domain;
- organization;
- named contact;
- normalized aliases;
- previous thread IDs.

If any prior outbound or inbound contact exists, skip new outreach and log `Skipped duplicate`, unless the latest user instruction explicitly asks for follow-up and the existing thread supports it.

## Message Rules

For every recipient:

- body is at most `OUTREACH_WORD_LIMIT` words;
- subject is 4-7 words;
- first sentence references specific public evidence about that recipient;
- no placeholders, merge tags, or generic template residue;
- no emojis;
- no "excited to";
- no "game-changer";
- calm, confident, direct tone;
- include `OUTREACH_LINK`;
- end with `OUTREACH_CONTACT`;
- plain text only.

Allowed structure:

- subject line with 4-7 specific words;
- greeting using the recipient's real first name when known, otherwise the team name;
- one public-evidence hook about the recipient and why it maps to the company;
- one compact sentence with the company one-liner, both metrics, and the ask;
- one short call to action;
- the exact configured contact line.

Before sending, reject any draft that contains bracketed placeholders, unresolved merge tags, invented facts, or claims unsupported by the campaign context or public source evidence.

## Sending Loop

Default batch target is `OUTREACH_BATCH_SIZE`.

For each candidate:

1. Verify required campaign config remains present.
2. Verify source URL still supports the hook.
3. Run duplicate gate against mailbox and Notion.
4. Create or update the Notion candidate page.
5. Draft subject and body.
6. Validate word count, subject length, required link, contact line, and no banned phrases.
7. Send through SMTP.
8. Save SMTP result, message ID if available, timestamp, subject, and body to Notion.
9. If send fails, retry once after a short delay. If retry fails, mark `Failed` and continue.
10. If a bounce is detected, mark `Bounced` and continue.

Rate limit:

- never exceed `OUTREACH_SENDS_PER_HOUR`;
- if the provider returns rate, auth, spam, or abuse warnings, stop sending and report the exact provider message without exposing secrets.

## Self-Correction Loop

After every 20 sends:

- audit replies, bounces, and delivery failures;
- identify subject patterns, hooks, target classes, and source types that are working;
- adjust future target selection and hooks;
- write a short strategy note to Notion;
- continue only if SMTP and mailbox health are normal.

Do not change company facts, metrics, ask, link, or contact line unless the user changes them.

## Compliance Rules

Follow applicable anti-spam and privacy rules for the sender and recipient locations.

Minimum requirements:

- use truthful sender identity;
- include a working reply/contact route;
- honor opt-outs immediately;
- do not misrepresent affiliation, traction, funding status, portfolio links, or prior relationship;
- do not bypass provider limits or anti-abuse controls;
- keep evidence for why each recipient was targeted.

If the requested campaign appears illegal, deceptive, credential-seeking, harassment-oriented, or privacy-invasive, stop and explain the blocker.

## End State Report

Report one of these outcomes in plain English:

- the number of sends completed, replies received, meetings or positive conversations booked, and bounces, followed by `Next wave ready`;
- the exact blocker, the missing value or provider problem to fix, and that the run should be restarted after the fix.

Also include:

- Notion database ID used, redacted only if the user asks;
- SMTP sender email;
- final target profile;
- send count;
- skipped duplicate count;
- skipped no-route count;
- failed count;
- proof that every send was logged.
