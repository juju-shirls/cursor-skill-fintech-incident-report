# fintech-incident-report

Cursor Agent Skill for generating **customer-facing production incident / RCA reports** for fintech and payment platforms.

## Features

- Enterprise-grade structure (Executive Summary → Timeline → Impact → RCA → Prevention)
- **Concise 5-milestone timeline** (customer report → engineering response → root cause → resolved → customer validation)
- Explicit impact split: payment processing, settlement, fund flow, webhook/callback
- Customer-readable technical language conversion rules
- Gold example: Sadad PayCloud Webhook incident (2026-05-19)

## Install (Cursor)

### Option A — Clone into personal skills

```bash
git clone https://github.com/juju-shirls/cursor-skill-fintech-incident-report.git
```

Copy or symlink the folder to:

- **Windows:** `%USERPROFILE%\.cursor\skills\fintech-incident-report\`
- **macOS/Linux:** `~/.cursor/skills/fintech-incident-report/`

Ensure `SKILL.md` is at `fintech-incident-report/SKILL.md`.

### Option B — Project skill

Copy this repository into your project as `.cursor/skills/fintech-incident-report/`.

## Usage

In Cursor Agent, mention the skill or ask naturally:

- “根据群聊记录写一份对外事故报告”
- “Generate customer-facing RCA for this payment outage”
- “Write incident report like the Sadad webhook case”

Provide: chat logs, investigation notes, metrics, and customer local timezone.

## Repository layout

```
fintech-incident-report/
├── SKILL.md
├── README.md
└── references/
    ├── report-template.md
    └── example-sadad-paycloud-webhook.md
```

## License

MIT
