# Payment Systems Monitoring & API Analytics
### SWIFT Payment Monitoring | ISO 20022 Migration Preparation | AML Compliance

---

## 📌 Project Overview

This project involved defining and delivering a **centralised SWIFT payment monitoring solution** for the treasury team at Xerox Corporation, providing real-time visibility of cross-border MT103/MT202 payment flows.

A secondary workstream produced forward-looking **ISO 20022 field mapping documentation** (MT to MX), preparing the organisation for future migration to the MX message standard.

---

## 🎯 Objectives

- Deliver real-time monitoring of SWIFT cross-border payment flows (MT103, MT202)
- Reduce payment incident resolution time through automated alerting
- Produce MT103 → pacs.008 field mapping document as ISO 20022 migration preparation
- Document AML transaction monitoring requirements with the Compliance team

---

## 🛠️ Tools & Technologies

| Category | Details |
|---|---|
| **Payment Standards** | SWIFT MT103, MT202, MT199, MT299, ISO 20022 (pacs.008, pacs.009) |
| **Reporting & BI** | Power BI, DAX |
| **Data** | SQL, Excel |
| **Documentation** | Confluence, Visio |
| **Other** | Treasury Management System (TMS) |

---

## 🔄 My Approach (As Business Analyst)

**1. Discovery & Requirements Gathering**
- Conducted stakeholder discovery workshops with treasury and IT teams
- Performed gap analysis to identify monitoring blind spots in existing SWIFT payment flows
- Mapped the AS-IS end-to-end SWIFT payment chain:
  `MT103 → Correspondent Routing → MT202 → Settlement Confirmation → MT199/MT299 Return Messages`

**2. TO-BE Monitoring Design**
- Defined monitoring requirements for each payment stage with automated alert thresholds
- Translated MT103 fields into business-readable dashboard labels
- Defined DAX measures for: failure rate by corridor, SLA breach count, processing time by correspondent bank

**3. ISO 20022 Migration Preparation**
- Produced MT103 → pacs.008 field mapping document
- Mapped key SWIFT fields to equivalent ISO 20022 MX structured elements
- Document adopted as the foundation for the organisation's later ISO 20022 migration project

**4. AML Compliance Documentation**
- Collaborated with Compliance team to document AML transaction monitoring requirements
- Documented: high-value payment checks, suspicious activity detection, escalation workflows, SAR reporting processes
- Replaced informal analyst-dependent process with a structured, auditable AML framework

**5. Delivery Management**
- Managed three parallel workstreams: data integration, Power BI reporting, ISO 20022 mapping
- Coordinated vendors and reported progress to treasury leadership

---

## 📊 Key Artefacts Produced

- ✅ AS-IS SWIFT payment flow diagram (end-to-end correspondent banking chain)
- ✅ TO-BE monitoring requirements document with alert thresholds
- ✅ MT103 → pacs.008 ISO 20022 field mapping document
- ✅ Power BI dashboard specifications (DAX measures, KPI definitions)
- ✅ AML transaction monitoring requirements document
- ✅ Escalation workflow and SAR reporting process documentation

---

## 📈 Results & Impact

| Metric | Outcome |
|---|---|
| Incident resolution time | Reduced by **30%** |
| Payment visibility | Treasury team gained **real-time** cross-border payment monitoring for the first time |
| ISO 20022 readiness | MT103 → pacs.008 mapping document adopted as foundation for organisation's migration project |
| AML compliance | Informal process replaced with structured, **auditable framework** |

---

## 💡 Key Domain Knowledge Demonstrated

- **SWIFT message types**: MT103 (customer credit transfer), MT202 (financial institution transfer), MT199/MT299 (free format / return messages)
- **ISO 20022 MX formats**: pacs.008 (customer credit transfer), pacs.009 (financial institution credit transfer)
- **MT to MX field mapping**: understanding of structural differences between MT flat-file format and MX XML schema
- **AML monitoring**: high-value thresholds, structuring detection, SAR escalation workflows
- **Correspondent banking chain**: how cross-border payments route through intermediary banks

---

## 🏢 Client & Context

- **Client:** Xerox Corporation
- **Employer:** Atos Global IT Solutions and Services
- **Role:** Business Analyst – Payment Systems & API Monitoring
- **Duration:** Part of a broader engagement (Jul 2013 – Aug 2019)
- **Location:** India

---

## 📁 Repository Structure
