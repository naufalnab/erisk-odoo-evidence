# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 06: Teryx Tech Vacancy Requirement-to-Evidence Matrix

- **Target Opportunity:** Remote Odoo Developer at Teryx Tech (Dubai, UAE)
- **Salary Band Reference:** AED 10,000 – 18,000 / month (depending on technical assessment)
- **Candidate:** Naufal Nabila
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Auditor Role:** International Technical Recruiter & Senior Odoo Technical Architect

---

### 1. High-Level Vacancy Match Dashboard

```
+--------------------------------------------------------------------------+
| REQUIREMENT                       | STATUS       | CONFIDENCE LEVEL      |
+--------------------------------------------------------------------------+
| 1. Hands-on Odoo Development     | MATCH        | HIGH (Nearly 3 Years) |
| 2. Python & Odoo ORM             | MATCH        | VERY HIGH             |
| 3. XML Views & Modern Web Client | MATCH        | VERY HIGH (inc. OWL)  |
| 4. PostgreSQL Database           | MATCH        | HIGH (Level 2 Deep)   |
| 5. Construction / Project ERP    | STRONG MATCH | HIGH (Governance/PHO) |
| 6. Odoo HR Workflows             | MATCH        | HIGH                  |
| 7. Odoo Accounting               | GAP          | UNVERIFIED IN ERISK   |
| 8. Ubuntu / Linux Deployment     | MATCH        | HIGH (Docker/Bash/GCP)|
| 9. Independent Delivery & English| MATCH        | HIGH (Written E2E)    |
| 10. Arabic Language Fluency      | BONUS ASSET  | HIGH (User-Reported)  |
+--------------------------------------------------------------------------+
```

---

### 2. Granular Requirement Breakdown & Interview Playbook

#### Requirement 1: Hands-on Odoo Development Experience (3+ Years Preferred)
- **Vacancy Expectation:** Proven production experience customizing Odoo, managing release cycles, and resolving real enterprise bugs.
- **Matching Naufal Evidence:**
  - Odoo career began around December 2023 at PT Arkana Solusi Digital, continuing through October 2026 at PT Nusa Teknologi Inovasi (~2 years 10 months / nearly 3 full years of production experience).
  - 160 authored commits on ERISK alone across 2025–2026, leading Change Requests 3.01–3.12.
- **Confidence:** **HIGH**.
- **Identified Gap / Risk:** Candidate is ~2 months shy of a strict 36-month calendar milestone.
- **Recommended CV Framing:** *"Nearly 3 years of production Odoo engineering experience across enterprise construction risk and HR systems."* (Never round up to 4 years).
- **Likely Interview Question:** *"Walk us through your day-to-day role in your most recent enterprise Odoo project."*
- **Suggested Truthful Answer:**
  > *"Over the past nearly three years, I have worked as a core backend and security Odoo engineer. In my recent work on the ERISK enterprise platform for Indonesian infrastructure clients like Adhi Karya, I was responsible for delivering core Change Requests: architecting multi-tier construction handover validation state machines, engineering enterprise SSO with brute-force rate limiting, and optimizing mathematical exposure computation engines. I work end-to-end from database modeling and security rules to automated Playwright testing and Cloud Run deployments."*

---

#### Requirement 2: Domain Knowledge — Odoo Construction & Project Modules
- **Vacancy Expectation:** Experience building or adapting Odoo workflows for construction contractors, engineering projects, or job costing.
- **Matching Naufal Evidence:**
  - Engineered construction project lifecycle automation in `erisk_system_validation/models/hr_department.py`:
    - Model extended with `category='project'`, `eat` (Contract Value / Nilai Kontrak), and `is_tender` (Tender proposal separation).
    - Multi-tier approval state machine governing construction handover milestones: `active` $\rightarrow$ `pho` (Provisional Hand Over) $\rightarrow$ `fho` (Final Hand Over).
    - Enforced mandatory rejection comment wizards (`ProjectStageCommentWizard`) and audit log archiving into Odoo chatter.
- **Confidence:** **HIGH (Construction ERP & Governance)**.
- **Identified Gap / Risk:** Does not directly customize native `project.project` or `project.task` (WBS).
- **Recommended CV Framing:** *"Engineered enterprise construction project governance and handover approval workflows (PHO/FHO) tied to contract values and risk matrices on Odoo."*
- **Likely Interview Question:** *"How did you handle project milestone handovers and contract risk in your construction projects?"*
- **Suggested Truthful Answer:**
  > *"In our construction risk system, projects were structured under an extended organizational model that tracked contract values (`eat`) and separated active construction from tender bids. I built the multi-tier validation system that governed the critical construction handovers—PHO (Provisional Hand Over) and FHO (Final Hand Over). The workflow prevented unauthorized stage skipping, required formal approval from assigned management tiers, enforced mandatory comment logging upon rejection via a custom wizard, and maintained an unalterable audit trail in the chatter."*

---

#### Requirement 3: Domain Knowledge — Odoo HR & Employee Modules
- **Vacancy Expectation:** Customization of HR departments, employee records, roles, and departmental hierarchies.
- **Matching Naufal Evidence:**
  - Deep extensions to `hr.department`: PIC assignment (`pic_id`), parent company rollups (`parent_name`), composite performance ratings (`komposit_ids`), and departmental risk lifecycle statuses.
  - Multi-company and department-level record rules enforcing data isolation across corporate subsidiaries (e.g., ADCP).
