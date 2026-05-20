# Report Template (copy and fill)

## Title

`{Customer} {Product} Incident Report - {Short Title}`

Example: `Sadad PayCloud Incident Report - Webhook Notification Interruption`

---

## 1. Executive Summary

{One paragraph: incident start (local + Beijing), what failed, what was NOT affected (payment/settlement/fund flow), customer-visible impact, identification of root cause category, recovery and compensation outcome.}

---

## 1.1 Incident Timeline

| Beijing Time | {Local Time Zone} | Description |
|---|---|---|
| {customer_report_time} | {local} | Customer reported {symptom}; {vendor} acknowledged and began investigation |
| {engineering_response_time} | {local} | Engineering response and investigation initiated |
| {root_cause_time} | {local} | Root cause identified: {cause summary}; recovery/compensation started |
| {resolved_time} | {local} | Issue resolved: {restore/compensation completed} |
| {validation_time} | {local} | Customer confirmed {recovery / notification receipt} |

---

## 2. Impact Analysis

{One short paragraph on business impact; clarify payment/settlement not affected if true.}

### 2.1 Incident Duration

| Time Zone | Duration |
|---|---|
| Beijing Time | {start} ~ {end} |
| {Local} | {start} ~ {end} |

### 2.2 Impact Scope

| Category | Impact |
|---|---|
| Affected Service | |
| Core Transaction Processing | Not affected / Affected |
| Settlement / Fund Flow | Not affected / Affected |
| Affected Transactions | ~{n} |
| Compensation / Notifications | {n} resent |
| Merchant / Operational Impact | |

---

## 3. Root Cause Analysis

{Paragraph: architecture context, what failed at direct cause time, customer-readable explanation.}

{Optional bullet list of architectural weaknesses: auto-recovery, monitoring, cross-site network, queue depth alerts, etc.}

---

## 4. Resolution & Preventive Measures

**Immediate**

- {restore}
- {compensation}

**Short-term**

- {monitoring, alerts, auto-restart, webhook logging}

**Long-term**

- {MQ isolation, DR drills, HA redesign}
