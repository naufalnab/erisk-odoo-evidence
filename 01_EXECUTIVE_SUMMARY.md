# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 01: Executive Summary

- **Target Candidate:** Naufal Nabila
- **Target Opportunity:** International Remote Odoo Developer (notably Teryx Tech, UAE)
- **Primary Audited Repository:** `https://github.com/nusativ/erisk-system`
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Audit Baseline Date:** 2026-10-29 15:14:48 +0700
- **Auditor Role:** Senior Odoo Technical Architect, Git Forensics Analyst, PostgreSQL Engineer, Linux/DevOps Reviewer, and International Technical Recruiter
- **Operating Boundary:** Read-only forensic analysis of source code, version history, documentation, tests, and local runtime verification artifacts.

---

### 1. Executive Summary & Core Verdict

A comprehensive forensic audit of the `erisk-system` codebase reveals that **Naufal Nabila** has been a core, high-impact backend and security engineer on the platform, accounting for **160 out of 525 total commits (~30.5%)** across the repository's lifecycle. While the project was originally bootstrapped in late 2023 by upstream developers (Amirul and Galang Arbi), Naufal's contributions spanned two intensive development phases: an initial formula and record-rule hardening phase in **August–September 2025**, followed by major feature authorship, enterprise security architecture, AI integration, and Change Request delivery between **February and October 2026**.

The repository represents an enterprise-grade Risk Management Information System (**ERISK / ERISKA**) built on **Odoo 16.0 Community**, tailored for state-owned infrastructure and construction enterprises (demonstrably associated with PT Adhi Karya Tbk and subsidiaries like PT Adhi Commuter Properti).

#### Top 5 Strongest Verified Achievements (Ready for CV)

1. **Enterprise Construction Stage & Handover Workflow Automation (CR 3.05):**
   - Architected and implemented multi-tier approval state-machines for construction project lifecycles (`active` $\rightarrow$ `pho` [Provisional Hand Over] $\rightarrow$ `fho` [Final Hand Over]) using `base_tier_validation` on an extended `hr.department` project entity.
   - Enforced strict authorization boundaries: Super Admin reset protections, automated reviewer comment archiving into chatter, and atomic approval rollbacks.
   - *Evidence:* `erisk_system_validation/models/hr_department.py`, Commits `eb4702d`, `4aba9af`, `219f786`.

2. **Enterprise Authentication, Password Policy & Anti-Brute-Force Engine:**
   - Engineered the security architecture in `erisk_sso`, enforcing complexity rules (uppercase, lowercase, digits, symbols), configurable expiration intervals, and automated Indonesian reminder crons via `mail.template`.
   - Built a brute-force mitigation engine using isolated PostgreSQL cursors (`cr = registry(dbname).cursor()`) to log remote IP addresses and failed attempts in `api.log` without poisoning the primary transaction.
   - Closed critical web vulnerabilities: Open Redirect sanitization on `/web/login`, clickjacking defense via global `X-Frame-Options: SAMEORIGIN` monkey-patching, and Content-Security-Policy headers.
   - *Evidence:* `erisk_sso/models/model.py`, `erisk_sso/controllers/main.py`, Commits `1b8744e`, `8d8078f`, `8b316c3`.

3. **Deterministic Quantitative Risk Impact Scoring & Mathematical Exposure Modeling (CR 3.06, 3.08):**
   - Authored the core mathematical engine in `erisk_system/models/impact_scoring.py`, providing deterministic, order-independent interval matching for risk exposure based on contract thresholds.
   - Formulated and verified the multi-variable exposure equation: $\text{Eksposur} = \text{Skala Dampak} \times \text{Nilai Tengah Probabilitas} \times 1\% \times \text{Batasan Risiko}$.
   - Implemented historical storage preservation to prevent mass-rewrite of stored legacy values during formula recomputations, validated by Odoo ORM transaction tests.
   - *Evidence:* `erisk_system/models/impact_scoring.py`, `erisk_system/models/risk_register.py`, Commits `eb4702d`, `b188414`, `84474a2`.

