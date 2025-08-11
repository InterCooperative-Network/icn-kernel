# ICN Kernel v1 — Project Charter (v0.1)

**Status:** Draft 0.1 (sync with new repo)
**Owners:** Matt (Lead), Core Maintainers (TBD)
**Audience:** Protocol, runtime, security, and governance contributors

---

## Repository

**Name:** `icn-kernel`

**Short Description:** Spec‑first minimal kernel for the InterCooperative Network (ICN): identity/attest, jobs (CCL/WASM), append‑only event log & DAG, **Contribution Credits (CC)** issuance, and governance/policy evaluation; includes a **v1‑minimal tokens** module for inter‑organization settlement (invoices/transfers/clearing).

**Repo Companions (planned):** `icn-spec` (versioned specs), `icn-conformance` (golden tests & fixtures), `icn-ccl` (language/tooling), `icn-examples` (reference configs/programs).

---

## 0) Terminology

* **Contribution Credits (CC):** Formal name for the protocol’s non‑transferable, rate‑based access credits. Some UIs/docs may refer to these informally as **“mana.”**
* **Organizations:** Umbrella term for co‑ops and communities (same architecture, different function).
* **Federations:** Coordination overlays spanning multiple organizations.

---

## 1) Mission & Vision

**Mission:** Build a minimal, secure, spec‑first kernel for the InterCooperative Network (ICN) that enables global participatory economics and governance across **organizations** and their **federations**.

**Vision:** An alternative cooperative economy where organizations interoperate outside the capitalist model *among themselves*: resources are priced by verified contribution, access is governed by signed policy (not plutocracy), and coordination scales via federations without requiring global consensus.

---

## 2) Outcomes (v1 exit criteria)

By the end of v1, we ship:

* **Kernel surfaces (5):** Identity/Attest, Jobs (WASM/CCL), Event Log + DAG, Issuance (**Contribution Credits**), Governance/Policy evaluation.
* **Tokens module (v1‑minimal):** Local ledgers for orgs/federations; inter‑organization settlement primitives (invoice/transfer/clearing); **no markets/DEX**.
* **Spec Pack v1:** Event type schemas, Policy Object (+ obligations), CCL ABI/Program Manifest, OpenAPI v1 for kernel endpoints.
* **Conformance Suite v1:** Golden tests for job→receipt, issuance, governance transitions, policy decisions, and inter‑org settlement.
* **Reference Deployment:** One federation with ≥2 organizations, ≥3 executors, demo workloads, dashboards, and incident playbooks.
* **Security Baseline:** Reproducible builds, signed releases, sandboxed WASM, policy‑enforced egress, incident response runbooks.

---

## 3) Non‑Goals (v1)

* No global consensus or multi‑federation finality (asynchronous checkpoint exchange only).
* **No token markets/DEX, staking, or speculative yield.** Tokens exist for treasury/accounting and inter‑org settlement only.
* No heterogeneous runtimes beyond WASM/CCL.
* ZK proofs used narrowly/optionally; not required for every flow.
* Governance cannot rewrite history or execute arbitrary runtime code.

---

## 4) Invariants & Design Constraints (lock)

1. **Event‑sourced core:** If it isn’t in the append‑only log, it didn’t happen. All state is a projection.
2. **Deterministic edge:** CCL→WASM programs are pure state transitions: inputs + ProgramID → outputs + receipt.
3. **Adversarial edges; cooperative policy:** Trust is computed and scoped; governance raises trust but never bypasses safety.
4. **Local‑first, reconcile‑later:** Nodes make progress offline; reconciliation uses proofs, not authority.
5. **Version everything:** All artifacts/events carry schemas with N/N−1 compatibility; upgrades are governed.

---

## 5) System Boundaries & Actors

* **Identities:** keypairs with attestations; may represent people, orgs, services, nodes.
* **Organizations:** operational units with their own policies and treasuries.
* **Federations:** coordination overlays that sign checkpoints/policies; cannot dictate local history.
* **Nodes:** run kernel services; **Executors** run jobs; **Witnesses/Auditors** measure/verify; **Maintainers** sign releases.

---

## 6) Architecture Summary (v1 components)

