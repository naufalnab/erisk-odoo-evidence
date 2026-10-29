# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 04: Construction Industry & Odoo Project Evidence Analysis

- **Target Candidate:** Naufal Nabila
- **Target Opportunity:** International Remote Odoo Developer (Teryx Tech, UAE)
- **Primary Target Vacancy Requirement:** *"Domain Knowledge: Proven track record working with Odoo Construction/Project, HR, and Accounting modules."*
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Auditor Role:** Senior Odoo Technical Architect & International Technical Recruiter

---

### 1. The Core Architectural Question: How Does ERISK Model "Projects"?

In enterprise Odoo ecosystems, there are two primary ways development teams implement project functionality:
1. **Approach 1 (Native Project Addon):** Extending Odoo's native `project.project` and `project.task` models (standard Odoo Project / Timesheets / Field Service).
2. **Approach 2 (Custom Organizational Hierarchy):** Modeling enterprise construction projects as specialized organizational business units within `hr.department`, associating cost centers, contract boundaries, stage approvals, and risk profiles.

**Forensic Audit Finding:**
The ERISK system **exclusively uses Approach 2**. The core enterprise risk management features (governing infrastructure projects at state-owned enterprise PT Adhi Karya Tbk) do **not** link to or extend `project.project` or `project.task`. Instead, construction projects are first-class business entities modeled within an extended `hr.department` model.

```
       [ Odoo Standard ]                   [ ERISK Enterprise Architecture ]
       
    project.project (Model)                  hr.department (Inherited Model)
            |                                               |
     project.task (Model)                     +-------------+-------------+
            |                                 |                           |
    [Task WBS / Timesheet]          category='department'        category='project'
                                     (Corporate Dept)           (Construction Site)
                                                                          |
                                                      +-------------------+-------------------+
                                                      |                   |                   |
                                                eat (Nilai Kontrak)   is_tender       project_stage
                                                (Contract Value)    (Tender Phase)   (ACTIVE/PHO/FHO)
                                                                                              |
                                                                                      tier.validation
                                                                                   (Multi-Tier Approval)
```

---

### 2. Feature-by-Feature Forensic Classification

Every feature related to projects, construction, and risk governance is classified into one of four objective tiers:
- **Category A:** Direct Odoo Project implementation (demonstrably uses or extends `project.project` or `project.task`).
- **Category B:** Construction-industry ERP experience (business functionality supports construction operations but does not extend native Odoo Project).
- **Category C:** General risk management (functionality applicable across any enterprise domain).
- **Category D:** Unverified.

