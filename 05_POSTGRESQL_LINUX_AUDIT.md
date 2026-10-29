# Forensic Engineering Audit & Career Evidence Report: ERISK System
## Document 05: PostgreSQL Database & Linux / DevOps Audit

- **Target Candidate:** Naufal Nabila
- **Target Opportunity:** International Remote Odoo Developer (Teryx Tech, UAE)
- **Primary Vacancy Requirements:**
  - *"Technical Skills: Strong proficiency in Python, PostgreSQL, and XML."*
  - *"Infrastructure: Experience managing Odoo servers on Ubuntu (deployment, server optimization, backups)."*
- **Audit Baseline Commit (HEAD):** `7e0c035acfe8263c0fee0dc8b152f059a12bc63f`
- **Auditor Role:** PostgreSQL Engineer & Linux/DevOps Reviewer

---

### PART 1: PostgreSQL Deep-Dive

#### 1. Three-Tier Competency Assessment

| Level | Definition | Forensic Evidence in Code | Candidate Status |
|---|---|---|:---:|
| **Level 1: ORM / Database-Backed Development** | Defining models, relational fields (M2o, O2m, M2m), ORM CRUD (`search`, `create`, `write`, `unlink`), domain queries, computed stored/non-stored fields, and Python constraints. | Extensive: Designed models across `erisk_system`, `erisk_sso`, `erisk_system_validation`; wrote complex computed fields, SQL constraints (`unique (category, id_number)`), and Many2many relationship models. | **VERIFIED HANDS-ON (ADVANCED)** |
| **Level 2: Direct PostgreSQL Engineering** | Raw SQL queries via `env.cr.execute`, SQL injection defense (`psycopg2.sql`), isolated transaction cursors, cache invalidation, and direct SQL regression tests. | **Concrete Code Evidence:**<br/>1. `psycopg2.sql` dynamic query construction with strict alphanumeric whitelist regexes to guarantee zero SQL injection (`izi_dashboard/models/common/izi_dashboard.py:121-140`).<br/>2. Dedicated autocommit database cursor creation (`cr = registry(dbname).cursor()`, `cr._cnx.autocommit = True`) for audit log persistence during transaction failures (`erisk_sso/models/api_log.py:47-57`).<br/>3. Parametrized SQL execution (`cr.execute("UPDATE ...")`) and ORM cache invalidation (`record.invalidate_recordset()`) in `erisk_system/tests/test_frd_runtime.py:480-504`. | **VERIFIED HANDS-ON (SOLID)** |
| **Level 3: Database Administration & Tuning** | Production DBA, query performance optimization using `EXPLAIN ANALYZE`, index tuning (GIN/GiST/partial indexes), connection pooling (`pgbouncer`), vacuum tuning, replication, disaster recovery. | Not found in application repository. Basic `pg_dump` backup generation documented in local QA logs (`QA_301_DELETE_RISK_REGISTER.md:31`). No custom index tuning, server config tuning, or `EXPLAIN` analysis observed. | **NOT VERIFIED / DO NOT CLAIM AS PRIMARY EXPERTISE** |

---

#### 2. Detailed Code Evidence for Level 2 PostgreSQL Engineering

##### Evidence 1: SQL Injection Protection via `psycopg2.sql` and Whitelisting
In `izi_dashboard/models/common/izi_dashboard.py:113-140` (Commit `eb4702d`):
```python
# Regex validation to prevent SQL injection in dynamic identifier names
if (
    not table_name
    or not re.fullmatch(r'[A-Za-z_][A-Za-z0-9_]*', table_name)
    or not field_name
    or not re.fullmatch(r'[A-Za-z_][A-Za-z0-9_]*', field_name)
):
    raise ValidationError(_('The dashboard filter table or field name is invalid.'))

# Secure query composition using psycopg2.sql.Identifier and parameterized values
statement = sql.SQL(
    'SELECT {field} FROM {table}{where} GROUP BY {field} LIMIT %s'
).format(
    field=sql.Identifier(field_name),
    table=sql.Identifier(table_name),
    where=where_clause,
)
self.env.cr.execute(statement, query_values)
results = self.env.cr.dictfetchall()
```
*Analysis:* This demonstrates deep knowledge of PostgreSQL communication protocols. Instead of naive string formatting (`f"SELECT ..."`), Naufal used AST-safe `psycopg2.sql.Identifier` combined with strict pre-execution regex validation and positional parameter binding (`%s`).

##### Evidence 2: Transaction-Isolated Cursor Execution
In `erisk_sso/models/api_log.py:47-57` (Commit `1b8744e`):
```python
# Acquire an independent database cursor from the registry pool
db_name = self._cr.dbname
cr = registry(db_name).cursor()
cr._cnx.autocommit = True

insert_query = 'INSERT INTO api_log (%s) VALUES (%s) ' % (
    query_columns, query_placeholders)
cr.execute(insert_query, vals)
cr.close()
```
*Analysis:* When an authentication failure occurs in Odoo, the main transaction is rolled back by `AccessDenied`. To ensure failed login attempts and client IP addresses are permanently persisted for brute-force tracking, Naufal opened an independent autocommit cursor to bypass the parent rollback boundary.

