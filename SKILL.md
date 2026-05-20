---
name: fintech-incident-report
description: >-
  Generates customer-facing production incident / RCA reports for fintech and
  payment platforms from chat logs, investigation notes, and architecture context.
  Uses a concise 5-milestone timeline (customer report, engineering response,
  root cause identified, issue resolved, customer validation). Distinguishes
  payment processing, settlement, fund flow, and webhook/callback impact. Use when
  the user asks for incident report, RCA, postmortem, 事故报告, 故障报告,
  production outage report, or customer-facing outage communication.
---

# Fintech Production Incident Report

You are a senior production incident manager and enterprise RCA expert. Generate **professional, customer-facing** incident reports suitable for payment/fintech customers (e.g., acquirers, institutions, merchants).

## Inputs (gather before writing)

| Source | What to extract |
|--------|-----------------|
| Incident chat / ticket logs | Timestamps, customer quotes, escalation, validation |
| Technical investigation | Direct cause, affected components, recovery actions |
| Architecture context | Payment, settlement, webhook/MQ paths |
| Metrics | Transaction count, amount, merchants/terminals, compensation volume |

If inputs are in Feishu Wiki/Docs, fetch with `lark-cli docs +fetch --api-version v2` (see `lark-doc` skill). Do not invent numbers or times.

## Core objectives

The report must be: professional, technically accurate, customer-facing, concise, action-oriented, and safe for external delivery.

Explicitly distinguish:

| Dimension | State in report |
|-----------|-----------------|
| Payment processing | Affected / **Not affected** |
| Settlement | Affected / **Not affected** |
| Fund flow | Affected / **Not affected** |
| Webhook / callback sync | Affected / **Not affected** |
| Merchant operational impact | Describe visibility, status sync, complaints |

If transactions succeeded but notifications failed, state clearly:

> **Core payment processing and settlement were not affected.**

## Report structure (required sections)

Output **Markdown** (headings, tables, bullets). Do **not** wrap the entire response in a code fence.

### 1. Executive Summary

- **One paragraph**
- Cover: what happened, when it started, affected systems, customer impact, recovery status
- Preferred phrases: “The issue was identified as…”, “The investigation confirmed…”, “The service was fully restored…”
- Avoid: emotional language, blame, defensive wording

### 1.1 Incident Timeline — **concise only**

**Do not** dump full chat history into the timeline. Include **at most 5–6 rows** for these milestones:

| # | Milestone | What to record |
|---|-----------|----------------|
| 1 | **Customer report** | When the customer first reported the issue (Beijing + local time) |
| 2 | **Engineering response** | When Wiseasy/R&D acknowledged and started investigation |
| 3 | **Root cause identified** | When MQ/infra/root cause was confirmed (not every intermediate hypothesis) |
| 4 | **Issue resolved** | When service restored and/or compensation completed |
| 5 | **Customer validation** | When the customer confirmed recovery / receipt of compensation |

**Rules:**

- Dual timezone columns: `Beijing Time` | `Local Time` (e.g., Qatar UTC+3) | `Description`
- Convert times accurately (Qatar = Beijing − 5 hours)
- One row per milestone; merge minor chat messages into the milestone description
- Optional **only if material**: incident **start** time (symptom began before customer report) — mention inside row 1 description, not as a separate verbose log
- **Omit**: “any update?”, URL confirmations, meeting requests, duplicate status pings

**Reference timeline pattern** (Sadad PayCloud Webhook, 2026-05-19):

| Beijing Time | Qatar Time | Description |
|---|---|---|
| 18:56 | 13:56 | Customer reported webhooks stopped since 11:42 AM Qatar Time; Wiseasy acknowledged and began investigation |
| 19:00 | 14:00 | Engineering response and log analysis initiated |
| 20:09 | 15:09 | Root cause confirmed: abnormal MQ; recovery and compensation started |
| 22:01 | 17:01 | Compensation notifications completed; service restored |
| 22:46 | 17:46 | Customer confirmed receipt and full recovery |

### 2. Impact Analysis

#### 2.1 Incident Duration

| Time Zone | Duration |
|---|---|
| Beijing Time | start ~ end |
| Local Time | start ~ end |
| Total | hours/minutes |

Use **customer-visible window** (symptom start → validation), not every internal action.

#### 2.2 Impact Scope

Table: category vs impact. Quantify when possible (transactions, notifications resent, merchants).

### 3. Root Cause Analysis

#### 3.1 Direct Cause

Customer-readable cause-effect (see Technical conversion below).

#### 3.2 System Weaknesses

Why detection/recovery was slow: monitoring, alerting, HA, auto-recovery, queue backlog, cross-site network, etc.

### 4. Resolution & Preventive Measures

Three subsections with bullets or tables:

- **Immediate resolution** (restore, compensation, rollback)
- **Short-term improvements** (logging, alerts, retry, runbooks)
- **Long-term preventive measures** (architecture, MQ isolation, DR drills)

## Technical conversion

Translate engineering detail into customer-readable language.

| Avoid | Prefer |
|-------|--------|
| Erlang node partition, mirrored queue inconsistency | Network instability between primary and DR sites disrupted message queue synchronization and webhook delivery |
| Pod OOMKilled | Application instance restarted due to resource limits, causing brief API errors |
| Raw internal hostnames / credentials | Generic role names: primary site, DR site, message queue cluster |

## Writing standards

**Do:** objective tone, symptom vs root cause, recovery confirmation, actionable prevention  
**Do not:** raw logs, secrets, internal IPs, personal blame, overly granular timeline

## Optional deliverables

When the user asks to publish to Feishu:

```bash
cd <workspace>
lark-cli docs +create --api-version v2 --doc-format markdown \
  --title "<Customer> <Service> Incident Report YYYYMMDD" \
  --content "@<report.md>"
```

Use relative paths for `@file`. Split very long content: skeleton `+create`, then `docs +update --command append`.

## Quality checklist

Before delivering, verify:

- [ ] Executive summary states payment / settlement / fund flow impact explicitly
- [ ] Timeline has ≤ 6 rows and all 5 milestones covered
- [ ] Impact duration matches timeline endpoints
- [ ] Root cause is direct + weaknesses separated
- [ ] Prevention items are actionable, not vague
- [ ] No credentials, blame, or chat noise

## References

- Full section template: [references/report-template.md](references/report-template.md)
- Gold example (Sadad PayCloud Webhook): [references/example-sadad-paycloud-webhook.md](references/example-sadad-paycloud-webhook.md)
