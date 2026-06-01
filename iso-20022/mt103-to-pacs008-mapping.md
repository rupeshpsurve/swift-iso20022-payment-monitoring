# MT103 to pacs.008 Field Mapping Document
### SWIFT MT Format → ISO 20022 MX Format Migration Reference

---

## 📌 Purpose

This document maps SWIFT MT103 (Customer Credit Transfer) fields to their equivalent 
ISO 20022 pacs.008 (FIToFICustomerCreditTransfer) XML elements.

Produced as part of the Payment Systems Monitoring project at Xerox Corporation to 
support the organisation's forward-looking ISO 20022 migration preparation.

---

## 📖 Background

| Item | Detail |
|---|---|
| **MT103** | SWIFT legacy flat-file format for customer credit transfers (cross-border payments) |
| **pacs.008** | ISO 20022 MX XML equivalent — FIToFICustomerCreditTransfer |
| **Why migrate?** | Swift mandatory migration deadline; richer structured data; better AML screening |
| **Key difference** | MT103 uses positional fields (e.g. :32A:); pacs.008 uses nested XML elements |

---

## 🗺️ Field Mapping Table

| MT103 Field | Field Name | pacs.008 XML Element | Notes |
|---|---|---|---|
| :20: | Transaction Reference | GrpHdr/MsgId | Unique message identifier |
| :23B: | Bank Operation Code | Not directly mapped | CRED = standard credit transfer |
| :32A: | Value Date / Currency / Amount | CdtTrfTxInf/IntrBkSttlmAmt + IntrBkSttlmDt | Split into amount and date in MX |
| :33B: | Currency / Instructed Amount | CdtTrfTxInf/InstdAmt | Original instructed amount |
| :50K: / :50A: | Ordering Customer (Debtor) | CdtTrfTxInf/Dbtr + DbtrAcct | Structured name & account in MX |
| :52A: | Ordering Institution | CdtTrfTxInf/DbtrAgt | Debtor's bank (BIC) |
| :53A: | Sender's Correspondent | CdtTrfTxInf/PrvsInstgAgt1 | Intermediary bank |
| :54A: | Receiver's Correspondent | CdtTrfTxInf/IntrmyAgt1 | Intermediary agent |
| :56A: | Intermediary Institution | CdtTrfTxInf/IntrmyAgt2 | Second intermediary if applicable |
| :57A: | Account With Institution | CdtTrfTxInf/CdtrAgt | Creditor's bank (BIC) |
| :59: / :59A: | Beneficiary Customer (Creditor) | CdtTrfTxInf/Cdtr + CdtrAcct | Structured name & IBAN in MX |
| :70: | Remittance Information | CdtTrfTxInf/RmtInf/Ustrd | Unstructured remittance info |
| :71A: | Details of Charges | CdtTrfTxInf/ChrgBr | SHA/OUR/BEN → DEBT/CRED/SHAR |
| :71F: | Sender's Charges | CdtTrfTxInf/ChrgsInf | Charge amount and agent |
| :72: | Sender to Receiver Info | CdtTrfTxInf/InstrForNxtAgt | Instructions for next agent |
| :77B: | Regulatory Reporting | CdtTrfTxInf/RgltryRptg | AML / compliance reporting codes |

---

## 🔍 Key Structural Differences

**1. Unstructured vs Structured Data**
- MT103 field :50K: accepts free-text debtor name and address in 4 lines
- pacs.008 requires structured elements: `Nm`, `PstlAdr`, `StrtNm`, `PstCd`, `TwnNm`, `Ctry`
- Impact: data cleansing required before migration — free-text addresses must be parsed

**2. Amount and Date Separation**
- MT103 field :32A: combines value date + currency + amount in one field (e.g. `230615EUR10000,`)
- pacs.008 separates these into `IntrBkSttlmAmt` (with Ccy attribute) and `IntrBkSttlmDt`
- Impact: parsing logic needed to split the combined MT field during migration

