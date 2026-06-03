# AML Transaction Monitoring Requirements
### Anti-Money Laundering | SWIFT Payment Monitoring | Compliance Framework

---

## 📌 Purpose

This document defines the AML transaction monitoring requirements produced in 
collaboration with the Compliance team as part of the centralised SWIFT payment 
monitoring solution at Xerox Corporation.

These requirements replaced an informal, analyst-dependent AML process with a 
structured, auditable framework aligned to FCA regulatory expectations.

---

## 👥 Stakeholders

| Stakeholder | Role | Interest |
|---|---|---|
| Compliance Manager | Owner | Regulatory adherence, SAR filing |
| Compliance Officers | End users | Daily transaction review and escalation |
| Treasury Manager | Contributor | Payment flow context and business rules |
| IT Operations | Technical delivery | System configuration and alert setup |
| FCA (Regulator) | External | SAR submissions, regulatory compliance |

---

## 🔍 AS-IS Pain Points

| # | Pain Point | Risk |
|---|---|---|
| 1 | AML checks reliant on individual analyst judgement — no documented rules | Inconsistent screening; regulatory risk |
| 2 | No automated detection of structuring patterns | Structuring activity could go undetected |
| 3 | High-value payment thresholds not formally defined or configured | Manual monitoring unreliable |
| 4 | No auditable log of AML decisions | Unable to demonstrate compliance to regulator |
| 5 | SAR filing process undocumented — dependent on one analyst | Key-person risk; process gaps |

---

## ✅ AML Monitoring Requirements

### 1. High-Value Payment Detection

| Req ID | Requirement | Priority |
|---|---|---|
| AML-001 | System shall flag all outbound MT103 payments above the configurable high-value threshold | Must Have |
| AML-002 | Default high-value threshold shall be set at £10,000 (configurable by Compliance Manager) | Must Have |
| AML-003 | Flagged payments shall appear in the Compliance review queue within 15 minutes of detection | Must Have |
| AML-004 | System shall display full payment details for each flagged transaction: amount, currency, debtor, creditor, correspondent bank, value date | Must Have |

### 2. Structuring Detection

| Req ID | Requirement | Priority |
|---|---|---|
| AML-005 | System shall detect structuring patterns — multiple payments from the same account within a 24-hour window where combined total exceeds the high-value threshold but each individual payment is below it | Must Have |
| AML-006 | Structuring alerts shall be flagged separately from high-value alerts in the Compliance queue | Must Have |
| AML-007 | System shall retain a 90-day rolling history of payments per account for pattern detection | Must Have |
| AML-008 | Compliance Manager shall be able to configure the structuring detection window (default 24 hours) | Should Have |

### 3. Suspicious Activity Detection

| Req ID | Requirement | Priority |
|---|---|---|
| AML-009 | System shall flag payments to high-risk jurisdictions as defined on the internal watchlist | Must Have |
| AML-010 | System shall flag payments where beneficiary name partially matches sanctions screening list | Must Have |
| AML-011 | Sanctions list shall be updatable by Compliance Manager without IT involvement | Must Have |
| AML-012 | System shall flag round-sum payments above £5,000 (configurable) as potentially suspicious | Should Have |

### 4. Compliance Review Workflow

| Req ID | Requirement | Priority |
|---|---|---|
| AML-013 | Compliance officer shall be able to take one of three actions on a flagged payment: Approve, Escalate, or File SAR | Must Have |
| AML-014 | Approved payments shall be released automatically with officer ID and timestamp logged | Must Have |
| AML-015 | Escalated payments shall be routed to Compliance Manager queue with reason noted | Must Have |
| AML-016 | All review decisions shall be logged with: timestamp, officer ID, action taken, and free-text rationale | Must Have |
| AML-017 | Flagged payments not actioned within 1 hour shall trigger an escalation alert to Compliance Manager | Must Have |

### 5. SAR Filing Requirements

| Req ID | Requirement | Priority |
|---|---|---|
| AML-018 | System shall provide a structured SAR filing template pre-populated with flagged payment data | Must Have |
| AML-019 | SAR submissions shall follow FCA reporting requirements and include all mandatory fields | Must Have |
| AML-020 | Filed SARs shall be logged in the system with reference number, date, and filing officer | Must Have |
| AML-021 | Payment subject to SAR shall be held and not released until Compliance Manager authorises | Must Have |

---

## 🔄 Escalation & SAR Workflow