4. **Production AI Analytics Gateway on Google Cloud Platform:**
   - Architected and built an independent microservice (`izi_gemini_gateway`) bridging Odoo's IZI Dashboard with Google Cloud Vertex AI (Gemini 2.0 / 3.5 Flash Lite).
   - Developed Pydantic v2 schemas, a sliding-window in-memory rate limiter, and a WSGI-to-ASGI adapter (`main.py` using `a2wsgi` and `functions_framework`).
   - Wrote a zero-downtime bash deployment automation (`deploy/gcp/deploy.sh`) configuring Cloud Run, Secret Manager key rotation, least-privilege IAM, and automated smoke testing.
   - *Evidence:* `izi_gemini_gateway/`, `deploy/gcp/deploy.sh`, `main.py`, Commits `aa2b059`, `8998b45`.

5. **Automated Quality Assurance & Odoo Regression Testing Harness:**
   - Authored comprehensive backend unit tests (`TransactionCase` in `test_frd_runtime.py` and `test_frd_workflows.py`, totaling 20+ automated backend test cases) covering access control, unlinking guards, and state transitions.
   - Maintained and executed `@playwright/test` end-to-end browser regression suites (`qa/eriska/tests/`) validating complex Odoo 16 web forms, multi-role permissions, and dynamic DOM states.
   - Authored technical investigation protocols for production-clone defects (e.g., Investigation Report `INVESTIGASI_NEXT_STEP_4824.md`).
   - *Evidence:* `erisk_system/tests/test_frd_runtime.py`, `qa/eriska/`, Commits `4aba9af`, `fc2ce24`, `3e5d032`.

---

### 2. Critical Domain Verdict: Construction & Odoo Project Relevance

| Category | Finding | Verdict |
|---|---|---|
| **Direct Native `project.project` Extension** | Native `project.project` and `project.task` models are NOT used by the core ERISK risk workflows. Native project modules appear only as 3rd-party Gantt libraries from Cybrosys and 7D, which Naufal never authored. | **NOT VERIFIED / DO NOT CLAIM NATIVE `project.project` CUSTOMIZATION** |
| **Construction Industry ERP & Project Governance** | ERISK models construction projects by extending `hr.department` with `category='project'`, `eat` (Contract Value / Nilai Kontrak), `is_tender` (Tender phase tracking), and `project_stage` (`active` $\rightarrow$ `pho` $\rightarrow$ `fho`). Handover milestones, contract limits, and project risk registers are deeply integrated. | **VERIFIED HANDS-ON: Construction Industry ERP & Project Governance** |
| **Defensible CV Terminology** | "Construction ERP Workflow Engineering", "Enterprise Project Risk Management", "Construction Handover (PHO/FHO) Lifecycle Automation". | **100% DEFENSIBLE** |

---

### 3. PostgreSQL Engineering Depth Verdict

- **Classification:** **Level 2 — Direct PostgreSQL Engineering & ORM Specialist**
- **Evidence:**
  - Dynamic SQL generation using `psycopg2.sql` (`sql.Identifier`, `sql.SQL`) with strict alphanumeric whitelist regexes to guarantee SQL injection immunity (`izi_dashboard/models/common/izi_dashboard.py`).
  - Isolated cursor transactions (`cr = registry(dbname).cursor()`, `cr._cnx.autocommit = True`) for audit logging without dirtying or rolling back parent business transactions (`erisk_sso/models/api_log.py`).
  - Direct parametrized SQL execution (`self.env.cr.execute("UPDATE ...")`) combined with low-level Odoo ORM cache invalidation (`record.invalidate_recordset()`, `record.flush_recordset()`) in test suites.
  - Definition of complex `_sql_constraints` enforcing composite data uniqueness across project years and quarters.
- **Boundary / Gap:** Level 3 (Production DBA, query optimization using `EXPLAIN ANALYZE`, vacuum tuning, high-availability clusters) is **NOT VERIFIED** in the repository (basic `pg_dump` backup scripts observed).

---

### 4. Linux, Ubuntu & DevOps Depth Verdict

- **Classification:** **Linux Command-Line & Cloud Container Deployment Specialist**
- **Evidence:**
  - Developed production Bash deployment automation (`deploy/gcp/deploy.sh`) managing Google Cloud Run, Secret Manager, IAM service accounts, and health assertions.
  - Containerization engineering with multi-stage Dockerfiles (`python:3.12-slim`, non-root system users, unprivileged execution).
  - Production-clone local debugging using containerized PostgreSQL databases (`erisk_dev`, `erisk_prod`), shell scripts, and Odoo CLI commands.
- **Historical Context:**
  - Initial Ubuntu desktop development at PT Arkana Solusi Digital: **USER-REPORTED** (consistent with Linux CLI proficiency).
  - Transition to macOS as primary development environment: **VERIFIED** by commit metadata (`.DS_Store`, file paths `/Users/mac/PycharmProjects/adhikarya/erisk-system`).

