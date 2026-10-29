# Engineering Evidence & Technical Audit: ERISK System
### Enterprise Risk Management & Construction Governance Platform on Odoo 16.0

**Engineer:** Naufal Nabila  
**Role:** Odoo Backend, Enterprise Security & Cloud Integration Engineer  
**GitHub Profile:** [@naufalnab](https://github.com/naufalnab)  
**Primary Tech Stack:** Odoo 16.0 Community, Python 3.12, PostgreSQL, OWL / JavaScript, Docker, Google Cloud Run, Playwright  
**Audit Baseline Date:** October 2026 | **Baseline Commit:** `7e0c035`

---

## 📌 Executive Summary

This repository hosts the **forensic engineering audit and technical evidence package** documenting the production contributions of **Naufal Nabila** on the **ERISK System (Enterprise Risk Management Information System)**. 

ERISK is an enterprise governance platform developed on **Odoo 16.0** for state-owned infrastructure and construction enterprises (associated with PT Adhi Karya Tbk and subsidiaries). Across the repository's development lifecycle, Naufal authored **160 commits (30.5% of total project commits)**, serving as a core backend and security architect across Change Requests 3.01–3.12, enterprise SSO, mathematical risk exposure engines, and Google Cloud Vertex AI integrations.

---

## 🏆 Top 5 Verified Engineering Contributions

1. **Construction Handover & Project Lifecycle Automation (CR 3.05):**
   Architected multi-tier approval state machines inheriting `tier.validation` to govern construction project lifecycles (`active` $\rightarrow$ `pho` [Provisional Hand Over] $\rightarrow$ `fho` [Final Hand Over]) on extended `hr.department` project entities with permission guards and automated chatter audit logs.
2. **Enterprise Authentication, SSO & Anti-Brute-Force Engine:**
   Engineered the `erisk_sso` security module, enforcing password complexity rules, automated expiration reminder crons, Open Redirect mitigation, clickjacking defense (`X-Frame-Options: SAMEORIGIN`), and isolated autocommit PostgreSQL cursors (`cr = registry(dbname).cursor()`) logging failed attempts without dirtying parent transactions.
3. **Deterministic Mathematical Risk Impact Scoring & Exposure Parity (CR 3.06, 3.08):**
   Authored an order-independent quantitative ratio selector (`impact_scoring.py`) with float boundary tolerance (`rel_tol=1e-12`), establishing parity between persisted exposure storage and display formulas without mass-rewriting historical audit records.
4. **Google Cloud Vertex AI Microservice Gateway on Cloud Run:**
   Built an asynchronous FastAPI microservice (`izi_gemini_gateway`) connecting Odoo BI dashboards to Google Vertex AI (Gemini Flash), complete with Pydantic v2 schemas, in-memory rate limiters, and 209 lines of automated zero-downtime Bash deployment pipelines.
5. **Modern Odoo 16 OWL JavaScript Patching & Test Automation:**
   Developed modern Odoo 16 OWL patches on `FormController` (`@web/core/utils/patch`) delivering non-blocking sticky strategy notifications, supported by automated Odoo `TransactionCase` suites and Playwright browser regressions.

---

## 📂 Documentation & Audit Deliverables Index

| Document | Description |
|---|---|
| [**01_EXECUTIVE_SUMMARY.md**](./01_EXECUTIVE_SUMMARY.md) | High-level audit findings, verified achievements, domain verdicts, and strategic recommendations. |
| [**02_TECHNICAL_ARCHITECTURE.md**](./02_TECHNICAL_ARCHITECTURE.md) | System architecture topology, module dependency maps, and entity relationship diagrams. |
| [**03_PERSONAL_CONTRIBUTIONS.md**](./03_PERSONAL_CONTRIBUTIONS.md) | Granular forensic analysis of Naufal's 160 commits, commit SHAs, file statistics, and ownership confidence. |
| [**04_CONSTRUCTION_PROJECT_EVIDENCE.md**](./04_CONSTRUCTION_PROJECT_EVIDENCE.md) | Detailed analysis of construction ERP modeling vs native Odoo Project customization. |
| [**05_POSTGRESQL_LINUX_AUDIT.md**](./05_POSTGRESQL_LINUX_AUDIT.md) | PostgreSQL competency classification (Level 1–3), raw SQL mechanics, and Linux/DevOps forensics. |
| [**06_TERYX_REQUIREMENT_MATRIX.md**](./06_TERYX_REQUIREMENT_MATRIX.md) | Requirement-to-evidence mapping against the Teryx Tech (UAE) vacancy, interview Q&A, and gap closure. |
| [**07_CV_BULLETS_EN_ID.md**](./07_CV_BULLETS_EN_ID.md) | ATS-optimized CV bullets (1, 3, and 5 bullet versions), skills matrix, and 75-second elevator pitch (EN/ID). |
| [**08_EVIDENCE_INDEX.csv**](./08_EVIDENCE_INDEX.csv) | Machine-readable tabular audit trail (25 verified claims, file paths, line ranges, and commit SHAs). |
| [**09_OPEN_QUESTIONS.md**](./09_OPEN_QUESTIONS.md) | Clarification checklist for candidate recollection regarding historical accounting and production access. |
| [**10_PUBLIC_PORTFOLIO_CASE_STUDY.md**](./10_PUBLIC_PORTFOLIO_CASE_STUDY.md) | Sanitized public case study safe for portfolio websites, LinkedIn articles, or client proposals. |

---

## 🛠️ Verified Technical Stack

- **ERP & Framework:** Odoo 16.0 Community, Odoo ORM, Tier Validation (`base_tier_validation`), OWL (Odoo Web Library), QWeb, XML.
- **Backend & APIs:** Python 3.12 / 3.13, FastAPI, Pydantic v2, `a2wsgi`, `functions_framework`.
- **Database:** PostgreSQL 14/15/16, `psycopg2.sql` (injection-safe dynamic SQL), isolated cursors, composite constraints.
- **Cloud & DevOps:** Linux / Ubuntu CLI, Docker, Google Cloud Run, GCP Secret Manager, Vertex AI, Bash automation.
- **Testing & QA:** Odoo `TransactionCase`, Pytest, Playwright (`@playwright/test`).
- **Languages:** Indonesian (Native), English (Professional Working Proficiency), Arabic (Professional Working Fluency).

---

## 🛡️ Confidentiality & Boundary Notice

All reports in this repository have been produced through a **read-only forensic audit**. No proprietary customer data, live database dumps, credentials, private API keys, or confidential financial records are contained in this repository. All public case studies comply with enterprise non-disclosure standards.
