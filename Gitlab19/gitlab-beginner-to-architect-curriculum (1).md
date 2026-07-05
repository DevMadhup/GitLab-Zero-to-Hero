<div align="center">

# 🦊 GitLab: Beginner to Architect
### The Complete Curriculum (v2)

![GitLab](https://img.shields.io/badge/GitLab-19.x-FC6D26?style=for-the-badge&logo=gitlab&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner_to_Architect-8E44AD?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-12--15_Months-2E8B57?style=for-the-badge)
![AI](https://img.shields.io/badge/Duo_Agent_Platform-Included-1F75FE?style=for-the-badge)
![Air Gapped](https://img.shields.io/badge/Air--Gapped-Fully_Covered-333333?style=for-the-badge)

</div>

> **Starting point:** Git basics known (add/commit/push)
> **Goal:** Full architect — admin + scaling + security + AI + **all available certifications** — nothing skipped
> **Covers:** GitLab 19.x current state, GitLab Duo Agent Platform (DAP)/AI end-to-end, and **both** internet-connected and air-gapped (offline) environments for every phase where it matters

⏱️ **Total estimated time:** 12–15 months at 8–10 hrs/week. This is an exhaustive path — treat it as a reference to work through phase by phase, not something to rush.

🔒 **How air-gapped is handled:** Rather than one bolt-on chapter, every phase from Phase 5 onward has an explicit **Air-Gapped Variant** section, because in the real world you don't learn air-gapped separately — you learn how each capability degrades/adapts without internet. Phase 11 makes you rebuild the entire stack fully offline as a capstone.

---

## 📋 Table of Contents

| # | Phase | Weeks | Focus |
|---|---|---|---|
| 0 | [Lab Setup](#phase-0--lab-setup-week-1) | 1 | Connected + air-gapped lab environments |
| 1 | [Git Deep Dive](#phase-1--git-deep-dive-weeks-2-3) | 2–3 | Internals, branching strategy, recovery |
| 2 | [Core GitLab Workflows](#phase-2--core-gitlab-workflows-weeks-4-6) | 4–6 | Groups, MRs, issues, boards |
| 3 | [CI/CD Mastery](#phase-3--cicd-mastery-weeks-7-11) | 7–11 | Pipelines, runners, registries |
| 4 | [DevSecOps & Security](#phase-4--devsecops--security-weeks-12-16) | 12–16 | Scanning, Secrets Manager, compliance |
| 5 | [GitLab Duo & Agent Platform](#phase-5--gitlab-duo--the-duo-agent-platform-ai-weeks-17-22) | 17–22 | AI, agents, flows, MCP, self-hosted models |
| 6 | [Administration / Self-Managed](#phase-6--gitlab-administration--self-managed-weeks-23-27) | 23–27 | Install, architecture, backup/DR |
| 7 | [Scaling, HA & Geo](#phase-7--scaling-high-availability--geo-weeks-28-33) | 28–33 | Gitaly Cluster, Geo, ClickHouse |
| 8 | [Enterprise Architecture](#phase-8--enterprise-architecture-governance--integrations-weeks-34-38) | 34–38 | Multi-tenant, GitOps, governance |
| 9 | [GitLab 19 Deep-Dive Review](#phase-9--gitlab-19-deep-dive-review-week-39) | 39 | Consolidate version-specific knowledge |
| 10 | [Certification Path](#phase-10--certification-path-complete-all-available) | — | Every available GitLab certification |
| 11 | [Capstone: Air-Gapped Rebuild](#phase-11--capstone-full-air-gapped-rebuild-weeks-40-44) | 40–44 | Full offline stack, zero internet |

---

## Phase 0 — Lab Setup `Week 1`

Build **two parallel environments** from day one so you internalize the difference early.

### 🌐 A. Internet-connected lab
1. GitLab.com SaaS account (free tier) — Phases 1–4
2. Local self-managed instance via Docker:
   ```bash
   docker run --detach --hostname gitlab.local -p 8080:80 -p 2222:22 --name gitlab gitlab/gitlab-ce:latest
   ```
3. Kubernetes sandbox:
   ```bash
   kind create cluster --name gitlab-lab
   ```
4. Free-tier cloud account (Oracle Cloud Free Tier / AWS / GCP credits) for Phase 7+ HA work

### 🔒 B. Air-gapped simulation lab
*(build now, use from Phase 5 onward)*

- A second VM with **no internet route** (block outbound via firewall rule or a host-only VirtualBox/VMware network) — this is your "air-gapped" target
- A separate **bastion/jump VM** that *does* have internet — your download/staging host, mirroring how real air-gapped transfers work (download on connected host → transfer via shared folder/USB image → install on disconnected host)
- This two-VM pattern is the actual architecture you'll use in Phase 11, so build it right the first time

> ✅ **Deliverable:** SaaS account, connected Docker GitLab, kind cluster, AND a disconnected VM + bastion pair ready for later phases.

---

## Phase 1 — Git Deep Dive `Weeks 2–3`

- Object model: blobs, trees, commits, refs (`git cat-file`, `git ls-tree`, `.git` internals)
- Branching strategies: Git Flow, GitHub Flow, Trunk-Based Development, GitLab Flow — when each fails at scale
- Rebase vs merge vs squash, interactive rebase, cherry-pick, reflog recovery, bisect
- Submodules vs subtrees vs monorepos — the decision matters hugely at architect level
- `.gitattributes`, `.gitignore`, Git LFS for large files
- Signed commits (GPG/SSH signing) — required for compliance later

> 🏁 **Capstone:** Recover a corrupted repo via reflog + fsck; build the same project as both monorepo and multi-repo-with-submodules, document trade-offs.

---

## Phase 2 — Core GitLab Workflows `Weeks 4–6`

- Projects, Groups, Subgroups — permission inheritance model
- Roles: `Guest` → `Reporter` → `Developer` → `Maintainer` → `Owner`, **custom roles**
- Merge Requests: approval rules, `CODEOWNERS`, merge trains, squash, draft MRs, **Rapid Diffs** (GitLab 19 beta — faster MR diff loading/scrolling)
- Issues, labels, milestones, boards, epics, roadmaps
- Wikis (now with emoji reactions as of 19.1), Snippets, Releases, Tags
- Notifications, webhooks, Slack/Jira integrations

> 🏁 **Capstone:** Group → subgroups → 2 projects, full label/board taxonomy, protected branches, MR approval rules, CODEOWNERS enforced.

---

## Phase 3 — CI/CD Mastery `Weeks 7–11`

- `.gitlab-ci.yml` fundamentals: stages, jobs, `script`, `rules` (modern standard, not `only/except`)
- Variables: project/group/instance level, protected & masked variables, predefined variables
- **Artifacts vs Cache** — artifacts = pipeline output shared across jobs/stages; cache = speed optimization, not guaranteed, scoped to runner
- `needs:` for DAG pipelines vs sequential stage execution
- `extends`, YAML anchors, `include` (local/project/remote/template)
- Multi-project & parent-child pipelines, `trigger:`
- Runners: shared/group/project runners, tags, autoscaling (Docker Machine / Kubernetes executor) — **set up your own Kubernetes-executor runner on your kind cluster**
- Environments & deployments, review apps, deployment freezes, rollback, `when: manual/delayed`
- **Scheduled pipeline execution policies** (GitLab 19.1, beta) — enforce recurring jobs org-wide independent of commits
- Docker-in-Docker vs Kaniko/Buildah
- Container Registry, Package Registry (npm, Maven, PyPI, Go, NuGet, Conan, generic)
- Pipeline efficiency: parallel matrix jobs, `interruptible`, DAG optimization

> 🏁 **Capstone:** Full pipeline: lint → parallel test matrix → build container → push to registry → deploy to kind via review app → manual promote. Under 3 minutes end-to-end.

> 🎓 **Certification checkpoint:** Ready for **GitLab Certified CI/CD Associate** here.

---

## Phase 4 — DevSecOps & Security `Weeks 12–16`

- Built-in scanners: SAST, DAST, Secret Detection, Dependency Scanning, Container Scanning, IaC Scanning, License Compliance
- **Dependency Scanning — Default profile** (GitLab 19): unified SCA control surface without editing CI YAML per project; MR-pipeline scans show only new vulns, branch-pipeline scans give full posture
- **Secret detection full-history scanning** (19.1): now scans every commit from a branch's divergence point, not just the latest push — closes a major historical-leak gap
- **Secret false positive detection via Duo** (19.1, GA): AI auto-assesses critical/high secret findings for false positives directly in the vulnerability report
- Security dashboards: advanced vulnerability management with trends, severity views, risk scores, CWE insights, PDF export
- Security policies: scan execution policies, MR approval policies (block merge on critical vuln)
- **GitLab Secrets Manager** (19.0, public beta, Premium/Ultimate): credentials stored and scoped inside GitLab itself, tied to group/project permissions and audit trail; works alongside Vault/AWS Secrets Manager/Azure Key Vault/GCP Secret Manager
- **Compliance Center**: create frameworks from template — **ISO 27001:2022, SOC 2, FedRAMP, NIST, CIS, TISAX**, and more (19 templates as of 19.1) — apply org-wide in one step
- Audit events, compliance pipelines enforced across groups
- SBOM generation, supply chain visibility

<details>
<summary>🔒 <b>Air-Gapped Variant — click to expand</b></summary>

- Scanners normally pull analyzer images from GitLab's container registry — in offline mode you must pre-pull, package as tar, transfer, and load into a local offline registry
- Use the vendored `Secure-Binaries.gitlab-ci.yml` template in a connected environment to download all scanner images as job artifacts, then transfer and load
- Vulnerability/CVE signature databases also need periodic manual refresh via the same download→transfer→load cycle
- Compliance framework templates and Secrets Manager work locally without issue (no external calls); only the *scanner engines'* signature updates are the offline-sensitive piece

</details>

> 🏁 **Capstone:** Full scanning suite + MR approval policy blocking on critical vulns + one compliance framework applied to a group + SBOM generated. Then repeat the scanner setup on your air-gapped VM using pre-transferred images.

> 🎓 **Certification checkpoint:** Maps to **GitLab Certified Security Associate**.

---

## Phase 5 — GitLab Duo & the Duo Agent Platform (AI) `Weeks 17–22`

This is now one of the largest and fastest-moving parts of GitLab — treat it as its own discipline, not an add-on.

### 5.1 Foundations
- GitLab Duo tiers: Duo Core, Duo Pro, Duo Enterprise — know what's gated where
- Duo Code Suggestions (inline completion) in supported IDEs and GitLab Web IDE
- **GitLab editor extensions**: bring GitLab + Duo into VS Code / JetBrains without leaving your editor — manage issues, review code, optimize pipelines inline
- Duo Chat (Agentic Chat): context-aware chat drawing on issues, MRs, pipelines, security findings, repo content; slash commands (`/explain`, `/test`, `/refactor`)
- Duo Code Review: automated MR review; **group-level custom review instructions** (19.0) via `.gitlab/duo/mr-review-instructions.yaml` — define once per group, combine with project-level overrides
- Model flexibility (19.1): Code Review Flow no longer locked to Anthropic Claude — can select GPT-5.2/GPT-5.3 Codex per group

### 5.2 Duo Agent Platform (DAP) — GA since January 2026
- **Agents vs Flows**: agents are single-purpose task automators; flows chain multiple agents across SDLC stages (e.g., issue → requirements analysis → code generation → tests → ready MR)
- **Foundational agents**: Planner, Data Analyst, Security Analyst, and others provided out of the box
- **AI Catalog**: central place to discover, activate, and govern agents/flows across projects and groups; publish approved configs org-wide
- **Custom flows** (beta): org-defined flows encoding your own standards, using `AGENTS.md` in-repo so agents follow project-specific conventions before committing
- **Developer Flow**: extends across the full MR lifecycle — resolve reviewer feedback, resolve conflicts, split oversized MRs, implement features at any stage
- **Software Development Flow**: plan → implement → test end-to-end with minimal human handoff
- **CI/CD migration & modernization flows**, issue-to-MR flows, code review flows — all built-in end-to-end flows
- **Knowledge Graph**: structured representation of your repo (files, functions, dependencies) so agents reason with real relationships, not just raw text
- **GitLab Orbit**: lifecycle context graph connecting code, pipelines, and deployments as grounded context — makes agents faster/cheaper/more accurate
- **Sessions**: every agent action is logged (`Project → AI → Sessions`) — critical for debugging and audit
- **Governance**: group-based access controls decide which agents/flows/models are allowed and where — maps to your existing role/group structure

### 5.3 MCP — Model Context Protocol
- **MCP Client** (GA): lets Duo Agentic Chat/IDE agents connect **out** to external systems — Jira, Slack, Confluence, Grafana — for context and actions
- **MCP Server** (beta): lets **external** AI tools (Claude Desktop, Cursor, other MCP clients) connect **into** your GitLab instance as a tool provider
- OAuth token lifetime for MCP clients is now configurable (19.1): 300–7200 seconds, useful for tightening security posture around agent credentials
- Build a small custom MCP integration connecting Duo to an external documentation source as a hands-on exercise

### 5.4 Self-hosted models & regulated/offline environments
- GitLab Duo Self-hosted: run your own LLM serving layer instead of GitLab-managed models — supports **vLLM, AWS Bedrock, Azure OpenAI**, and other platforms; GitLab 19.0 added support for additional self-hosted open-source models
- **AI Gateway**: the component that brokers requests between GitLab and your model backend — must be installed and configured for self-hosted setups
- GitLab Credits: the usage/billing model pooled across your org for Duo Agent Platform consumption; understand what does/doesn't consume credits (MCP clients themselves don't directly, but Agent Platform usage through them does)

<details>
<summary>🔒 <b>Air-Gapped Variant — click to expand (critical section: this is where most orgs get Duo wrong)</b></summary>

- **GitLab Duo Self-hosted with Offline Cloud License**: this is the officially supported path for air-gapped Duo — requires an Offline Cloud License activation (no phone-home licensing check)
- Full offline Duo stack: (1) stand up your own LLM serving infra locally (vLLM is the common choice for fully offline), (2) install the **AI Gateway** on your internal network with no external calls, (3) point GitLab's Duo configuration at your internal AI Gateway, (4) verify with GitLab's built-in Duo health-check command
- Model weights themselves must be transferred in via the same download-then-transfer pattern as everything else in an air-gapped environment — plan storage/GPU capacity for this ahead of time
- MCP Server/Client external integrations (Jira, Slack, etc.) generally won't work offline unless those tools are *also* hosted internally on the same isolated network — document this limitation for stakeholders

</details>

> 🏁 **Capstone:** Set up Duo Code Review with group-level instructions, build a custom AI Catalog flow that reads your `AGENTS.md`, wire up one MCP connection. Then — on your air-gapped VM — stand up a self-hosted vLLM instance + AI Gateway and get basic Duo Chat working with zero internet calls. Document the full architecture diagram for both.

> 🎓 **Certification checkpoint:** Maps to **GitLab Certified GitLab Duo Agent Platform Associate**.

---

## Phase 6 — GitLab Administration / Self-Managed `Weeks 23–27`

- Installation methods: Omnibus (Linux package), Docker, Helm chart (Kubernetes), Cloud-native hybrid
- Architecture components: Puma (web), Sidekiq (background jobs), Gitaly (Git storage), PostgreSQL, Redis, Workhorse, Registry, Pages
- `gitlab.rb` / `gitlab-ctl reconfigure` lifecycle
- **GitLab 19.0 Helm chart breaking changes** — know these cold as an architect:
  - **Gateway API + Envoy Gateway** replaces bundled NGINX Ingress as default networking (NGINX Ingress reached EOL March 2026, fully removed in GitLab 20.0)
  - **Bundled Bitnami PostgreSQL, Bitnami Redis, and MinIO charts are removed entirely** — these were only ever meant for PoC/test. Any Helm deployment must now use externally managed Postgres/Redis/object storage before upgrading to 19.0
  - Bundled Mattermost removed from the Linux package
- Backup & restore (`gitlab-backup`), disaster recovery drills — actually destroy and restore your instance
- Upgrade paths, version-hop rules, zero-downtime upgrade strategy
- SMTP, LDAP/SAML/SSO, SCIM provisioning
- Instance-wide settings via Admin Area / API, rate limiting
- Monitoring the instance: bundled Prometheus/Grafana, `gitlab-ctl tail`
- Object storage (S3-compatible) for artifacts, LFS, registry, backups

<details>
<summary>🔒 <b>Air-Gapped Variant — click to expand</b></summary>

- Full offline install procedure: download the `.deb`/`.rpm` package **and all dependency packages** on a connected machine of the same OS, transfer via physical media/local network, `dpkg -i`/`rpm -i` the dependencies first, then the GitLab package itself with `EXTERNAL_URL` set explicitly
- Set up your own **internal CA and TLS certificates** — no public CA reachable
- **Disable Version Check and Service Ping** (both phone home to GitLab and will just fail/log noise in an offline instance)
- **Disable runner version management** — it tries to fetch latest runner versions from GitLab.com
- Offline upgrades: download every required package for each version "stop" on a connected host, transfer the whole package directory, install offline stop-by-stop — never skip intermediate versions
- Build a **local package/container mirror** (e.g., a local Docker registry + tools like `verdaccio` for npm, or GitLab's own Package Registry as your internal mirror) so CI/CD jobs aren't trying to reach npmjs.org, PyPI, Maven Central, etc.
- Offline licensing: self-managed instances in fully disconnected networks need a **cloud licensing opt-out exemption** arranged with GitLab sales before purchase

</details>

> 🏁 **Capstone:** Fully rebuild your admin setup on the air-gapped VM: install from transferred packages, configure internal TLS, disable all phone-home checks, stand up a local package mirror, and run one full pipeline entirely offline using only local dependencies.

---

## Phase 7 — Scaling, High Availability & Geo `Weeks 28–33`

- Reference architectures by user count (1K/3K/10K/25K/50K+)
- Horizontal scaling: separate Gitaly, Sidekiq, Puma nodes; **Gitaly Cluster (Praefect)** for HA Git storage
- PostgreSQL HA (Patroni), Redis HA/Sentinel, load balancing (HAProxy/NGINX)
- **GitLab Geo**: multi-region replication for DR and global read performance — stand up primary + secondary
- **ClickHouse for advanced analytics**: runner fleet dashboards, contribution analytics, Duo/SDLC trend data, CI analytics via GraphQL — understand when GitLab uses ClickHouse as a secondary data store vs PostgreSQL as system of record; rolling-upgrade procedure for HA ClickHouse clusters
- Zero-downtime upgrades on HA clusters, capacity planning, performance tuning under load

<details>
<summary>🔒 <b>Air-Gapped Variant — click to expand</b></summary>

- HA components (Patroni, Sentinel, Praefect, HAProxy) are all self-hosted already — no special offline changes needed beyond ensuring their installer packages were transferred in Phase 6's mirror
- Geo secondary sites in a fully air-gapped multi-site setup must replicate over your own private network links (no public internet path) — plan bandwidth for Git and object storage replication accordingly
- ClickHouse Cloud is **not** an option offline — self-hosted ClickHouse only

</details>

> 🏁 **Capstone:** Stand up a minimal 2-node HA reference architecture with a Geo secondary; simulate primary failure and fail over; document RPO/RTO. Bonus: add self-hosted ClickHouse and pull one runner-fleet analytics dashboard.

---

## Phase 8 — Enterprise Architecture, Governance & Integrations `Weeks 34–38`

- Multi-tenant namespace/group architecture at enterprise scale
- GitLab Dedicated vs Self-Managed vs SaaS — trade-off matrix for leadership (cost, compliance, control)
- **Agent for Kubernetes (GitOps)**: pull-based deployments, fleet management across clusters
- Terraform integration: GitLab-managed state, plan/apply in CI
- **Value Stream Analytics, DORA metrics, Insights** — proving DevOps ROI
- **Advanced Search** (Elasticsearch/Zoekt-backed) at scale for large instances
- API & GraphQL mastery: automate group creation, bulk permission changes, internal tooling
- Cost governance: compute minutes, storage quotas, artifact expiration policies, **GitLab Credits pool management** for AI usage at org scale

<details>
<summary>🔒 <b>Air-Gapped Variant — click to expand</b></summary>

- Advanced Search backends (Elasticsearch) must be self-hosted and index-updated locally — no external search service calls
- Terraform providers/modules referenced in pipelines need to be vendored into your internal Package Registry mirror rather than pulled from the public Terraform Registry
- GitLab Agent for Kubernetes works fine air-gapped as long as the agent image itself was transferred into your local registry and clusters are on the same private network

</details>

> 🏁 **Capstone:** Design (diagram + written proposal) a full enterprise architecture for a fictional 5,000-engineer company: topology, HA/Geo plan, security + compliance framework enforcement, AI/Duo governance model, cost governance, and — critically — a section explaining which parts of the design change if this company is a defense contractor requiring full air-gap. This dual-mode thinking is what separates an architect from an admin.

---

## Phase 9 — GitLab 19 Deep-Dive Review `Week 39`

Consolidate everything version-specific you've absorbed across phases into one reference pass — this is what interviewers and real migration projects will probe.

**Breaking changes:**
- NGINX Ingress → Gateway API/Envoy default
- Bitnami Postgres/Redis/MinIO removal from Helm chart
- Mattermost removal
- Bitbucket Cloud import auth change (API tokens replace app passwords)

**New in 19.0:**
- Secrets Manager (beta)
- Developer Flow extended across MR lifecycle
- Self-hosted open-source model support
- Dependency Scanning Default profile
- Mermaid 11
- Rapid Diffs (beta)

**New in 19.1:**
- Group-level Duo review instructions
- Secret false-positive AI detection (GA)
- Full-commit-history secret scanning
- Compliance framework templates (19 total)
- Configurable OAuth token lifetime
- Scheduled pipeline execution policies (beta)
- Wiki emoji reactions
- Code Review Flow model choice (GPT-5.2/5.3 Codex)

> 💡 Understand GitLab's **breaking-change governance process**: leadership sign-off now required before any breaking change ships (17.0 had 80 breaking changes, 18.0 had 27, 19.0 projected 15 — trend of increasing upgrade stability).

> 🏁 **Deliverable:** Write your own one-page "what changed and why it matters" migration brief, as if presenting to a team about to upgrade a large self-managed fleet to 19.x.

---

## Phase 10 — Certification Path (Complete All Available)

Take these as you complete matching phases — this is the full current catalog, take every one:

| Certification | After Phase | Validates |
|---|---|---|
| 🎓 Certified Git Associate | 1 | Foundational Git/GitLab skills |
| 🎓 Certified Fundamentals Associate | 2 | Core workflows, basic CI/CD, security setup |
| 🎓 Certified CI/CD Associate | 3 | Pipeline design, CI/CD functions |
| 🎓 Certified Security Associate | 4 | SAST/DAST/secret detection, security policies |
| 🎓 Certified GitLab Duo Agent Platform Associate | 5 | Agentic chat, agent/flow config, MCP connections, AI-assisted workflows |
| 🎓 Certified Agile Portfolio Management Associate | 2 & 8 | Org structure, boards, epics, roadmaps, velocity |
| 🎓 Certified Project Management Associate | 2 | Issues, labels, milestones, groups, boards/epics/roadmaps for PM roles |

**Notes on the exam format** *(current as of mid-2026, always re-verify at [university.gitlab.com/pages/certifications](https://university.gitlab.com/pages/certifications) before scheduling)*:
- Exams were refreshed in 2026 with a new test-taking experience
- Each technical certification = written knowledge exam (80%+ to pass) **plus** a graded hands-on exam completed in a GitLab Demo Cloud sandbox, reviewed by GitLab Professional Services Engineers
- Written exams: multiple choice + multiple response, generally ~15 questions, unlimited retakes on the written portion
- Hands-on results delivered within ~7 business days
- Recommend starting with **Git Associate** even if you're experienced — it's the assumed prerequisite knowledge for everything else

> ℹ️ If GitLab's Professional Services engineer-level certifications (delivery/consulting-track, distinct from the University associate track) are relevant to your career goal (e.g., you want to work for a GitLab partner or in Professional Services), also check the GitLab Handbook's Professional Services Engineering certification page — that track is separate from the University associate exams and has its own enrollment path through GitLab directly rather than public self-service purchase.

---

## Phase 11 — Capstone: Full Air-Gapped Rebuild `Weeks 40–44`

The final proof of "irreplaceable": rebuild the **entire** stack — admin, CI/CD, security, and AI — on your disconnected VM with zero internet access at runtime, using only your bastion host for staged transfers.

**Checklist** *(do all of it, in this order)*:

- [ ] Provision the disconnected VM fresh
- [ ] Download GitLab package + all OS dependency packages on the bastion, transfer, install offline
- [ ] Configure internal CA + TLS, disable Version Check/Service Ping/runner version management
- [ ] Set up a local Package Registry mirror + local container registry for CI/CD dependencies
- [ ] Register a runner and get a full CI/CD pipeline (build → test → deploy) running with zero external network calls
- [ ] Load the Secure-Binaries security scanner images (downloaded via the vendored template on the bastion) and get SAST + Secret Detection running offline
- [ ] Stand up self-hosted vLLM + AI Gateway, activate Duo via Offline Cloud License, get Duo Chat answering questions about your offline repo
- [ ] Perform a full backup, wipe the instance, restore from backup — offline, end to end
- [ ] Write the architecture diagram and a runbook someone else could follow to reproduce this without you

> ⭐ **This is the deliverable that proves architect-level, not certificate-level, competence** — most certified users have never actually done this.

---

## 🔁 Ongoing (parallel track, all phases)

- Read the public GitLab Handbook — shows how GitLab runs GitLab itself, unusually good architect material
- Follow monthly release notes ([docs.gitlab.com/releases](https://docs.gitlab.com/releases)) — GitLab ships monthly, staying current is part of the job, and as you've seen, breaking changes and AI features move fast
- Contribute to GitLab's own open-source repo — small MRs teach the platform from the inside
- Join the GitLab community forum/Discord for production war-stories, especially around Duo Agent Platform adoption and air-gapped operations, both of which are still maturing fast

---

## ✅ What "irreplaceable" looks like at the end

- [ ] Design any org's group/permission structure from scratch and defend the decisions
- [ ] Write and debug any pipeline construct without searching docs
- [ ] Stand up, upgrade, back up, and disaster-recover a production instance solo — **in both connected and fully air-gapped modes**
- [ ] Architect an HA/Geo deployment and explain trade-offs to non-technical leadership
- [ ] Deploy and govern GitLab Duo Agent Platform responsibly, including a fully self-hosted/offline AI stack for regulated environments
- [ ] Hold **every currently available GitLab University certification**, not just one or two
- [ ] Have a portfolio (your Phase 11 runbook + Phase 8 architecture proposal) that proves all of the above with evidence, not just credentials

