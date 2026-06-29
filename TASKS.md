# TASKS — rare-cancer-registry-templates

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Itemized backlog for the open, **consent-first** rare-cancer registry **standards** kit. See
[`PLAN.md`](./PLAN.md) for context, the binding cancer guardrails, the licensing gate, and the
roadmap (M0–M4). **This project ships standards, never a data store** — no task may collect, store,
host, process, or redistribute patient data (identifiable, de-identified, or row-level aggregate).
Every example/fixture is **fully synthetic**.

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable `rare-cancer-registry-templates-<area>-NNN` (the table ID).
- **title** — the table Title.
- **project** — `rare-cancer-registry-templates`.
- **type** — `code | research | writing | data | design-spec | maintenance` (table "Type"). Note:
  `data` tasks here produce **schemas/crosswalks/synthetic fixtures**, never patient data.
- **lane** — `donated` for all tasks here. Any future metered run would be `funded` and must add
  `fundedBudgetUsd`.
- **priority** — `high | medium | low`.
- **domain** — e.g. `["cancer-research","open-standards","health-data","interoperability","governance"]`.
- **riskTier** — `low | medium | high`. **HIGH** = patient/family-facing content (consent/explainers,
  needs oncologist+advocate sign-off) **or** the regulatory/legal mapping (needs bioethics/legal
  sign-off). Standards engineering/CI = low–medium; consent *architecture* = medium (bioethics touch).
- **urgent** — boolean (default `false`).
- **deliverable** — `pr | dataset | document | translation`. (`dataset` = schema/crosswalk/synthetic
  artifact set; **not** patient data.)