##### Evidence 3: Cache Invalidation & Direct SQL Assertion
In `erisk_system/tests/test_frd_runtime.py:481-504` (Commit `fc2ce24`):
```python
# Directly update storage via raw SQL to simulate legacy database data
self.env.cr.execute(
    "UPDATE nsv_risk_register SET eksposur_risk_value = %s WHERE id = %s",
    (legacy_value, risk.id),
)
# Force Odoo ORM to dump its in-memory cache and re-read from PostgreSQL
risk.invalidate_recordset(["eksposur_risk_value", "impact_exposure_formula"])
self.env.cr.execute(
    "SELECT eksposur_risk_value FROM nsv_risk_register WHERE id = %s",
    (risk.id,),
)
self.assertAlmostEqual(self.env.cr.fetchone()[0], legacy_value)
```
*Analysis:* Demonstrates thorough understanding of Odoo ORM cache coherency vs PostgreSQL disk persistence.

---

#### 3. Recommended PostgreSQL CV Phrasing

- **Accurate & Persuasive Statements:**
  - *"Proficient in PostgreSQL database design, relational modeling, composite uniqueness constraints, and high-performance Odoo ORM queries."*
  - *"Experienced in direct PostgreSQL interaction using `psycopg2.sql` parameterized queries with SQL-injection prevention and dedicated autocommit cursors for transactional isolation."*
  - *"Skilled in database migration safety, ORM cache invalidation testing (`invalidate_recordset`), and routine backup/restore via `pg_dump`."*
- **Terms to Avoid on CV:**
  - *"PostgreSQL Database Administrator (DBA)"* (unsupported).
  - *"High-Availability PostgreSQL Clustering / Replication Engineer"* (unsupported).
  - *"Query Tuning via EXPLAIN ANALYZE"* (unless separately prepared for in interview questions).

---

### PART 2: Ubuntu, Linux & DevOps Audit

#### 1. Five-Level Classification of Linux / DevOps Experience

| Level | Definition | Forensic Verification |
|---|---|---|
| **1. Desktop Linux Development** | Using Ubuntu as daily OS, package management (`apt`), local virtualenvs, Git workflows. | **USER-REPORTED** for initial period at PT Arkana Solusi Digital. Compatible with Linux CLI competency demonstrated in scripts. |
| **2. Linux Command-Line Proficiency** | Terminal mastery, Bash scripting, process inspection, file permissions, SSH, environment variables. | **VERIFIED HANDS-ON**: Authored 209-line bash deployment script (`deploy/gcp/deploy.sh`) with strict flags (`set -euo pipefail`), regex parsing, and tool checks. |
| **3. Containerization (Docker)** | Dockerfile creation, multi-stage builds, non-root users, container networking, bind mounts. | **VERIFIED HANDS-ON**: Root `Dockerfile` using `python:3.12-slim`, unprivileged `gateway` system user, and containerized Odoo/PostgreSQL development environments (`erisk_dev`, `erisk_prod`). |
| **4. Cloud & Server Deployment** | Deploying Odoo/microservices to remote Linux servers, Cloud Run, Secret Manager, Nginx/reverse proxy. | **VERIFIED HANDS-ON**: Production deployment automation to Google Cloud Run (Jakarta region), health-check automation via `curl`, zero-downtime candidate tagging. |
| **5. Full Production SysAdmin / DBA** | Production systemd configuration, 24/7 uptime monitoring, log rotation, network firewalls, high-availability setups. | **PARTIALLY VERIFIED**: Shell-level service operations verified; full OS-level infrastructure responsibility remains unverified in repository. |

---

#### 2. Evidence of macOS as Primary Workstation

While Naufal started on Ubuntu at Arkana, repository forensic evidence proves his primary workstation transitioned to macOS:
1. **Repository Artifacts:** Commit history includes `.DS_Store` files committed during repository operations.
2. **Path Evidence in Commit Logs:** Commit `8b316c3` explicitly includes absolute paths from macOS:
   `file:///Users/mac/PycharmProjects/adhikarya/erisk-system/erisk_sso/controllers/main.py`.
3. **Implication for Recruiter:** Naufal is a cross-platform engineer fluent in both **Ubuntu/Linux environments** (for server deployment and containerized development) and **macOS** (for local development productivity).

---

#### 3. Recommended Ubuntu / Linux / DevOps CV Phrasing

- **For CV Summary & Skills:**
  - *"DevOps & Infrastructure: Linux / Ubuntu CLI, Docker containerization, Google Cloud Run, Secret Manager, Bash shell scripting, automated deployment pipelines."*
- **For CV Experience Bullets:**
  - *"Authored automated zero-downtime Bash deployment pipelines deploying containerized microservices to Google Cloud Run with Secret Manager key rotation and health assertion suites."*
  - *"Managed containerized Odoo 16 development and staging environments on Linux/Docker, maintaining database dumps, schema upgrades, and regression testing pipelines."*