| Business Feature | Technical Implementation | Code Reference | Classification | Defensible Claim for CV |
|---|---|---|:---:|---|
| **Construction Project Data Model** | `hr.department` extended with `category='project'`, `eat` (Nilai Kontrak / Contract Value), `id_number` (No Proyek), and unique composite SQL constraint. | [`erisk_system/models/hr_department.py:39-91`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/hr_department.py#L39-L91) | **B** | *"Engineered enterprise project data models tracking contract value and site codes for state-owned construction projects."* |
| **Construction Handover Lifecycles (PHO / FHO)** | Multi-tier approval state machine transitioning projects: `active` $\rightarrow$ `pho` (Provisional Hand Over) $\rightarrow$ `fho` (Final Hand Over). Strict validation preventing unauthorized stage jumps, Super Admin reset safeguards, and mandatory rejection comment wizards. | [`erisk_system_validation/models/hr_department.py:16-198`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system_validation/models/hr_department.py#L16-L198) | **B** | *"Automated construction project handover lifecycles (PHO/FHO) using multi-tier approval state machines with audit logging."* |
| **Tender Project Governance** | Separate isolation of Tender projects (`is_tender=True`), dedicated menu/action filtering, and risk lifecycle exclusions from operational dashboards. | [`erisk_system/models/hr_department.py:49-80`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/hr_department.py#L49-L80) | **B** | *"Implemented tender risk governance, separating pre-bid proposals from active construction execution."* |
| **Project Risk Assessment & Register** | Enterprise risk register tied to construction projects, tracking financial exposure, mitigations, and Key Risk Indicators. | [`erisk_system/models/risk_register.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/risk_register.py) | **B & C** | *"Developed enterprise project risk registers and periodic monitoring workflows for large-scale engineering projects."* |
| **Quantitative Risk Scoring vs Contract Limits** | Dynamic mathematical scoring comparing estimated risk impact against corporate project limit thresholds: $\text{Ratio} = \text{Impact} / \text{Project Limit}$. | [`erisk_system/models/impact_scoring.py`](file:///c:/Users/Naska%20Jaladewa/Documents/naufal/erisk-system/erisk_system/models/impact_scoring.py) | **B & C** | *"Engineered quantitative risk evaluation algorithms benchmarking potential project losses against contract exposure boundaries."* |
| **Native Task Gantt / WBS Customization** | Third-party modules (`project_custom_gantt`, `project_extended_7d`) installed in repository. | `project_custom_gantt/`, `project_extended_7d/` | **A (Present in repo)** | **DO NOT CLAIM**: Authored by Cybrosys and 7D in 2023–2024; Naufal has 0 commits in these directories. |

---

### 3. Critical Verification of CV Terminology

An international technical recruiter or hiring manager at an enterprise like Teryx Tech will rigorously probe resume claims. Here is the verdict on common terms:

#### 1. "Odoo Project Customization"
- **Verdict:** **UNSUPPORTED / RISKY WITHOUT CLARIFICATION**.
- **Explanation:** In the Odoo ecosystem, "Odoo Project Customization" universally implies modifying `project.project`, `project.task`, timesheets, or agile sprint boards. Naufal did not customize native `project.project`.
- **Recommended Alternative:** *"Enterprise Project Governance & Workflow Customization on Odoo"*.

#### 2. "Construction Industry ERP Development"
- **Verdict:** **100% VERIFIED & FULLY JUSTIFIED**.
- **Explanation:** Naufal engineered core ERP workflows specifically designed for construction enterprise operations:
  - Contract Value tracking (`eat` / Nilai Kontrak).
  - Tender stage segregation (`is_tender`).
  - Formal Indonesian construction handover milestones: PHO (*Provisional Hand Over* / Serah Terima Pertama Pekerjaan) and FHO (*Final Hand Over* / Serah Terima Akhir Pekerjaan).
  - Integration with corporate state-owned infrastructure clients (PT Adhi Karya Tbk / PT Adhi Commuter Properti).
- **Recommended CV Phrasing:** *"Engineered enterprise Odoo ERP workflows for the Indonesian construction and infrastructure sector, implementing multi-tier PHO/FHO handover approvals and tender governance."*

#### 3. "Project Risk Management System"
- **Verdict:** **100% VERIFIED & FULLY JUSTIFIED**.
- **Explanation:** The entire application (ERISK) is an Enterprise Project Risk Management platform. Naufal was a primary developer of the risk assessment engine, probability/impact calculations, and treatment plan execution.

#### 4. "Enterprise Risk Management on Odoo"
- **Verdict:** **100% VERIFIED & FULLY JUSTIFIED**.
- **Explanation:** 160 commits directly building risk matrices, KRI tracking, LED (Loss Event Databases), strategic risk boundaries, and executive dashboards.

#### 5. "Construction Workflow Automation"
- **Verdict:** **100% VERIFIED & FULLY JUSTIFIED**.
- **Explanation:** The automated tier validation (`HrDepartmentProjectStageValidation`) completely automates the submission, review, approval, rejection, and restart cycles for construction project milestone handovers.

---

### 4. Strategic Interview Preparation: Bridging the "Odoo Project" Question

When interviewing with Teryx Tech, technical interviewers will likely ask:
> *"We see you worked on construction and project systems. Have you customized the native `project.project` and `project.task` models in Odoo?"*

#### Recommended Truthful Technical Response:
> *"In the ERISK enterprise platform for Adhi Karya, construction projects were governed at the enterprise portfolio level rather than through standard operational tasks. Because the client required unified financial exposure limits, multi-tier executive approvals, and tender phase separation across corporate departments and construction sites, we modeled construction projects as specialized project units within `hr.department`. I architected the multi-tier approval state machine governing the construction project lifecycle—from Active execution to PHO (Provisional Hand Over) and FHO (Final Hand Over)—integrated with contract values (`eat`) and enterprise risk registers. While native `project.task` is standard for daily task tracking, our focus was higher-level construction governance, contract limits, and risk mitigation workflows."*

This response demonstrates:
1. Deep architectural understanding of Odoo data modeling.
2. Honest, transparent representation of engineering work.
3. Sophisticated explanation of why an enterprise chose this specific architectural pattern.
4. Immediate credibility with Senior Architects and Technical Directors.