- **tokenEstimate** — `small | medium | large` (table "Size").
- **status** — `open | in-progress | review | delivered | done` (start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — `jdev1977` / rare-cancer beneficiary class until a named partner is secured.
- **verifiedNeed** — **`false`** while no committed adopting community/steward and no reviewers are
  secured (honest; the *gap* is real, the last-mile beneficiary is TO BE SECURED).
- **outputLicense** — `MIT` (or `Apache-2.0` — open) for code; `CC-BY-4.0` for schemas/crosswalks/
  templates/docs; `CC-BY-SA-4.0` where a share-alike upstream is incorporated.

> **Standing guardrails on every task (binding):**
> 1. **No real patient data, ever** — synthetic fixtures + published-aggregate context only; the CI
>    "no real data" scan + synthetic-data attestation must pass.
> 2. **No source touched until its `sources/allowlist.yml` entry is `approved`** by the licence
>    reviewer. Any task proposing to embed/redistribute **SNOMED CT, ICD-O-3, LOINC, or ICCR**
>    proprietary content is **refused and flagged** — crosswalk *stubs* (codes/IDs only) are the
>    sole permitted use.
> 3. **No HIGH-tier (patient-facing or legal) artifact ships without credentialed sign-off.**
> 4. **Provenance on every administered item; "not medical advice" / "not legal advice" notices on
>    the relevant surfaces.**

---

## Milestone M0 — Governance & standards spine (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-gov-001 | Standards-not-a-data-store + synthetic-data-only constitution | design-spec | small | medium | document | — | Maintainer + licence reviewer |
| rare-cancer-registry-templates-license-001 | Source allow-list schema + licence-gate policy (open-lead, restricted-stub rule) | design-spec | small | medium | document | — | Licence reviewer |
| rare-cancer-registry-templates-license-002 | Analyse & record ≥5 candidate standards; approve the open core | research | medium | medium | document | rare-cancer-registry-templates-license-001 | Licence reviewer |
| rare-cancer-registry-templates-prov-001 | Provenance model + countable assertion (administered-item) unit | design-spec | small | low | document | rare-cancer-registry-templates-gov-001 | Maintainer |
| rare-cancer-registry-templates-ci-001 | CI scaffold: provenance linter + licence deny-list + "no real data" scan | code | medium | low | pr | rare-cancer-registry-templates-prov-001, rare-cancer-registry-templates-license-001 | Maintainer |
| rare-cancer-registry-templates-partner-001 | Reviewer + adopting-community outreach (oncologist, advocate, bioethics/legal, licence) | research | small | low | document | — | Maintainer |

**Acceptance criteria (key M0 tasks)**

- **rare-cancer-registry-templates-gov-001**
  - States, as a binding project constitution, that the project **ships standards and never collects,
    stores, hosts, processes, or redistributes patient data** (identifiable, de-identified, or
    row-level aggregate).
  - Mandates **fully synthetic** fixtures + **published-aggregate** context only; defines the
    per-fixture **synthetic-data attestation** format.
  - Defines the **responsibility boundary**: adopters own IRB/REC approval, consent, and custody;
    Elyos holds nothing.
  - Requires the constitution text atop every published template and the docs site.
- **rare-cancer-registry-templates-license-001**
  - `sources/allowlist.yml` schema defines: name, custodian, URL, version, licence/legal basis,
    rights analysis, `restricted: true|false`, and `status: approved|rejected|pending`.
  - Policy states the **open-lead rule** (Mondo/ORDO/HPO/OncoTree/NCIt/mCODE/OMOP/Phenopackets/DUO
    primary) and the **restricted-stub rule** (SNOMED CT/ICD-O-3/LOINC/ICCR = code-only crosswalk
    stubs; embedding proprietary content is categorically `rejected`).
  - States the "no artifact more permissive than its most-restrictive embedded source" licence rule.
- **rare-cancer-registry-templates-license-002**
  - ≥5 standards analysed with recorded licence determination; the **open core approved**; SNOMED CT,
    ICD-O-3, LOINC, ICCR recorded as `restricted` (stub-only); ISO 11179 recorded as discipline-only.
  - OMOP **vocabulary** per-vocab licence caveat captured (some require SNOMED/CPT licences → open
    vocabs only).
- **rare-cancer-registry-templates-ci-001**
  - CI fails on any administered item lacking a resolvable provenance link.
  - CI **licence deny-list** scan fails on embedded restricted terminology
    (descriptions/hierarchy of SNOMED/ICD-O-3/LOINC or ICCR derivative content).
  - CI **"no real data"** scan + synthetic-attestation check fail the build on any fixture lacking an
    attestation. `pnpm build && pnpm test && pnpm lint` green.

**M0 Definition of Done:** constitution (no-data + synthetic-only) and provenance model (with the
countable administered-item unit) merged; allow-list schema + licence-gate policy merged with ≥5
standards analysed and the open core approved; CI gates (provenance, licence deny-list, "no real
data") live and green; reviewer + community outreach initiated with status logged; **a qualified
licence reviewer named (hard exit — if the seat is empty, M0 cannot exit; escalate per PLAN.md).**

---

## Milestone M1 — Core CDE set + interoperability crosswalks

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-cde-001 | Minimal-core CDE set in LinkML (de-identified-by-design) | design-spec | large | medium | document | rare-cancer-registry-templates-ci-001, rare-cancer-registry-templates-license-002 | Standards reviewer |
| rare-cancer-registry-templates-vocab-001 | Open-vocabulary bindings + restricted-terminology crosswalk stubs | data | medium | medium | dataset | rare-cancer-registry-templates-cde-001 | Standards + licence reviewers |
| rare-cancer-registry-templates-crosswalk-001 | Crosswalks CDE ⇄ mCODE/FHIR, OMOP CDM, GA4GH Phenopackets | data | large | medium | dataset | rare-cancer-registry-templates-vocab-001 | Standards reviewer |
| rare-cancer-registry-templates-fixtures-001 | Synthetic conformance fixtures + round-trip test | code | medium | low | pr | rare-cancer-registry-templates-crosswalk-001 | Maintainer |
| rare-cancer-registry-templates-validate-001 | Validator CLI (CDE schema + crosswalk round-trip) | code | medium | low | pr | rare-cancer-registry-templates-fixtures-001 | Maintainer |

**Acceptance criteria (key M1 tasks)**

- **rare-cancer-registry-templates-cde-001**
  - Minimal core holds **only de-identified-by-design** fields (e.g., year not full DOB, generalised
    geography, no free-text identifiers); each field carries a `deIdentDesign` note.
  - Re-identification-risky elements are placed in **optional, clearly-flagged modules**, not the core.
  - Every CDE carries definition, datatype, value-set binding, `patientFacing` flag, and **100%
    provenance** (source standard + identifier + retrieval date + licence note) per the M0 model.
  - Includes a **consent-metadata** module (consent status, scope/data-use, revocation state).
- **rare-cancer-registry-templates-vocab-001**
  - Disease/phenotype/oncology bindings use **open** systems (Mondo/ORDO/HPO/OncoTree/NCIt) as primary.
  - Bindings to SNOMED CT/ICD-O-3/LOINC are **crosswalk stubs**: target codes referenced by identifier
    only, **no** proprietary descriptions/hierarchy embedded; licence deny-list linter green.
- **rare-cancer-registry-templates-crosswalk-001**
  - A **synthetic** record expressed in the CDE set **round-trips to mCODE FHIR, OMOP CDM, and
    Phenopackets** and validates against each.
  - Every crosswalk row carries provenance; lossy mappings are documented, not silently dropped.
- **rare-cancer-registry-templates-validate-001**
  - `validate` checks a synthetic dataset against the CDE schema and runs the crosswalk round-trip;
    fails on schema violations or round-trip mismatches.
  - Refuses to run on any input lacking a synthetic-data attestation (no-real-data guard).

**M1 Definition of Done:** minimal-core CDE set + consent module authored with 100% provenance and
open bindings; restricted bindings present only as stubs (licence linter green); crosswalks to
mCODE/OMOP/Phenopackets proven by a synthetic round-trip; validator CLI green in CI; **licence
reviewer signed off all bindings.** `pnpm build && pnpm test && pnpm lint` green.

---

## Milestone M2 — Consent & governance toolkit + regulatory mapping (HIGH-tier, reviewer-gated)

> **Entry gate (hard):** HIGH-tier reviewers secured — **clinical oncologist + patient advocate**
> (patient-facing) and **bioethics/IRB or health-data-privacy** (legal mapping). If not secured by the
> M2 entry date, **M2 does not start; the project holds at M1.**

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-consent-001 | Consent templates (broad/tiered/dynamic) + FHIR Consent model w/ revocation + DUO | design-spec | large | high | document | rare-cancer-registry-templates-cde-001 | Bioethics + advocate + oncologist |
| rare-cancer-registry-templates-consent-002 | Deceased-patient / memorial-registry consent pattern + ethics notes | writing | medium | high | document | rare-cancer-registry-templates-consent-001 | Bioethics + advocate |
| rare-cancer-registry-templates-governance-001 | Governance-charter + data-sharing-agreement + FAIR/CARE checklist templates | design-spec | medium | medium | document | rare-cancer-registry-templates-consent-001 | Bioethics + maintainer |
| rare-cancer-registry-templates-compliance-001 | Informational GDPR/HIPAA/Common-Rule mapping (primary-source-cited) | research | large | high | document | rare-cancer-registry-templates-consent-001 | Bioethics/legal reviewer |
| rare-cancer-registry-templates-govcheck-001 | Governance-conformance checklist in the validator | code | medium | medium | pr | rare-cancer-registry-templates-governance-001, rare-cancer-registry-templates-validate-001 | Maintainer |

**Acceptance criteria (key M2 tasks)**

- **rare-cancer-registry-templates-consent-001**
  - Templates specify, for each consent model, a working **revocation + downstream-propagation** path,
    an explicit **data-use specification** (GA4GH **DUO** codes), lawful-basis placeholders, and a
    return-of-results policy.
  - Provided as both human-readable text **and** a structured **FHIR Consent** resource.
  - Carries **"education only — not medical advice"** notice; **oncologist + patient-advocate sign-off
    recorded** before publish; consent language plain-language reviewed.
- **rare-cancer-registry-templates-consent-002**
  - Pattern treats deceased data **with** obligations, not as a consent-free shortcut: includes the
    **HIPAA 50-year decedent rule** (§164.502(f)), **GDPR Recital 27 + member-state** caveats,
    next-of-kin/authorised-representative consent path, and surviving-relative/genetic-implication
    notes; **bioethics + advocate sign-off recorded.**
- **rare-cancer-registry-templates-compliance-001**
  - Every GDPR (Art. 9 special-category basis, Art. 89 safeguards), HIPAA (de-identification Safe
    Harbor/Expert Determination; decedent rule), and Common Rule (§46.116(d) broad consent) claim is
    **primary-source-cited**.
  - Every page labelled **"informational — not legal advice; consult your IRB/REC, DPO, and counsel."**
  - **Bioethics/legal reviewer sign-off recorded**; `lastVerified`/`validUntil` set per claim.
- **rare-cancer-registry-templates-govcheck-001**
  - The validator's governance checklist fails an adopter's consent artifact that lacks a revocation
    path, a data-use specification, a lawful-basis placeholder, or provenance — making
    **non-consensual use fail by default**.

**M2 Definition of Done:** consent templates (with revocation + DUO data-use), governance/DSA/FAIR-CARE
templates, deceased-patient pattern, and the informational regulatory mapping all authored,
primary-source-cited, and **HIGH-tier signed off** (oncologist+advocate for patient-facing;
bioethics/legal for the mapping); "not medical/legal advice" notices in place; governance-conformance
checklist live in the validator. `pnpm build && pnpm test && pnpm lint` green.

---

## Milestone M3 — Conformance, starter-kit & pilot adoption (shipped)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-ig-001 | FHIR ImplementationGuide + LinkML-generated docs starter-kit | code | large | medium | pr | rare-cancer-registry-templates-crosswalk-001, rare-cancer-registry-templates-consent-001 | Standards reviewer + maintainer |
| rare-cancer-registry-templates-testbank-001 | Synthetic conformance test-bank (CDE + crosswalk + governance) | data | medium | low | dataset | rare-cancer-registry-templates-govcheck-001, rare-cancer-registry-templates-ig-001 | Maintainer |
| rare-cancer-registry-templates-adopt-001 | Secure adopting community; stand up/harmonise a consent-first registry against the standard | research | large | medium | document | rare-cancer-registry-templates-testbank-001, rare-cancer-registry-templates-partner-001 | Steward + maintainer |
| rare-cancer-registry-templates-sustain-001 | Sustainability: version policy, staleness cadence, upstream-contribution + reviewer rotation plan | writing | small | low | document | rare-cancer-registry-templates-adopt-001 | Maintainer |

**Acceptance criteria (key M3 tasks)**

- **rare-cancer-registry-templates-ig-001**
  - Published FHIR IG + docs site lead with the **standards-not-a-data-store** and **not-medical-advice**
    notices; render the CDE set, crosswalks, and consent/governance templates with provenance.
  - Adopter onboarding guide explains running the registry **consent-first** under the adopter's own
    IRB/REC and custody.
- **rare-cancer-registry-templates-adopt-001**
  - A **named** rare-cancer community/registry adopts the CDE set **and** the consent/governance
    toolkit to stand up or harmonise a **consent-first** registry (their data, their custody).
  - A **conformance report** (CDE + crosswalk round-trip + governance checklist) is produced on the
    adopter's **synthetic** test data; ≥1 documented adoption/citation recorded.
  - Confirms **no patient data** ever flowed to this project.

**M3 Definition of Done (project "shipped"):** FHIR IG + starter-kit + synthetic test-bank published
and green; a secured rare-cancer community has adopted the standards for a consent-first registry with
artifacts independently reviewed; outcomes recorded; sustainability/version/staleness plan in effect;
**no patient data held by this project.**

---

## Milestone M4 — Sustain & standards stewardship (post-delivery)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-extend-001 | Per-disease extension pattern, validated on one exemplar subtype | design-spec | medium | medium | document | rare-cancer-registry-templates-adopt-001 | Standards + oncologist reviewer |
| rare-cancer-registry-templates-upstream-001 | Upstream-contribution path (OHDSI/GA4GH/ERN) + maintenance rotation | maintenance | small | low | document | rare-cancer-registry-templates-sustain-001 | Maintainer |

**M4 Definition of Done:** extension pattern documented and validated on one exemplar rare cancer
(synthetic only); upstream-contribution conversation opened with a standards body; maintenance +
reviewer rotation, version policy, and staleness cadence operating; outcomes dashboard tracks
adoptions (not downloads).

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| rare-cancer-registry-templates-i18n-001 | Multilingual consent/explainer templates (plain-language) | writing | medium | high | translation | HIGH; per-language oncologist+advocate review; not medical advice |
| rare-cancer-registry-templates-dynconsent-001 | Reference (non-data-holding) dynamic-consent state schema | design-spec | medium | medium | document | Scope-creep risk; schema only, never a service holding data |
| rare-cancer-registry-templates-care-001 | CARE-principles / Indigenous data-governance template module | writing | medium | medium | document | Community-co-designed; not a generic checkbox |
| rare-cancer-registry-templates-deid-001 | De-identification design guidance (Safe Harbor / Expert Determination) | writing | medium | high | document | Guidance only; we never receive identifiable data to de-identify |
| rare-cancer-registry-templates-omopvocab-001 | Open-only OMOP vocabulary mapping pack | data | medium | medium | dataset | Exclude SNOMED/CPT-licensed vocabs; open members only |
| rare-cancer-registry-templates-jurmatrix-001 | Deceased-data jurisdictional rules matrix | research | medium | high | document | Bioethics/legal-gated; informational, not legal advice |

---

## Example task JSON

Schema-valid Task JSON for the first M0 task (`rare-cancer-registry-templates-gov-001`):

```json
{
  "id": "rare-cancer-registry-templates-gov-001",
  "title": "Standards-not-a-data-store + synthetic-data-only constitution",
  "project": "rare-cancer-registry-templates",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer-research", "open-standards", "health-data", "governance", "interoperability"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "An open, consent-first rare-cancer registry STANDARDS kit (CDEs, crosswalks, consent/governance templates) must, before any content, establish a binding constitution that it ships standards and never collects, stores, hosts, processes, or redistributes patient data of any kind. Binding Elyos cancer guardrails: open-access/aggregate/de-identified data only (here, fully SYNTHETIC fixtures + published-aggregate context only); consent-first registries (deceased/aggregate for any demonstration); no medical advice (patient-facing content is education-only with a 'not medical advice' notice and oncologist+advocate review); per-source licence verification; provenance on every assertion. Restricted terminologies (SNOMED CT, ICD-O-3, LOINC, ICCR) may only be referenced as code-only crosswalk stubs. No partner, steward, or expert reviewers are secured yet. See PLAN.md.",
  "objective": "Author the project constitution that (1) declares the standards-not-a-data-store boundary and the responsibility boundary (adopters own IRB/REC, consent, and custody; Elyos holds nothing), (2) mandates fully synthetic fixtures plus published-aggregate context only and defines the per-fixture synthetic-data attestation format, and (3) requires the no-data + not-medical-advice notices atop every published template and the docs site.",
  "acceptanceCriteria": [
    "Constitution states the project ships standards and never collects, stores, hosts, processes, or redistributes patient data (identifiable, de-identified, or row-level aggregate).",
    "Mandates fully synthetic fixtures + published-aggregate context only; defines the synthetic-data attestation format used by the CI 'no real data' scan.",
    "Defines the responsibility boundary: adopters own ethics approval, consent, and data custody; Elyos holds nothing.",
    "Requires the standards-not-a-data-store and 'not medical advice' notices atop every template and the docs site.",
    "References the binding cancer guardrails and the open-lead / restricted-stub licensing rule.",
    "Published as a human-readable governance document committed via PR, with provenance to the Elyos cancer guardrails and good-deed definition."
  ],
  "resources": [
    "planning/projects/rare-cancer-registry-templates/PLAN.md",
    "CLAUDE.md",
    "docs/good-deed-definition.md",
    "planning/ROADMAP.md"
  ],
  "output": "A governance constitution document (governance/standards-not-a-data-store.md) plus the synthetic-data attestation template, committed via PR, establishing the no-data boundary, synthetic-only policy, responsibility boundary, and required notices.",
  "requestor": "jdev1977",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