**3. Charge Code Translation**
- MT103 :71A: uses: `SHA`, `OUR`, `BEN`
- pacs.008 uses: `SHAR`, `DEBT`, `CRED`
- Impact: direct translation required in transformation layer

**4. Remittance Information**
- MT103 :70: limited to 4 lines of 35 characters (140 chars total)
- pacs.008 `RmtInf` supports both unstructured (Ustrd) and fully structured remittance data
- Impact: structured remittance enables better reconciliation and straight-through processing

---

## ⚠️ Migration Considerations

| Risk | Detail | Mitigation |
|---|---|---|
| Unstructured debtor/creditor data | Free-text MT fields cannot map directly to structured MX elements | Data quality assessment and cleansing before migration |
| Truncation of remittance info | MT103 :70: is 140 char max; MX allows more but receiving systems may still truncate | Define max length rules in transformation spec |
| Charge code mismatch | SHA/OUR/BEN must be translated to SHAR/DEBT/CRED | Add translation mapping in transformation layer |
| BIC vs IBAN | MT103 accepts account numbers in various formats; pacs.008 expects IBAN | Validate and convert account formats pre-migration |
| Regulatory reporting fields | :77B: free-text in MT103; structured `RgltryRptg` block in MX | Define code list mapping with Compliance team |

---

## 📋 Sample pacs.008 XML Structure (Simplified)

```xml
<Document>
  <FIToFICstmrCdtTrf>
    <GrpHdr>
      <MsgId>XEROX20230615001</MsgId>          <!-- maps from MT103 :20: -->
      <CreDtTm>2023-06-15T10:30:00</CreDtTm>
      <NbOfTxs>1</NbOfTxs>
      <SttlmInf>
        <SttlmMtd>CLRG</SttlmMtd>
      </SttlmInf>
    </GrpHdr>
    <CdtTrfTxInf>
      <IntrBkSttlmAmt Ccy="EUR">10000</IntrBkSttlmAmt>  <!-- maps from MT103 :32A: -->
      <IntrBkSttlmDt>2023-06-15</IntrBkSttlmDt>          <!-- maps from MT103 :32A: -->
      <ChrgBr>SHAR</ChrgBr>                               <!-- maps from MT103 :71A: SHA -->
      <Dbtr>
        <Nm>Xerox Corporation</Nm>                        <!-- maps from MT103 :50K: -->
      </Dbtr>
      <DbtrAcct>
        <Id><IBAN>GB29NWBK60161331926819</IBAN></Id>      <!-- maps from MT103 :50K: -->
      </DbtrAcct>
      <DbtrAgt>
        <FinInstnId><BICFI>NWBKGB2L</BICFI></FinInstnId>  <!-- maps from MT103 :52A: -->
      </DbtrAgt>
      <CdtrAgt>
        <FinInstnId><BICFI>DEUTDEDB</BICFI></FinInstnId>  <!-- maps from MT103 :57A: -->
      </CdtrAgt>
      <Cdtr>
        <Nm>Beneficiary Name</Nm>                         <!-- maps from MT103 :59: -->
      </Cdtr>
      <CdtrAcct>
        <Id><IBAN>DE89370400440532013000</IBAN></Id>       <!-- maps from MT103 :59: -->
      </CdtrAcct>
      <RmtInf>
        <Ustrd>Invoice 2023-456 payment</Ustrd>            <!-- maps from MT103 :70: -->
      </RmtInf>
    </CdtTrfTxInf>
  </FIToFICstmrCdtTrf>
</Document>
```

---

## 📚 Reference

- SWIFT MT103 Field Specifications — SWIFT Standards
- ISO 20022 pacs.008.001.08 — FIToFICustomerCreditTransfer
- SWIFT Migration Guide — ISO 20022 for Cross-Border Payments

---

*Produced by Rupesh Surve | Business Analyst | Part of Payment Systems Monitoring Portfolio*
