# End-to-End AI Training Data Pipeline & Quality Assurance Framework

## 📌 Overview
This repository contains an end-to-end data pipeline and Quality Assurance (QA) framework designed specifically for **RLHF (Reinforcement Learning from Human Feedback) and AI Data Operations**. 

The pipeline ingests raw crowdsourced AI annotation logs, performs automated **PII (Personally Identifiable Information) masking** for GDPR compliance, executes **SQL-based deduplication and anomaly detection**, and generates structured quality audit reports.

---

## 🛠️ Tech Stack & Workflow
- **Languages:** Python 3.x, SQL (SQLite / PostgreSQL Dialect)
- **Libraries:** `pandas`, `re` (Regex), `sqlite3`, `numpy`
- **Key Techniques:** Data Masking, Window Functions (`ROW_NUMBER()`), Anomaly Detection, Data Integrity Auditing

---

## 🚀 Key Pipeline Steps

### 1. Data Ingestion & Synthetic Flaw Simulation (Python)
Generated a structured dataset simulating 1,000+ crowdsourced RLHF tasks with intentional operational flaws:
- Sensitive PII leaks (emails, phone numbers, IP addresses).
- Quality anomalies (short responses, suspiciously low response times).
- Hallucinations / unverified factual statements.

### 2. PII Masking & Privacy Compliance (Python)
Implemented Regex-based transformation logic to anonymize sensitive details:
- Emails $\rightarrow$ `[MASKED_EMAIL]`
- Phone Numbers $\rightarrow$ `[MASKED_PHONE]`
- IP Addresses $\rightarrow$ `[MASKED_IP]`

### 3. SQL Data Validation & Deduplication
Using advanced SQL logic, the framework:
- Removes duplicate submissions per prompt and annotator using `ROW_NUMBER() OVER (PARTITION BY prompt, annotator_id ORDER BY created_at DESC)`.
- Flags quality issues automatically using conditional `CASE WHEN` logic:
  - `Response Length < 15` $\rightarrow$ `FLAG: Response Too Short`
  - `Response Time < 5s` $\rightarrow$ `FLAG: Suspicious Fast Completion`
  - Pattern matching for hallucinations $\rightarrow$ `FLAG: Hallucination / Fake Fact`

---

## 📊 Sample Pipeline Output (Quality Breakdown)
```text
Quality Status Breakdown:
- VALID: 44 tasks
- FLAG: Response Too Short: 3 tasks
- FLAG: Suspicious Fast Completion: 3 tasks
