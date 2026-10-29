# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 03: Forensic Analysis of Personal Contributions (Naufal Nabila)

- **Target Candidate:** Naufal Nabila
- **Identified Git Identities:**
  1. `naufalnab <naufalnabila27@gmail.com>` (124 commits)
  2. `Naufal Nabila <73274962+naufalnab@users.noreply.github.com>` (31 commits)
  3. `naufalnabila <naufal.nabila.2008@gmail.com>` (5 commits)
  - **Total Personally Authored Commits:** **160 commits** (30.5% of repository total).
- **Target Repository:** `https://github.com/nusativ/erisk-system`
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Auditor Role:** Git Forensics Analyst & Senior Odoo Technical Architect

---

### 1. Contribution Overview & Development Timeline

Naufal Nabila's verified work spans two critical eras of the repository's lifecycle:

```
[2023-11: Bootstrapped by Amirul/Galang] 
   |
[2024: Base Tables & Core Views Built]
   |
   +---> [2025-08 to 2025-09: Phase 1 — Naufal Joins]
   |     * Residual exposure calculation fixes (commits e7371c2, 4ef3072, eed3144).
   |     * Sudo elevation on strategic risk lookup to bypass record-rule deadlocks.
   |
   +---> [2026-02 to 2026-03: Phase 2 — Security & SSO Architecture]
   |     * Full implementation of `erisk_sso` (85 commits).
   |     * Password policy enforcement, expiry crons, brute-force logging, X-Frame-Options.
   |
   +---> [2026-05 to 2026-08: Phase 3 — AI Gateway & Cloud Run Microservices]
   |     * Google Vertex AI / Gemini integration with IZI Dashboard.
   |     * Microservice architecture (`izi_gemini_gateway`), Pydantic v2 schemas, GCP deployment bash script.
   |
   +---> [2026-10: Phase 4 — FRD v1.0 Delivery, Construction Workflows & Local QA]
         * Delivery of CR 3.01–3.12 (commits eb4702d, 4aba9af, 219f786, 84474a2, dc5c1fd, 7e0c035).
         * Construction PHO/FHO handover tier validation, unlinking guards, mathematical exposure parity.
         * QA regression harness (`test_frd_runtime.py`, Playwright suites).
```

---

### 2. Deep-Dive Contribution Breakdown

#### Contribution 1: Construction Project Handover & Lifecycle Approval State Machine (CR 3.05)
1. **Feature Name:** Project Lifecycle & Multi-Tier Construction Handover Validation (`active` $\rightarrow$ `pho` $\rightarrow$ `fho`).
2. **Business Problem Solved:** Construction projects operate in distinct contractual stages. State-owned enterprise governance requires strict multi-level management sign-off before a project moves from Active construction to Provisional Hand Over (PHO) and Final Hand Over (FHO), with audit-trailed rejection reasons.
3. **Naufal's Specific Responsibilities:** Designed the tier-validation state machine on `hr.department`, wrote the transition constraints, Super Admin rollback gates, and rejection wizard dialogs.
4. **Technical Implementation:**
   - Inherited `hr.department` with `tier.validation` in `HrDepartmentProjectStageValidation`.
   - Overrode `write()` to block unauthorized stage updates and enforce complete tier reviews.
   - Built `action_request_project_stage()` and `action_return_project_to_active()`.
   - Created `ProjectStageCommentWizard` inheriting `comment.wizard` to mandate comments upon rejection.
   - Preserved review histories across stage transitions via `_archive_project_stage_reviews()`.