- **Confidence:** **HIGH**.
- **Identified Gap:** None. Fully supported by codebase.
- **Recommended CV Framing:** *"Extended Odoo HR data models (`hr.department`, `hr.employee`) to support multi-company hierarchies, department risk indexing, and role-based access rules."*

---

#### Requirement 4: Domain Knowledge — Odoo Accounting Module
- **Vacancy Expectation:** Invoicing, journal entries, tax handling, or chart of accounts.
- **Matching Naufal Evidence:**
  - **NOT FOUND** in the `erisk-system` repository.
  - Experience with accounting is **USER-REPORTED** from prior client work at PT Arkana Solusi Digital (invoicing, custom reporting, integrations).
- **Confidence:** **MEDIUM (User-Reported; Unverified in this Repo)**.
- **Identified Gap:** Must be addressed with honesty during technical interviews.
- **Recommended Action:**
  - Position prior Arkana accounting work on CV under the Arkana employment section.
  - Dedicate interview prep to reviewing Odoo 16/17 `account.move`, `account.move.line`, payment reconciliations, and journal entries.
- **Likely Interview Question:** *"Have you customized Odoo Accounting workflows, such as invoices or financial journals?"*
- **Suggested Truthful Answer:**
  > *"In my current project ERISK, our primary focus was enterprise risk governance, contract exposure, and construction milestone approvals. However, during my earlier work at PT Arkana Solusi Digital, I worked with standard Odoo business modules including invoicing and custom accounting reports. I understand the underlying `account.move` ORM architecture and can quickly apply my backend and workflow expertise to your accounting requirements."*

---

#### Requirement 5: Technical Skills — Python, PostgreSQL, XML & Web Framework
- **Vacancy Expectation:** Strong programming fundamentals, ORM mastery, custom views, and database skills.
- **Matching Naufal Evidence:**
  - **Python:** 160 commits writing advanced ORM models, transient wizards, custom exceptions (`ImpactScoreRangeError`), and mathematical helpers.
  - **XML:** Authored and refactored dozens of views (tree, form, search), action windows, `ir.cron` records, and email templates (`mail.template`).
  - **Web / JS:** Authored modern Odoo 16 OWL JavaScript patch (`risk_register_strategy_warning.js`) extending `FormController` via `@web/core/utils/patch`.
  - **PostgreSQL:** Level 2 engineering: `psycopg2.sql` injection prevention, isolated autocommit cursors (`cr = registry(dbname).cursor()`), composite SQL constraints, and cache invalidation testing.
- **Confidence:** **VERY HIGH**.
- **Likely Interview Question:** *"How do you ensure your custom SQL queries or ORM updates don't compromise security or dirty ongoing transactions?"*
- **Suggested Truthful Answer:**
  > *"When executing raw queries, I strictly avoid string concatenation. In our IZI Dashboard filtering integration, I used `psycopg2.sql.Identifier` along with strict alphanumeric whitelist regexes to completely prevent SQL injection. For audit logging in `api.log`, where failed authentication attempts must be preserved even if the main Odoo transaction rolls back, I opened an isolated cursor directly from the registry pool (`registry(dbname).cursor()`) with `autocommit=True` so logging persists independently of the parent transaction."*

---

#### Requirement 6: Infrastructure — Ubuntu Server, Deployment, Backups & DevOps
- **Vacancy Expectation:** Managing Odoo environments, running updates, setting up Docker, and taking routine backups.
- **Matching Naufal Evidence:**
  - Authored production Bash deployment scripts (`deploy/gcp/deploy.sh`) configuring Google Cloud Run, Secret Manager, and automated health smoke tests.
  - Containerization using Docker (`python:3.12-slim`, non-root execution).
  - Maintained local development and staging databases (`erisk_dev`, `erisk_prod`), executing schema upgrades and automated database backups (`pg_dump`).
  - Desktop Ubuntu Linux experience reported from Arkana tenure.
- **Confidence:** **HIGH**.
- **Recommended CV Framing:** *"Experienced in deploying and maintaining containerized Odoo environments on Linux/Docker, writing automated deployment scripts, and managing PostgreSQL database backups."*

---

#### Requirement 7: Soft Skills & Languages — English & Arabic Fluency
- **Vacancy Expectation:** Autonomous delivery, strong client communication, and cross-border collaboration in the UAE.
- **Matching Naufal Evidence:**
  - **English:** High technical proficiency evidenced by comprehensive code documentation, architectural markdown reports, commit messages, and Playwright test suites.
  - **Arabic:** Candidate reports professional fluency.
- **Strategic Impact:** **HUGE COMPETITIVE ADVANTAGE FOR UAE / DUBAI VACANCIES**.
  - While many international applicants speak English, finding an Odoo developer with solid backend engineering skills **and** professional Arabic fluency is exceptionally rare in the UAE market.
- **Recommendation:** Highlight Arabic and English fluency prominently at the very top of the CV profile summary!

