# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 09: Open Questions & Candidate Verification Checklist

- **Target Candidate:** Naufal Nabila
- **Target Opportunity:** International Remote Odoo Developer (Teryx Tech, UAE)
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Auditor Role:** Senior Technical Architect & Career Portfolio Strategist

---

### Purpose of this Document

This audit has strictly adhered to **read-only forensic investigation** of accessible code, commits, and documentation. While Naufal's technical contributions in ERISK are comprehensively established, certain historical, operational, and client-confidentiality aspects cannot be verified from git commits alone. 

Naufal should review the following questions to solidify his interview preparation and CV accuracy.

---

### 1. Prior Accounting Experience at PT Arkana Solusi Digital

- **Context:** The target vacancy at Teryx Tech specifically lists **Odoo Accounting** alongside Construction and HR. The `erisk-system` repository contains **zero accounting models** (`account.move` is only present in two commented-out lines).
- **Questions for Naufal:**
  1. *During your tenure at PT Arkana Solusi Digital (starting ~Dec 2023), what specific Odoo Accounting modules or workflows did you develop or customize?* (e.g., Invoicing, Vendor Bills, Payment Gateways, Indonesian e-Faktur tax integration, custom financial reporting with QWeb or `report_xlsx`?)
  2. *Which Odoo versions did you work on at Arkana? (Odoo 15, 16, or 17? Community or Enterprise?)*
  3. *Do you have code samples, pull requests, or reference architecture notes from those projects that you can review prior to technical interviews?*
- **Actionable Advice:** If you customized invoicing or accounting at Arkana, add 1–2 specific bullets under your Arkana Solusi Digital CV entry. In interviews, state clearly: *"In ERISK, our focus was project risk and governance, but at Arkana I worked directly on core invoicing and financial report customization."*

---

### 2. Production Server Access & Linux Infrastructure Boundary

- **Context:** In `docs/INVESTIGASI_NEXT_STEP_4824.md` and `docs/QA_301_DELETE_RISK_REGISTER.md`, testing is documented on local Docker containers (`erisk-local-investigation-db-1`, `erisk_dev`, and a local clone of `erisk_prod`), with explicit statements that direct production access was restricted.
- **Questions for Naufal:**
  1. *Did you have direct SSH access to the staging or production Ubuntu servers at Nusativ or Adhi Karya, or was deployment handled via a dedicated DevOps engineer / CI/CD runner?*
  2. *Did you manage Nginx reverse proxies, systemd services (`systemctl restart odoo`), SSL certificates (Certbot/Let's Encrypt), or automated database cron backups on remote Linux servers?*
  3. *What was your exact role in the GCP Cloud Run deployment (`deploy/gcp/deploy.sh`)—did you execute this against live GCP infrastructure or was it tested on a development project?*
- **Actionable Advice:** If your server deployment was collaborative or limited to development/staging environments, describe yourself as *"Proficient in Linux development, Docker containerization, and automated deployment scripting"*, rather than claiming *"Lead 24/7 Production Systems Administrator"*.

---

### 3. Client Name Disclosure & NDA Sensitivity (Adhi Karya / Nusativ)

- **Context:** Multiple documentation files in the repository explicitly name **PT Adhi Karya Tbk** and **PT Adhi Commuter Properti (ADCP)**. However, client confidentiality agreements (NDAs) often restrict public disclosure on LinkedIn or public resumes.
- **Questions for Naufal:**
  1. *Is PT Adhi Karya Tbk publicly acknowledgeable as a client on your CV or LinkedIn profile, or does your agreement with PT Nusa Teknologi Inovasi (Nusativ) require anonymization?*
  2. *Is the ERISK platform publicly referenced in client press releases or Nusativ portfolios?*
- **Actionable Advice:**
  - **For Private Direct Applications (e.g., direct email to Teryx Tech recruiters):** Naming *"Enterprise Risk Management System for PT Adhi Karya Tbk (via PT Nusa Teknologi Inovasi)"* is standard industry practice.
  - **For Public Platforms (LinkedIn, Personal Website, Public GitHub):** Use sanitized phrasing: *"Enterprise Risk Management & Construction Governance Platform for a leading Indonesian State-Owned Infrastructure Enterprise."*

---

### 4. AI-Assisted Development & Coding Tooling Attribution

- **Context:** Commit `8b316c3` contains a link formatted as `cci:1://file:///Users/mac/PycharmProjects/...`, which indicates the use of an AI-assisted IDE (such as Cursor or PyCharm AI Assistant). Commit subjects also show structured semantic styles.
- **Questions for Naufal:**
  1. *What AI tools (e.g., Cursor, GitHub Copilot, Claude Code) did you incorporate into your engineering workflow?*
  2. *How do you typically explain your use of AI tools in technical interviews?*
- **Actionable Advice:** Modern international recruiters and engineering leaders (especially in innovative tech hubs like the UAE) view developers who effectively leverage AI tools as **highly productive 10x engineers**, provided they deeply understand the underlying code. 
  - *How to pitch this:* *"I actively leverage modern AI-assisted engineering tools (like Cursor and LLM coding assistants) to accelerate test generation, documentation, and boilerplate refactoring, while maintaining strict architectural ownership, security validation, and deterministic mathematical accuracy."*

---

### 5. Verification Checklist for Candidate Prior to Interview

- [ ] Confirm Arkana Solusi Digital exact start and end dates (Month/Year).
- [ ] Review Odoo 16 native `account.move` and `account.move.line` models to refresh standard accounting terminology.
- [ ] Practice explaining the difference between native `project.project` and the ERISK `hr.department` construction governance model in English.
- [ ] Practice delivering the 75-second elevator pitch (from Document 07) out loud in English.
- [ ] Prepare 1–2 sentences in Arabic highlighting your technical background for UAE interviewers.