* **Identity & Attestation:** DID‑style identities, revocation lists, attestations (hardware claims, roles, uptime). Used by all other components.
* **Governance & Policy:** Signed **Policy Objects** (who/what/where/rates/obligations). Evaluator returns allow/deny + reasons + obligations. Governance issues trust weights, delegations, SLO charters, version gates.
* **Jobs & Scheduler:** Submit → eligibility filter (policy, capability, data locality) → risk‑adjusted bid (trust‑aware) → execute in WASM sandbox → emit receipt.
* **Execution (CCL/WASM):** Deterministic programs with explicit capabilities (storage, time, network, secrets). Metering produces a resource‑usage vector embedded in the receipt.
* **Event Log & DAG:** Single canonical append‑only log of typed events (Receipts, Policy, Identity, Checkpoints, Issuance, Treasury). Hash‑linked DAG with periodic federated checkpoints.
* **Economy (dual):**

  * **Contribution Credits (CC):** Non‑transferable, expiring access credits earned from measured contribution and verified execution; required for spam resistance and baseline access.
  * **Tokens (v1‑minimal):** Transferable within/among organizations and federations for budgeting and **inter‑org settlement** (invoice/transfer/clearing). Tokens cannot buy protocol rights; they may purchase *priority* within policy bounds.
* **Networking:** Authenticated pubsub; membership/policy channels per org/federation; checkpoint gossip; no global locks.
* **Observability:** Metrics, traces, decision rationales, incident timelines; evidence store references for every critical action.
* **Security:** Minimal TCB, WASM sandbox/jail, egress policy, signed artifacts, kill‑switch policies (freeze issuance, quarantine identities).

---

## 7) Economics Model (v1)

* **Contribution Credits (CC):** Non‑transferable, rate‑based credits that function as access quota and spam resistance.
* **Regen rate (earned):** `regen_rate = g(contribution_vector, scarcity_index, trust_weight)`; contribution aggregates CPU, memory, storage, bandwidth, uptime, and successful job receipts; `scarcity_index` is an EMA of available resources; `trust_weight` is governance‑assigned.
* **Bucket mechanics:** Credits accumulate continuously at `regen_rate` up to `cap = α × daily_regen` (default α≈5–7). Spending deducts balance; unused credits **soft‑decay** over time.
* **Adaptation/decay:** Regen rate decays with inactivity (EMA half‑life ≈ 14 days by default). A small universal **floor** ensures baseline governance participation.
* **Fees/Burn:** A minimum CC burn per job (anti‑spam); dynamic burn increases under scarcity.
* **Witnessing:** Issuance uses **minimum‑consistent** multi‑witness measurement (self + random peers + optional TEE). Discrepancies trigger audits.
* **Slashing:** Fraud evidence yields **negative issuance** and **trust‑weight** reductions; probation windows apply.

### Inter‑Organization Settlement (Tokens v1‑minimal)

* **Purpose:** Allow organizations to operate a cooperative economy among themselves—budgeting, purchasing, and reciprocal services—*without* relying on capitalist infrastructure.
* **Ledger:** Per‑org and per‑federation token ledgers with typed events: `Mint`, `Burn`, `Transfer`, `Invoice`, `Settle`, `Clear`.
* **Flows:**

  * **Invoice → Transfer:** Org A invoices Org B; upon approval, a policy‑checked transfer occurs (or accrues to a clearing account).
  * **Periodic Clearing:** Federation clears bilateral obligations on a cadence (e.g., weekly) using net positions; emits `Clear` events.
  * **Budgeting:** Governance‑approved `Mint` to organizational treasuries; spending gated by policy/SLO charters.
* **Constraints:**

  * No markets/DEX; no perpetual claims on future governance rights.
  * **Priority auctions** allowed only within policy caps; CC burn remains mandatory (tokens cannot bypass spam resistance or rights).
  * Optional **demurrage** (slow decay) may be configured per federation to discourage hoarding.

---

## 8) Governance Model (v1)

* **Scope:** Organizations and federations. (Organizations differ by function, not architecture.)
* **Objects:** Policy Objects, Trust Weights/Delegations, Budget/Treasury approvals, Version Gates.
* **Mechanics:** Membership‑weighted voting; quorum/thresholds; **sortition** for reviewer pools (audits/checkpoints). Effective‑from timestamps; immutable history.
* **Limits:** Governance cannot execute arbitrary code, cannot rewrite history, cannot bypass safety invariants.

---

## 9) Privacy & ZK Posture (v1)

* **Tier 1:** Private inputs, public receipts (hashes + capability trace summaries).
* **Tier 2:** Selective disclosure of evidence (policy‑required obligations).
* **Tier 3 (optional):** ZK claims for targeted properties (range proofs on usage; predicate satisfaction; "no forbidden API called").

---

## 10) Security Assumptions & Threats

* **Threats:** Sybil farms, fake metrics, bribed witnesses, key theft, supply‑chain attacks, data exfiltration.
* **Controls:** Short‑lived tokens and key rotation; multi‑witness probes; provenance in receipts (ProgramID, compiler version, source commit); path‑scoped PR rules; incident kill‑switches; immutable audit log.

---

## 11) Deliverables & Artifacts

