# Production Incident Report Generator

AI-powered production incident RCA (Root Cause Analysis) generator for payment systems, fintech platforms, and enterprise infrastructure incidents.

Generate professional customer-facing incident reports automatically from:

- Incident chat logs
- Technical findings
- Recovery records
- System investigation results
- Existing RCA templates

Built as a [Cursor Agent Skill](https://cursor.com/docs/context/skills) — works in Cursor IDE and compatible agent environments.

---

## Features

### Automated Incident Report Generation

Generate enterprise-grade reports including:

- Executive Summary
- Incident Timeline (concise 5-milestone format)
- Impact Analysis
- Root Cause Analysis
- Resolution Actions
- Preventive Measures

### Designed for Fintech & Payment Systems

Optimized for incidents such as:

- Webhook failures
- MQ interruptions
- Payment gateway timeout
- Transaction synchronization failures
- DR site instability
- Redis / Database outages
- Kubernetes pod abnormalities
- API latency spikes

### Enterprise RCA Standards

The generated reports follow:

- Customer-facing communication standards
- Postmortem best practices
- Fintech operational reporting standards
- Production incident management workflows

Additional capabilities:

- **5-milestone timeline** — customer report → engineering response → root cause → resolved → customer validation (no chat noise)
- **Impact dimensions** — payment processing, settlement, fund flow, webhook/callback, merchant operations
- **Technical translation** — engineering findings converted to customer-readable language
- **Gold example** — Sadad PayCloud Webhook incident (2026-05-19)

---

## Example Output

```markdown
# Sadad PayCloud Production Incident Report

## 1. Executive Summary

On May 19, 2026, Sadad reported that PayCloud production stopped sending webhook
notifications since 11:42 AM Qatar Time (16:42 Beijing Time). Payment processing
and settlement were not affected. The issue was identified as RabbitMQ cluster
interruption due to network instability between primary and DR sites. Service was
fully restored the same day with compensation notifications completed.

## 1.1 Incident Timeline

| Beijing Time | Qatar Time | Description |
|---|---|---|
| 2026-05-19 18:56 | 2026-05-19 13:56 | Customer reported issue; Wiseasy acknowledged and began investigation |
| 2026-05-19 19:00 | 2026-05-19 14:00 | Engineering response and log analysis initiated |
| 2026-05-19 20:09 | 2026-05-19 15:09 | Root cause confirmed: abnormal MQ; recovery and compensation started |
| 2026-05-19 22:01 | 2026-05-19 17:01 | Compensation completed; service restored |
| 2026-05-19 22:46 | 2026-05-19 17:46 | Customer confirmed receipt and full recovery |

## 2. Impact Analysis

## 3. Root Cause Analysis

## 4. Resolution & Preventive Measures
```

See the full reference: [`references/example-sadad-paycloud-webhook.md`](references/example-sadad-paycloud-webhook.md)

---

## Install

### Personal skill (recommended)

```bash
git clone https://github.com/juju-shirls/cursor-skill-fintech-incident-report.git
```

Copy or symlink to:

| OS | Path |
|---|---|
| Windows | `%USERPROFILE%\.cursor\skills\fintech-incident-report\` |
| macOS / Linux | `~/.cursor/skills/fintech-incident-report/` |

Ensure `SKILL.md` lives at `fintech-incident-report/SKILL.md`, then **restart Cursor**.

### Project skill

Copy this repo into your project:

`.cursor/skills/fintech-incident-report/`

---

## Usage

Provide incident inputs (chat logs, investigation notes, metrics, customer timezone), then ask the agent:

- “根据群聊记录写一份对外事故报告”
- “Generate customer-facing RCA for this payment outage”
- “Use fintech-incident-report — timeline with key milestones only”

Optional: publish to Feishu with `lark-cli docs +create` (see `SKILL.md`).

---

## Repository Layout

```
fintech-incident-report/
├── SKILL.md                              # Agent instructions
├── README.md
├── LICENSE
└── references/
    ├── report-template.md                # Blank template
    └── example-sadad-paycloud-webhook.md # Gold example
```

---

## License

MIT — see [LICENSE](LICENSE).
