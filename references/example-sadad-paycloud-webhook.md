# Gold Example — Sadad PayCloud Webhook (2026-05-19)

Reference output aligned with customer-facing PDF. Use this density and timeline granularity—not the verbose 20-row chat-derived version.

---

## 1. Executive Summary

On May 19, 2026, Sadad reported that the PayCloud production environment stopped automatically sending webhook notifications to the Sadad institution endpoint starting from **11:42 AM Qatar Time (16:42 Beijing Time)**.

Although payment transactions continued to be processed successfully, asynchronous callback notifications were not delivered in real time. As a result, transaction statuses could not be synchronized promptly to Sadad merchants, causing some transactions to remain in "Processing", "Not Applied", or "Sync Failed" status on the Sadad side.

After investigation, the issue was identified as an abnormal **RabbitMQ cluster interruption** caused by network instability between the main site and DR site. The Wiseasy R&D and Operations teams immediately initiated emergency troubleshooting, MQ recovery, and compensation notification procedures. The system was fully restored on the same day, and all affected webhook notifications were successfully resent.

---

## 1.1 Incident Timeline

| Beijing Time | Qatar Time | Description |
|---|---|---|
| 2026-05-19 18:56 | 2026-05-19 13:56 | Sadad reported webhook notifications had stopped since 11:42 AM Qatar Time; Wiseasy acknowledged and initiated investigation |
| 2026-05-19 19:00 | 2026-05-19 14:00 | Engineering response and log analysis initiated |
| 2026-05-19 20:09 | 2026-05-19 15:09 | Root cause confirmed: abnormal MQ service; recovery and compensation tasks started |
| 2026-05-19 22:01 | 2026-05-19 17:01 | Compensation notifications completed; service restored |
| 2026-05-19 22:46 | 2026-05-19 17:46 | Sadad confirmed receipt of notifications and full service recovery |

---

## 2. Impact Analysis

The webhook interruption did not affect normal payment transaction processing or settlement. However, delayed transaction result synchronization impacted merchant visibility and customer experience.

### 2.1 Incident Duration

| Time Zone | Duration |
|---|---|
| Beijing Time | 2026-05-19 16:42 ~ 2026-05-19 22:46 |
| Qatar Time | 2026-05-19 11:42 ~ 2026-05-19 17:46 |

### 2.2 Impact Scope

| Category | Impact |
|---|---|
| Affected Service | PayCloud asynchronous webhook notification service |
| Core Transaction Processing | Not affected |
| Settlement / Fund Flow | Not affected |
| Affected Transactions | Approximately 10,000 transactions |
| Compensation Notifications | 12,325 notifications resent successfully |
| Merchant Impact | Delayed status sync; merchant complaints and trust concerns |

---

## 3. Root Cause Analysis

PayCloud uses RabbitMQ for asynchronous webhook notification processing. The MQ architecture was deployed as a **cross-region cluster** between the main site and the DR site.

At approximately **16:40 Beijing Time**, network instability between the primary and DR sites caused abnormal synchronization within the RabbitMQ cluster. Multiple MQ nodes at the primary site became unavailable, preventing webhook messages from being delivered successfully.

Architectural weaknesses included: missing automatic recovery, lack of cross-site network quality monitoring, and absence of queue backlog alerting.

---

## 4. Resolution & Preventive Measures

- Restore abnormal RabbitMQ cluster services and complete compensation notifications
- Configure automatic restart and recovery policies for RabbitMQ services
- Add webhook delivery logging and ERROR-level monitoring alerts
- Separate primary-site and DR-site MQ clusters to eliminate cross-region dependency risks
- Implement queue depth monitoring and backlog alerting
- Conduct regular disaster recovery drills and MQ failover testing
