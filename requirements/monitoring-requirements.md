# TO-BE Monitoring Requirements
### SWIFT Payment Monitoring Solution — Treasury Operations

---

## 📌 Purpose

This document defines the TO-BE monitoring requirements for the centralised SWIFT 
payment monitoring solution delivered for the Xerox Corporation treasury team.

Requirements were gathered through stakeholder discovery workshops and gap analysis 
with treasury and IT teams, and translated into dashboard specifications and 
automated alert thresholds.

---

## 👥 Stakeholders

| Stakeholder | Role | Interest |
|---|---|---|
| Treasury Manager | Primary owner | Real-time payment visibility, SLA tracking |
| IT Operations | Technical delivery | Alert configuration, system integration |
| Compliance Team | AML oversight | Suspicious activity detection, SAR reporting |
| Correspondent Banks | External | Settlement confirmation, return messages |

---

## 🔍 AS-IS Pain Points (Gap Analysis Findings)

| # | Pain Point | Business Impact |
|---|---|---|
| 1 | No real-time visibility of MT103 payment status after dispatch | Treasury unaware of delays until counterparty raised issue |
| 2 | MT202 settlement confirmations tracked manually in Excel | High risk of missed settlements; time-consuming reconciliation |
| 3 | MT199/MT299 return messages not systematically captured | Returned payments discovered days late; cash flow impact |
| 4 | No SLA tracking per payment corridor | Breach of correspondent bank SLAs undetected |
| 5 | AML monitoring reliant on individual analyst judgement | No auditable, consistent process for suspicious activity |
| 6 | No single dashboard — data spread across TMS, email, Excel | Slow incident resolution; no consolidated view |

---

## ✅ TO-BE Monitoring Requirements

### 1. Payment Status Tracking

| Req ID | Requirement | Priority |
|---|---|---|
| MON-001 | System shall display real-time status of all outbound MT103 payments | Must Have |
| MON-002 | System shall track MT202 settlement confirmations against each MT103 | Must Have |
| MON-003 | System shall capture and flag MT199/MT299 return messages automatically | Must Have |
| MON-004 | Payment status shall reflect all stages: Sent → Acknowledged → Settled → Returned | Must Have |
| MON-005 | Status dashboard shall refresh at maximum 15-minute intervals | Should Have |

### 2. Alert Thresholds

| Req ID | Requirement | Threshold | Priority |
|---|---|---|---|
| ALT-001 | Alert when MT103 payment has no acknowledgement | > 2 hours after dispatch | Must Have |
| ALT-002 | Alert when MT202 settlement not received | > 4 hours after MT103 sent | Must Have |
| ALT-003 | Alert when MT199/MT299 return message received | Immediate on receipt | Must Have |
| ALT-004 | Alert when payment processing time exceeds SLA by corridor | Configurable per corridor | Must Have |
| ALT-005 | Daily summary alert of all SLA breaches and unresolved payments | 08:00 each business day | Should Have |

### 3. Power BI Dashboard Requirements

| Req ID | Requirement | Priority |
|---|---|---|
| DB-001 | Dashboard shall show total payment volume by corridor (daily/weekly/monthly) | Must Have |
| DB-002 | Dashboard shall show failure rate % by payment corridor | Must Have |
| DB-003 | Dashboard shall show average processing time by correspondent bank | Must Have |
| DB-004 | Dashboard shall show SLA breach count by corridor and date range | Must Have |
| DB-005 | Dashboard shall show top 10 corridors by failure rate | Should Have |
| DB-006 | Dashboard shall allow filtering by date range, currency, correspondent bank | Should Have |
| DB-007 | Dashboard shall show trend line of payment failures over rolling 30 days | Could Have |

### 4. DAX Measures Defined

| Measure Name | Definition |
|---|---|
| Failure Rate % | `DIVIDE(COUNT(failed payments), COUNT(total payments), 0) * 100` |
| SLA Breach Count | Count of payments exceeding corridor-specific SLA threshold |
| Avg Processing Time | Average time in hours from MT103 dispatch to MT202 settlement receipt |
| Return Rate % | `DIVIDE(COUNT(MT199/MT299 returns), COUNT(MT103 sent), 0) * 100` |

### 5. AML Monitoring Requirements

| Req ID | Requirement | Priority |
|---|---|---|
| AML-001 | System shall flag payments above high-value threshold (configurable, default £10,000) | Must Have |
| AML-002 | System shall detect structuring patterns — multiple payments just below threshold within 24hrs | Must Have |
| AML-003 | Flagged payments shall be routed to Compliance queue for review within 1 hour | Must Have |
| AML-004 | Compliance officer shall be able to approve, escalate, or file SAR from the system | Must Have |
| AML-005 | All AML decisions shall be logged with timestamp, officer ID, and rationale | Must Have |
| AML-006 | SAR filing workflow shall follow FCA reporting requirements | Must Have |

---

## 🔄 Escalation Workflow