---

### 5. Alignment with Target Vacancy (Teryx Tech, UAE)

| Vacancy Requirement | Naufal's Forensic Match | Confidence | Gap / Action |
|---|---|---|---|
| **Odoo Development (3+ yrs)** | Hands-on since Dec 2023 (~2 yrs 10 mos). 160 commits on ERISK, leading FRD v1.0 and core security modules. | **HIGH** | Frame as *"Nearly 3 years of production Odoo engineering"* (do not state "4 years"). |
| **Python & Odoo ORM** | Advanced ORM inheritance, wizards, computed stored/nonstored fields, constraints, cache manipulation. | **HIGH** | Fully supported by code. |
| **XML & Web Frontend** | Extensive XML views, menus, action windows, QWeb templates, and modern Odoo 16 OWL JavaScript patch. | **HIGH** | Highlight modern OWL `FormController` patch (`risk_register_strategy_warning.js`). |
| **PostgreSQL** | Level 2: ORM + `psycopg2.sql` injection prevention + isolated cursors + raw SQL testing. | **HIGH** | Emphasize data integrity and transactional isolation. |
| **Construction / Project** | Construction handover workflows (PHO/FHO), contract boundaries, tender tracking, risk matrices. | **MEDIUM-HIGH** | Clarify construction governance context; do not claim native `project.project` WBS. |
| **HR Module** | Deep extensions to `hr.department` and `hr.employee` hierarchies, multi-tier approval integrations. | **HIGH** | Fully verified hands-on. |
| **Accounting Module** | **NOT FOUND** in ERISK repository. | **GAP** | Classified as USER-REPORTED from earlier Arkana tenure. Prepare accounting study guide. |
| **Ubuntu / Deployment** | Docker, Bash scripts, GCP Cloud Run, Odoo upgrade CLI, local containerized PostgreSQL. | **HIGH** | Highlight Linux CLI and container deployment capabilities. |
| **Language Fluency** | English: Verified through documentation and code. Arabic: User-reported professional fluency. | **HIGH** | **Major competitive advantage** for UAE/Middle East clients. |

---

### 6. Deliverable Directory Map

All detailed audit deliverables have been generated and saved outside the repository at:
`C:\Users\Naska Jaladewa\Documents\cv-evidence\erisk-odoo\`

1. [`01_EXECUTIVE_SUMMARY.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/01_EXECUTIVE_SUMMARY.md): Overall audit findings, verified achievements, and strategic verdicts.
2. [`02_TECHNICAL_ARCHITECTURE.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/02_TECHNICAL_ARCHITECTURE.md): Full module dependency map, model relationships, and data flow.
3. [`03_PERSONAL_CONTRIBUTIONS.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/03_PERSONAL_CONTRIBUTIONS.md): Granular forensics on Naufal's 160 commits, commit SHAs, and ownership scores.
4. [`04_CONSTRUCTION_PROJECT_EVIDENCE.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/04_CONSTRUCTION_PROJECT_EVIDENCE.md): Exact construction domain mapping vs native Odoo Project analysis.
5. [`05_POSTGRESQL_LINUX_AUDIT.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/05_POSTGRESQL_LINUX_AUDIT.md): Deep-dive into database levels, raw SQL, and Linux/DevOps forensics.
6. [`06_TERYX_REQUIREMENT_MATRIX.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/06_TERYX_REQUIREMENT_MATRIX.md): Detailed requirement-by-requirement gap analysis and interview answers.
7. [`07_CV_BULLETS_EN_ID.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/07_CV_BULLETS_EN_ID.md): ATS-optimized English CV bullets, Indonesian explanations, and 90-second pitch.
8. [`08_EVIDENCE_INDEX.csv`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/08_EVIDENCE_INDEX.csv): Audit trail with commit SHAs, file paths, line ranges, and confidence ratings.
9. [`09_OPEN_QUESTIONS.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/09_OPEN_QUESTIONS.md): Technical questions requiring Naufal's personal recollection to confirm historical details.
10. [`10_PUBLIC_PORTFOLIO_CASE_STUDY.md`](file:///C:/Users/Naska%20Jaladewa/Documents/cv-evidence/erisk-odoo/10_PUBLIC_PORTFOLIO_CASE_STUDY.md): Sanitized, enterprise-safe public case study suitable for LinkedIn or portfolio websites.