* **Spec Pack v1**

  * Event Types: `Identity*`, `Policy*`, `Job*`, `ComputeReceipt`, `Issuance*`, `Treasury*`, `Checkpoint*`.
  * Policy Object schema (with **obligations**).
  * CCL ABI & Program Manifest (determinism contract, capability list, metering semantics).
  * OpenAPI v1 for kernel endpoints.
* **Code**

  * `icn-kernel` runtime (the five surfaces above) + tokens v1‑minimal.
  * `icn-conformance` tests (golden fixtures; replayable logs).
  * `icn-examples` (reference programs, org/federation configs, settlement demos).
* **Ops**

  * Playbooks: key rotation, compromise response, issuance freeze, ledger replay, repo rollback.
  * Dashboards: Federation/Economic/Compute/Treasury health.

---

## 12) Milestones & Exit Criteria

* **M0 — Repo & Spec Cut:** Repos created; Spec Pack skeleton; CODEOWNERS and signing; CI lint/test.
* **M1 — Minimal Kernel:** Identity/Attest; Jobs→WASM→Receipt; Event Log; OpenAPI v1 skeleton. *Exit:* golden job→receipt passes.
* **M2 — Scheduler & CC Issuance:** Eligibility filter; risk‑adjusted bidding; issuance daemon; negative issuance. *Exit:* issuance tests pass; scarcity dynamics verified.
* **M3 — Governance/Policy:** Policy Objects; evaluator; trust weights; obligations enforced. *Exit:* policy decisions produce verifiable rationales.
* **M4 — Inter‑Org Settlement (Tokens v1‑minimal):** Treasury events; invoice→transfer→clear; budgeting. *Exit:* federation demo clears weekly without deficits.
* **M5 — Conformance & Observability:** Golden suites; metrics/traces; incident timelines. *Exit:* dashboards green on reference deployment.
* **M6 — Privacy Baseline & Hardening:** Private inputs/public receipts; capability traces; selective disclosure; threat drills; reproducible builds; signed release v1.0.0.

---

## 13) Success Metrics (initial)

* **Reliability:** 99% policy eval < 50 ms; issuance finality < 5 s; job P50 completion predictability within ±15%.
* **Security:** 0 critical sandbox escapes; SBOM + provenance for every release; signed checkpoints only.
* **Auditability:** 100% critical actions with human‑readable rationale; full replay from genesis < 30 minutes for reference deployment.
* **Economy:** Inter‑org settlement clears on schedule ≥ 99% of periods without manual intervention; CC burn/regen parameters maintain < 5% spam incidents.
* **Quality:** ≥85% conformance coverage; deterministic re‑execution rate ≈ 100% for receipts.

---

## 14) Repository Plan & CI/CD

* **Repos:** `icn-spec`, `icn-kernel`, `icn-ccl`, `icn-conformance`, `icn-examples`.
* **Branching:** trunk‑based; protected main; release branches; mandatory signed commits for maintainers.
* **CI:** build, test, conformance replay, SBOM, provenance attestation, container scan.
* **Releases:** semver; signed artifacts; changelogs; upgrade notes (N/N−1 support).

---

## 15) Risks & Mitigations (initial)

* **Scope creep:** Strict v1 non‑goals; RFC gate for new surfaces.
* **Verification cost:** Multi‑witness minimalism first; ZK optional; TEEs as additive trust weight, not requirement.
* **Governance capture / token drift:** Non‑transferable rights; sortition; transparency; baseline universal service caps via policy; optional demurrage; priority caps.
* **Supply chain:** Reproducible builds; ProgramID pinning; ban‑list policy for compromised hashes.

---

## 16) Interfaces with MCP (meta‑tooling)

* MCP produces: Spec edits (PRs), kernel tasks, conformance fixtures, reference programs. MCP has **no runtime role** in ICN.

---

## 17) Open Questions (to resolve in RFCs)

* Default scarcity/regen/cap/decay parameters and governance bounds for CC.
* Federation checkpoint quorum math and reviewer selection randomness.
* Capability taxonomy (minimum set for v1) and egress policy grammar.
* Token budget controls (caps, demurrage) and priority auction fairness.
* Privacy tiers: which claims warrant ZK in v1.1.

---

## 18) Document Control

* **Change process:** PR with change log; review by Owners + Security + Governance maintainers.
* **Approvals required:** Any change to invariants, event schemas, or policy evaluator requires 2‑of‑N multi‑sig from designated maintainers.
* **Next review:** On completion of M0.

## See Also

- [Value Theory & Distribution Model](philosophy/value-theory.md) — How unequal contributions coexist with equal membership
- [Economic Model Specification](economy/economic-model.md) — Detailed 3×3 economic layers and scales
- [Governance Model Specification](governance/governance-model.md) — Democratic decision-making at all levels