5. **Relevant Files & Code References:**
   - [`erisk_system_validation/models/hr_department.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system_validation/models/hr_department.py#L16-L198)
   - `erisk_system_validation/data/tier_project_stage.xml`
6. **Commit & PR Evidence:**
   - Commit `eb4702df502ab8e973c63741e4ab02b42aa87bab` (2026-10-20)
   - Commit `4aba9af1a2f5ff0439b7244ae7be26730f064ec5` (2026-10-21)
   - Commit `219f786f076a9b5c2bddcd8338a2c165b3c934ee` (2026-10-21)
7. **Tests & Validation:** Verified in `erisk_system_validation/tests/test_frd_workflows.py` (passes stage request, rejection, unauthorized denial, and Super Admin reset).
8. **Development Timeframe:** October 2026.
9. **Ownership Confidence:** **HIGH (Sole Author of Implementation & Tests)**.

---

#### Contribution 2: Enterprise Password Security, Brute-Force Defense & Session Hardening
1. **Feature Name:** Enterprise SSO, Strong Password Enforcement, and Attack Mitigation.
2. **Business Problem Solved:** ERISK holds high-confidentiality risk registries and state-owned enterprise financials. It required strict enterprise-grade authentication defenses against credential stuffing, brute force, and unauthorized session embedding.
3. **Naufal's Specific Responsibilities:** Full end-to-end architecture of `erisk_sso`, covering backend validation, transient database logging, cron automation, controller hooks, and HTTP response headers.
4. **Technical Implementation:**
   - **Password Complexity Engine:** Overrode `res.users._set_password()` to enforce regex rules (length $\ge 8$, uppercase, lowercase, numbers, symbols) while safely bypassing installer/module-update modes (`tools.config['init']`).
   - **Brute Force & Rate Limiter:** Added IP tracking via `request.httprequest.environ.get('REMOTE_ADDR')` and implemented isolated cursor writes (`cr = registry(dbname).cursor()`, `cr._cnx.autocommit = True`) to persist audit logs even during transaction rollbacks.
   - **Configurable Expiry & Reminders:** Added `password_last_update`, automated `ir.cron` job calculating days-to-expire against `ir.config_parameter`, and dispatched Indonesian QWeb notifications via `mail.template`.
   - **Web Security Hardening:** Monkey-patched `odoo.http.Response` initialization to enforce `X-Frame-Options: SAMEORIGIN` globally; sanitized `redirect` parameters in `/web/login` and `/web/reset_password` to eliminate Open Redirect flaws.
   - **Modern Client-Side UX:** Added client-side JavaScript popup validation in reset/change password forms to prevent jarring page reloads.
5. **Relevant Files & Code References:**
   - [`erisk_sso/models/model.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_sso/models/model.py) (46 commits)
   - [`erisk_sso/controllers/main.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_sso/controllers/main.py#L95-L140) (27 commits)
   - [`erisk_sso/models/api_log.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_sso/models/api_log.py#L24-L60)
   - `erisk_sso/data/mail_template_data.xml`, `erisk_sso/data/ir_cron_data.xml`
6. **Commit & PR Evidence:**
   - Commit `1b8744e32dfd0b43b0eebf94b302b62365bd24b5` (2026-02-05)
   - Commit `8d8078f1618367980b1e428453ef69fe319b2657` (2026-02-05)
   - Commit `398fbb028e938923d8eb2c7b5eb972688d6ceb56` (2026-02-06)
   - Commit `8b316c3ff22ccf6bcf26b0823d2f74e38bbc7c61` (2026-03-12 / PR #156)
7. **Tests & Validation:** Unit tests in `erisk_sso/tests/test_login_security.py`; HTTP smoke test passing with status 200/403.
8. **Development Timeframe:** February – March 2026.
9. **Ownership Confidence:** **HIGH (Sole Author of Security Subsystem)**.

---

#### Contribution 3: Deterministic Quantitative Impact Scoring & Mathematical Exposure Modeling (CR 3.06, 3.08)
1. **Feature Name:** Deterministic Impact Score Interval Selector & Exposure Calculation Parity.
2. **Business Problem Solved:** Quantitative risk assessments previously suffered from non-deterministic score range lookups and floating-point boundary errors when evaluating financial impact against corporate risk limits.
3. **Naufal's Specific Responsibilities:** Designed the mathematical helper module, implemented strict boundary checks, eliminated floating-point rounding ambiguities, and decoupled stored financial data from display strings.
4. **Technical Implementation:**
   - Authored `choose_impact_score_range()` in `erisk_system/models/impact_scoring.py`:
     - Enforced lower-inclusive, upper-exclusive range matching ($[low, high)$).
     - Handled exact upper boundary matching with floating-point tolerance (`math.isclose(..., rel_tol=1e-12)`).
     - Raised deterministic `ImpactScoreRangeError` for configuration gaps, overlaps, negative impacts, or zero denominators.
   - Refactored `erisk_system/models/risk_register.py` and `risk_update.py` exposure formulas:
     $$\text{Eksposur} = \text{Skala Dampak} \times \text{Nilai Probabilitas (Midpoint)} \times 1\% \times \text{Batasan Risiko}$$
   - Added cache-invalidation tests (`record.flush_recordset()`, `record.invalidate_recordset()`) verifying that updating formula text never mutates persisted database storage.
5. **Relevant Files & Code References:**
   - [`erisk_system/models/impact_scoring.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/impact_scoring.py#L1-L56)
   - [`erisk_system/models/risk_register.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/risk_register.py#L435-L549)
   - [`tests/test_impact_scoring_helper.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/tests/test_impact_scoring_helper.py#L1-L53)
6. **Commit & PR Evidence:**
   - Commit `eb4702df502ab8e973c63741e4ab02b42aa87bab` (2026-10-20)
   - Commit `219f786f076a9b5c2bddcd8338a2c165b3c934ee` (2026-10-21)
   - Commit `b1884144577da3b2b8e3a39e9f60bc9fc850d52b` (2026-10-22)
   - Commit `84474a2cb586e9e4f45447c4e207904d9c7e0f22` (2026-10-23)
7. **Tests & Validation:** 7 deterministic unit tests passing in `tests/test_impact_scoring_helper.py`; full parity test in `erisk_system/tests/test_frd_runtime.py:460-504`.
8. **Development Timeframe:** October 2026.
9. **Ownership Confidence:** **HIGH (Sole Author of Helper & Parity Logic)**.

---

#### Contribution 4: Google Cloud Vertex AI Gateway & FastAPI Integration
1. **Feature Name:** Microservice Integration for AI-Powered BI Dashboards (`izi_gemini_gateway`).
2. **Business Problem Solved:** Odoo 16 lacked a native asynchronous interface to Google Vertex AI for dashboard commentary generation, and direct client API key storage created security and cost risks.
3. **Naufal's Specific Responsibilities:** Designed and built a standalone FastAPI microservice, Pydantic v2 schemas, in-memory rate limiting, WSGI/ASGI adapters, and a zero-downtime GCP Cloud Run deployment script.
4. **Technical Implementation:**
   - Built FastAPI app with Pydantic v2 validation models (`CompatibilityModel`, `APIKeyRequest`).
   - Implemented `SlidingWindowRateLimiter` to bound API costs.
   - Wrote `main.py` using `a2wsgi` and `functions_framework` to allow running the FastAPI app seamlessly inside Google Cloud Functions/Run.
   - Authored `deploy/gcp/deploy.sh` (209 lines of Bash): automated secret provisioning, IAM binding, candidate traffic tagging, health assertions via `curl`, and rolling traffic shifts.
5. **Relevant Files & Code References:**
   - [`izi_gemini_gateway/app.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/izi_gemini_gateway/app.py)
   - [`izi_gemini_gateway/models.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/izi_gemini_gateway/models.py)
   - [`main.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/main.py)
   - [`deploy/gcp/deploy.sh`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/deploy/gcp/deploy.sh)
6. **Commit & PR Evidence:**
   - Commit `aa2b059a373728e076713a9912e986f4271ef97f` (2026-08-05)
   - Commit `33633cb8e29a989ef3274be349e59d95447a164b` (2026-08-05)
   - Commit `8998b4566c7219cb86bf8f479d47915525a7d32c` (2026-05-13)
7. **Tests & Validation:** Unit tests in `tests/test_analysis_config.py`, `tests/test_gateway_contract.py`, and `tests/test_cloud_function_adapter.py`.
8. **Development Timeframe:** May – August 2026.
9. **Ownership Confidence:** **HIGH (100% Personal Authorship of Gateway Subsystem)**.

---

#### Contribution 5: Core Governance & Soft-Delete Workflow (CR 3.01)
1. **Feature Name:** Audited Risk Register Deletion Request & Unlink Prevention.
2. **Business Problem Solved:** Users previously could accidentally delete active or finalized risk registers, destroying enterprise audit records.
3. **Naufal's Specific Responsibilities:** Overrode Odoo's native ORM `unlink()` to block hard deletion, created a structured "Request Deletion" approval workflow, and verified multi-role permissions.
4. **Technical Implementation:**
   - In `erisk_system/models/risk_register.py`:
     - Blocked physical deletion of non-draft records with explicit error messaging.
     - Implemented `action_request_delete()`: restricted to record creator (`create_uid`), transitioning record to `approval_process_delete`.
     - Reserved final deletion/archive approval exclusively for Super Admin.
5. **Relevant Files & Code References:**
   - [`erisk_system/models/risk_register.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/risk_register.py#L380-L410)
   - `docs/QA_301_DELETE_RISK_REGISTER.md`
6. **Commit & PR Evidence:**
   - Commit `506e18fa4ad20387b32d1efbc64fe461b2067420` (2026-10-28)
   - Commit `dc5c1fd5aa1626f6eb5eef8677c3e53fa65239e0` (2026-10-28)
   - Commit `38bd81c96b8a8b19356396e8fa134d4ca9d54316` (2026-10-28)
7. **Tests & Validation:** Verified with `qa/eriska/scripts/verify_delete_request.py` (37 regression checks passed in local Odoo shell).
8. **Development Timeframe:** October 2026.
9. **Ownership Confidence:** **HIGH (Sole Author of Implementation and QA Script)**.

---

#### Contribution 6: Modern Odoo 16 OWL JavaScript Patch for Strategy Warnings
1. **Feature Name:** Dynamic Client-Side Strategy Warning Controller Patch (CR 3.03).
2. **Business Problem Solved:** When users created a risk register in a year without an active approved strategy, the backend fell back to prior-year parameters silently. Users needed immediate, non-blocking sticky warnings directly in the web client without losing draft form data.
3. **Naufal's Specific Responsibilities:** Created an OWL `FormController` patch intercepting save events and triggering asynchronous ORM queries.
4. **Technical Implementation:**
   - Patched `FormController.prototype` using `@web/core/utils/patch`.
   - Intercepted `saveButtonClicked()` to invoke `orm.call("nsv.risk.register", "get_strategy_warning")`.
   - Dispatched sticky notifications via Odoo's notification service.
5. **Relevant Files & Code References:**
   - [`erisk_system/static/src/js/risk_register_strategy_warning.js`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/static/src/js/risk_register_strategy_warning.js#L1-L40)
6. **Commit & PR Evidence:**
   - Commit `7e0c035acfe8263c0fee0dc8b152f059a12bc63f` (2026-10-29 / HEAD)
7. **Tests & Validation:** Verified on local Odoo 16 web client instance.
8. **Development Timeframe:** October 2026.
9. **Ownership Confidence:** **HIGH (Sole Author)**.

