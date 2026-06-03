# Power BI Dashboard — KPI Definitions & DAX Measures
### SWIFT Payment Monitoring | Treasury Operations Dashboard

---

## 📌 Purpose

This document defines the KPIs, DAX measures, and dashboard specifications for the 
Power BI reporting solution delivered as part of the centralised SWIFT payment 
monitoring project at Xerox Corporation.

---

## 📊 Dashboard Pages Overview

| Page | Audience | Purpose |
|---|---|---|
| 1. Payment Overview | Treasury Manager | High-level payment volume and health |
| 2. Corridor Performance | Treasury Operations | SLA and failure rates by payment corridor |
| 3. Correspondent Bank Analysis | Treasury Manager | Processing time by correspondent bank |
| 4. AML Flags Summary | Compliance Manager | Daily AML flag volume and resolution status |

---

## 🔢 KPI Definitions

### Page 1 — Payment Overview

| KPI | Definition | Target |
|---|---|---|
| Total Payments Sent | Count of all outbound MT103 messages in selected period | — |
| Total Value Processed | Sum of all MT103 payment amounts in selected period | — |
| Payment Success Rate | % of MT103 payments settled without return message | > 98% |
| Return Rate % | % of MT103 payments receiving MT199/MT299 return | < 2% |
| SLA Breach Rate % | % of payments exceeding corridor SLA threshold | < 1% |
| Avg Processing Time | Average hours from MT103 dispatch to MT202 settlement | < 4 hrs |

### Page 2 — Corridor Performance

| KPI | Definition | Target |
|---|---|---|
| Failure Rate by Corridor | % of failed payments per payment corridor | < 2% per corridor |
| SLA Breach Count by Corridor | Count of SLA breaches per corridor in selected period | 0 target |
| Top 10 Corridors by Failure Rate | Ranked list of corridors with highest failure rates | — |
| Avg Settlement Time by Corridor | Average hours to settlement per corridor | Corridor-specific |

### Page 3 — Correspondent Bank Analysis

| KPI | Definition | Target |
|---|---|---|
| Avg Processing Time by Bank | Average hours from dispatch to settlement per correspondent bank | < 4 hrs |
| SLA Breach Count by Bank | Count of SLA breaches attributable to each correspondent bank | 0 target |
| Return Rate by Bank | % of payments returned per correspondent bank | < 1% |

### Page 4 — AML Flags Summary

| KPI | Definition | Target |
|---|---|---|
| Total Flags Today | Count of AML flags raised in current business day | — |
| Flags Resolved | Count of flagged payments actioned (approved/escalated/SAR) | 100% within SLA |
| Flags Pending | Count of flagged payments not yet actioned | 0 after 1 hour |
| SAR Filed Count | Count of SAR submissions in selected period | — |
| Avg Review Time | Average time from flag raised to Compliance officer action | < 1 hour |

---

## 📐 DAX Measures

```dax
-- Payment Success Rate
Payment Success Rate % =
DIVIDE(
    COUNTROWS(FILTER(Payments, Payments[Status] = "Settled")),
    COUNTROWS(Payments),
    0
) * 100

-- Return Rate
Return Rate % =
DIVIDE(
    COUNTROWS(FILTER(Payments, Payments[ReturnMessage] IN {"MT199", "MT299"})),
    COUNTROWS(Payments),
    0
) * 100

-- SLA Breach Count
SLA Breach Count =
COUNTROWS(
    FILTER(Payments, Payments[ProcessingHours] > Payments[CorridorSLAHours])
)

-- SLA Breach Rate
SLA Breach Rate % =
DIVIDE([SLA Breach Count], COUNTROWS(Payments), 0) * 100

-- Average Processing Time (hours)
Avg Processing Time (hrs) =
AVERAGEX(
    FILTER(Payments, Payments[Status] = "Settled"),
    Payments[ProcessingHours]
)

-- Failure Rate by Corridor
Failure Rate by Corridor % =
DIVIDE(
    COUNTROWS(FILTER(Payments, Payments[Status] = "Failed")),
    COUNTROWS(Payments),
    0
) * 100

-- AML Flags Pending
AML Flags Pending =
COUNTROWS(
    FILTER(AMLFlags, AMLFlags[Status] = "Pending")
)

-- Avg AML Review Time (mins)
Avg AML Review Time (mins) =
AVERAGEX(
    FILTER(AMLFlags, AMLFlags[Status] <> "Pending"),
    AMLFlags[ReviewTimeMinutes]
)
```

---

## 🎛️ Filters & Slicers (All Pages)

| Filter | Type | Values |
|---|---|---|
| Date Range | Date slicer | Daily / Weekly / Monthly / Custom |
| Currency | Dropdown | GBP, EUR, USD, All |
| Corridor | Multi-select | All payment corridors |
| Correspondent Bank | Dropdown | All correspondent banks |
| Payment Status | Multi-select | Sent, Settled, Returned, Failed |

---

## 📁 Related Documents

- [Monitoring Requirements](../requirements/monitoring-requirements.md)
- [AML Requirements](../requirements/aml-requirements.md)

---

*Produced by Rupesh Surve | Business Analyst | Part of Payment Systems Monitoring Portfolio*
