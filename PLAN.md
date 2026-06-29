# rare-cancer-registry-templates — PLAN.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: **MEDIUM** (engineering/standards surface) with specific **HIGH**
> surfaces (patient-facing consent/education language; legal/regulatory mapping) gated on
> credentialed expert sign-off.

Open, **consent-first** registry **standards** — common data elements (CDEs), metadata schemas,
interoperability crosswalks, and data-governance + consent templates — that any rare-cancer
community, patient organisation, or natural-history registry can adopt. **This project ships
standards, not a data store.** It never collects, hosts, processes, or redistributes patient data
of any kind. Its outputs are schemas, profiles, ontology mappings, governance charters, and
consent artifacts that *adopters* instantiate under *their own* ethics approval and consent.

---

## Executive summary

Rare cancers (individually < 6 / 100,000 / year; collectively ~20–24% of all cancer diagnoses and a
disproportionate share of cancer mortality) are the textbook case where data fragmentation kills.
No single centre sees enough patients to learn from; small disease-specific registries spring up in
spreadsheets with incompatible fields, ad-hoc or absent consent, no provenance, and no path to
interoperate, so the data can never be pooled, compared, or reused — the very thing rare-disease
research most needs. The shortage is not data *collection* tooling; it is **shared, rigorous,
consent-first standards** that make small datasets trustworthy, FAIR, and poolable from day one.

This project fills that gap by publishing the **standards layer**: a minimal-yet-extensible rare-
cancer Common Data Element set mapped to existing open oncology standards (mCODE/FHIR, OMOP CDM,
GA4GH Phenopackets) and open disease/phenotype ontologies (Mondo, Orphanet/ORDO, HPO, OncoTree,
NCI Thesaurus); a **consent-first governance toolkit** (broad/tiered/dynamic consent templates,
Data Use Ontology tagging, governance-charter and data-sharing-agreement templates, and an
informational GDPR/HIPAA/Common-Rule mapping); and machine-readable conformance artifacts so an
adopter can stand up an interoperable registry without reinventing — or under-protecting — anything.

**The single most important design fact, and the headline guardrail, is that we are a standards
project and never a data custodian.** The Elyos cancer guardrails are binding and lead the design:

- **Open-access / aggregate / de-identified data only — and in practice, synthetic-only here.**
  Controlled-access resources (dbGaP, EGA, individual-level biobanks) and *any* identifiable patient
  data are categorically **out of scope**. Because this is a standards project, we go further than the
  minimum: **every example, fixture, and reference instance is fully synthetic**; we cite only
  *published aggregate* statistics (e.g., RARECAREnet incidence bands) for context, and we ingest **no**
  patient-level records, de-identified or otherwise. Re-identification risk is engineered to zero by
  never holding a record in the first place.
- **Consent-first by construction.** The registry templates we publish are *designed for* prospective,
  revocable, informed consent — not retrofitted onto already-collected data. The lower-barrier
  reference pattern we *demonstrate* uses **aggregate or deceased-patient (memorial-registry)** data
  only, and even that is shown with synthetic stand-ins. Living-patient registries built from our
  templates are run by adopters under their own IRB/REC approval and consent — that is their
  responsibility boundary, made explicit in every template.
- **No medical advice.** Nothing here diagnoses, prescribes, or guides treatment. Any patient- or
  family-facing language (consent forms, plain-language explainers) is **education-only**, carries an
  explicit **"not medical advice"** notice, and is **HIGH risk-tier**: it does not ship without
  **oncologist and patient-advocate review and sign-off**.
- **Provenance on every assertion.** Every CDE, every code-system binding, and every governance/legal
  claim carries a machine-checkable provenance link to its source standard or authority, with that
  source's licence verified and recorded before it ships.

**Risk posture.** The project is **MEDIUM** on its standards/engineering surface (it must be
domain-accurate and interoperable) and **HIGH** on two specific surfaces — patient/family-facing
consent and education language, and any regulatory/legal mapping (GDPR Art. 9, HIPAA, Common Rule).
HIGH surfaces require credentialed sign-off (a clinical oncologist *and* a patient advocate for
patient-facing content; a bioethics/IRB professional and/or a health-data-privacy attorney for the
legal mapping) before merge, and the legal mapping is always framed **"informational, not legal
advice."**

**Honesty note: no rare-cancer registry partner, patient organisation, or steward is yet secured,
and no expert reviewers are engaged.** Every delivery-dependent task is marked **TO BE SECURED** with
`verifiedNeed: false`. The project is not "shipped" on merge: it is shipped only when a real
rare-cancer community adopts the standards to stand up (or harmonise) a consent-first registry, with
the consent/governance artifacts independently reviewed. Until then the value is a reviewed,
reference-quality standards bundle — genuinely useful, but not yet *delivered* in the Elyos sense.

**Partner-acquisition plan (dated) and build-vs-mothball decision rule.** Outreach is time-boxed
against the build rather than left open-ended. By **2026-08-31**: ≥ 3 candidate adopters in active
conversation (a rare-cancer patient organisation, a natural-history-registry effort, or a rare-cancer
reference network such as an ERN/EURACAN-affiliated group) and the open-ontology licensing review
complete. By **2026-10-31**: a clinical-oncology reviewer and a patient-advocate reviewer secured
(gates HIGH patient-facing content); a bioethics/IRB or health-data-privacy reviewer secured (gates
the legal mapping). By **2027-01-31**: an adopting community/steward secured (gates the
adoption/shipped milestone). **Decision rule:** if no HIGH-tier reviewers are secured by the M2 entry
date, HIGH-tier (consent/legal/patient-facing) work does **not** start and the project holds at M1
(open standards + synthetic fixtures). If no adopting community is secured by **~2027-03-31**, the
project does **not** publish patient-facing consent templates to no one: it either (a) **pivots** the
last-mile beneficiary — hand the reviewed engineering/ontology standards to an existing standards
body (e.g., an OHDSI oncology workgroup, a GA4GH driver project, or a rare-disease registry network)
as a contribution — or (b) **mothballs** to maintenance-only, with the decision recorded in
governance. The non-patient-facing standards artifacts remain valuable in either case.

---

## Problem & beneficiaries

**Who is helped (directly):** the people who build and run small rare-cancer registries —
disease-specific **patient organisations and foundations**, **clinician-researchers** starting a
natural-history study, **rare-cancer reference networks** (e.g., ERN EURACAN–style groups), and
**bioinformaticians/data managers** who today hand-roll a schema and a consent form per registry.

**Who is helped (ultimately):** **rare-cancer patients and their families**, who benefit when
small, scattered datasets become trustworthy, consented, comparable, and poolable — the precondition
for natural-history insight, trial feasibility, and equitable inclusion of ultra-rare subtypes that
no single centre can study alone.

**The need.** A patient organisation that wants to "start a registry" faces a cruel gap: the
research value depends entirely on *interoperability and governance* they have no resources to get
right. The common failure modes are well documented in the rare-disease literature: incompatible
free-text fields (no controlled vocabulary, so "GIST" / "gastrointestinal stromal tumour" /
"c-KIT+ sarcoma" never join); absent or legally fragile consent (no revocation path, no data-use
specification, no lawful basis for sharing); no provenance or versioning; and no mapping to the
standards (FHIR/mCODE, OMOP, Phenopackets, ICD-O-3, ontologies) that downstream researchers and
trial sponsors actually require. The result is registries that can never be pooled and patients
whose participation is wasted. Authoritative building blocks exist (mCODE, OMOP, GA4GH Phenopackets
and DUO, Mondo/ORDO/HPO, ISO/IEC 11179, the ICCR pathology datasets, FAIR + CARE principles), but
they are scattered across specialist communities, partly behind restrictive licences, and have never
been **assembled into a rare-cancer-specific, consent-first, copy-and-adapt starter kit**.

**Verified need / partner:** **TO BE SECURED.** The *gap* is real and well-evidenced in the
rare-disease registry literature, but **no specific patient organisation, registry effort, or
reference network has yet committed to adopt or co-develop these standards**, and no expert reviewers
are engaged. Target partner profiles: a disease-specific rare-cancer foundation/patient group; an
academic natural-history-registry team; a rare-cancer reference network (ERN EURACAN affiliate, an
NCI rare-tumour effort, or a rare-disease registry infrastructure such as an RD-Connect/ERDRI-style
body); and, for review, a clinical oncologist, a patient advocate, and a bioethics/IRB or
health-data-privacy professional. Until a partner is secured the project builds the **agent-neutral
open-standards core, synthetic fixtures, and reviewer-ready drafts**, and marks all
adoption/HIGH-tier work `TO BE SECURED`. Outreach is **dated, not open-ended** (see the
partner-acquisition plan in the executive summary) and a **build-vs-mothball/pivot decision rule**
governs slippage.

---

## Goals and non-goals

**Goals**

- Publish an open, **rare-cancer-specific Common Data Element (CDE) set** — minimal core + optional
  modules — each element defined to ISO/IEC 11179 metadata discipline, bound to open code systems,
  and carrying provenance.
- Provide **interoperability crosswalks** from the CDE set to mCODE/HL7 FHIR, the OMOP CDM, and GA4GH
  Phenopackets, so an adopter's registry is poolable and trial-ready by construction.
- Ship a **consent-first governance toolkit**: broad/tiered/dynamic consent templates, a Data Use
  Ontology (DUO) tagging scheme, a governance-charter template, a data-sharing-agreement template,
  and a **deceased-patient / memorial-registry** consent pattern — all designed for revocability and
  explicit data-use specification.
- Provide an **informational** mapping of the CDE/consent design to GDPR Art. 9 / Art. 89, HIPAA
  (incl. de-identification standards and the decedent rule), and the revised Common Rule — clearly
  labelled **"informational, not legal advice,"** primary-source-cited, and expert-reviewed.
- Make conformance **machine-checkable**: JSON Schema / LinkML artifacts, a FHIR
  ImplementationGuide, a validator, and a synthetic conformance test-bank, so adopters can verify a
  registry's data *and* governance against the standard.
- Prove the standards are *usable* by validating them with a real rare-cancer community and getting
  at least one consent-first registry stood up or harmonised against them.

**Non-goals**

- **Not a registry and not a database.** We never collect, store, host, process, broker, or
  redistribute patient data — identifiable, de-identified, or aggregate-at-the-row-level. (Out of
  scope, full stop; see *Scope* and *Security & privacy*.)
- **Not a source of medical advice**, diagnosis, prognosis, treatment guidance, or eligibility
  determinations. Patient-facing text is education-only with a "not medical advice" notice.
- **Not legal advice.** The regulatory mapping is informational and routes adopters to their own
  counsel, IRB/REC, and Data Protection Officer.
- **Not a controlled-access or genomic-data-sharing platform.** No dbGaP/EGA-style access brokering,
  no individual-level genomic data, no Beacon/DUO *enforcement* engine — we publish the *tagging
  vocabulary and templates*, not an access-control service holding data.
- **Not a redistributor of restrictively-licensed terminologies.** We *reference* SNOMED CT, ICD-O-3,
  LOINC, and ICCR datasets by code/identifier and provide crosswalk *stubs*; we do not embed or
  re-publish their proprietary content (see *Data, licensing & compliance*).
- **Not a single-disease project.** It is a reusable template kit; per-disease specialisation
  (e.g., a GIST or Ewing module) is an extension pattern, validated on at most one or two exemplar
  subtypes, not an exhaustive per-cancer build.

---

## Success metrics (outcomes)

Outcome-centric and beneficiary-first. Adoption-of-standards and patient-protection outcomes count;
"number of schema files" and downloads do **not** count as success on their own.

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Consent-first registries stood up or harmonised against the standard | 0 | ≥ 1 real adopter registry (pilot) instantiates the CDE set **and** the consent/governance toolkit | Adopter confirmation + a conformance report; recorded in the outcomes log |
| Patient consent artifacts reviewed before any patient-facing publication | n/a | **100%** of patient/family-facing consent & education text carries recorded **oncologist + patient-advocate** sign-off | Reviewers ledger; CI gate on `patient-facing: true` artifacts |
| CDE provenance coverage | n/a | **100%** of CDEs carry a resolvable provenance link + a verified source-licence note | Provenance-completeness linter in CI |
| Restrictive-terminology containment | n/a | **0** files redistribute SNOMED CT / ICD-O-3 / LOINC / ICCR proprietary content; only codes/IDs + crosswalk stubs present | Licence linter (deny-list scan) in CI + licence reviewer audit |
| Interoperability round-trip | n/a | A synthetic record expressed in the CDE set round-trips to **mCODE FHIR, OMOP CDM, and Phenopackets** and validates against each | Crosswalk conformance test in CI |
| Synthetic-only guarantee | n/a | **0** real patient records (de-identified or otherwise) in the repo or pipeline; every fixture provably synthetic | Data-provenance attestation + CI "no real-data" scan |
| Consent-revocation completeness | n/a | **100%** of consent templates specify a working **revocation + downstream-propagation** path and a data-use specification | Governance-template checklist; expert review |
| Regulatory-mapping sourcing | n/a | **100%** of GDPR/HIPAA/Common-Rule claims primary-source-cited and expert-reviewed; every page labelled "informational, not legal advice" | Citation-coverage test; bioethics/legal reviewer sign-off |
| Standards-staleness containment | n/a | **0** code-system/standard bindings served past their `validUntil` without an auto-flag + re-verification | Staleness test against `lastVerified`/`validUntil` |
| Reuse by a standards/registry community | 0 | ≥ 1 documented downstream adoption, citation, or upstream contribution (e.g., into an OHDSI/GA4GH/ERN workstream) | Documented citation/use |

**Defining success outcome (Definition of Shipped):** a real rare-cancer community adopts the
standards to stand up or harmonise a **consent-first** registry, with the consent and governance
artifacts independently reviewed (oncologist + advocate for patient-facing text; bioethics/legal for
the regulatory mapping), conformance demonstrated on synthetic data, and **no patient data ever held
by this project**.

---

## Scope

**In scope**

- A **rare-cancer CDE set**: a minimal core (de-identified-by-design fields only) + optional modules
  for diagnosis/histology, staging, molecular/biomarker, treatment, outcome/vital status, and
  **consent metadata**, each defined to ISO/IEC 11179 discipline and bound to open vocabularies.
- **Interoperability crosswalks** to mCODE/FHIR, OMOP CDM, and GA4GH Phenopackets, plus crosswalk
  *stubs* (code mappings, not content) to SNOMED CT / ICD-O-3 / LOINC for adopters who hold the
  relevant licences.
- A **consent-first governance toolkit**: consent templates (broad, tiered, dynamic), a DUO-based
  data-use tagging scheme, governance-charter and data-sharing-agreement templates, a
  deceased-patient/memorial-registry pattern, and a FAIR + CARE alignment checklist.
- An **informational regulatory mapping** (GDPR Art. 9/89, HIPAA incl. de-identification + decedent
  rule, revised Common Rule), expert-reviewed and clearly non-advisory.
- **Machine-readable artifacts + conformance tooling**: LinkML/JSON-Schema definitions, a FHIR
  ImplementationGuide, a validator, and a **synthetic** conformance test-bank.
- A documentation site (the "starter kit") and an adopter onboarding guide.

**Out of scope (explicitly will NOT do)**

- **Any patient data.** No collection, storage, hosting, processing, brokering, or redistribution of
  identifiable, de-identified, *or* row-level aggregate patient data. We are not a data custodian.
  (Synthetic fixtures and *published* aggregate statistics for context only.)
- **Controlled-access data and individual-level genomics** (dbGaP, EGA, biobank microdata): not
  ingested, not mapped from real records, not brokered.
- **De-identification *as a service*.** We publish de-identification *guidance and field-level design*
  so adopters de-identify; we never receive identifiable data to de-identify.
- **Medical advice / clinical decision support / eligibility determination.**
- **Legal advice or jurisdiction-specific legal opinions** beyond the cited informational mapping.
- **Redistribution of proprietary terminologies** (SNOMED CT, ICD-O-3, LOINC full tables; ICCR
  datasets under CC BY-NC-ND): referenced by code/ID + crosswalk stub only.
- **A running access-control / Beacon / consent-enforcement service holding data.**
- An exhaustive per-rare-cancer build; specialisation is demonstrated, not completed for all subtypes.

The **standards-not-a-data-store boundary is the project's ethical spine**, not a disclaimer. It is
encoded as a CI-enforced policy (a "no real data" scan + a synthetic-data attestation on every
fixture) and stated at the top of every template so an adopter cannot mistake the templates for a
hosted service or believe Elyos has assumed custody of their patients' data.

---

## Solution approach & architecture

This is a **content/data-standards** project (not an app): the "pipeline" produces specifications,
machine artifacts, and validators rather than a running service. Stack: TypeScript/ESM + pnpm for
tooling/validators; **LinkML** as the authoring source-of-truth for the CDE metadata model (it
generates JSON Schema, SHACL, and documentation and supports class/slot-level provenance and
mappings); **HL7 FHIR + mCODE** profiles authored as a FHIR ImplementationGuide; **OMOP CDM** and
**GA4GH Phenopackets** as crosswalk targets. All artifacts are plain, diffable text in git.

**Components**

1. **CDE metadata model (`cde/`, LinkML source-of-truth).** Each Common Data Element is a LinkML
   slot/class with: a definition, datatype, permissible-value set bound to a **named code system**, a
   **provenance** annotation (source standard + identifier + retrieval date + licence note + the
   ISO/IEC 11179 administered-item discipline), a `patientFacing` flag, and a `deIdentDesign` note
   stating why the field is safe-by-design (e.g., year-of-birth not full DOB; geographic generalisation;
   no free-text identifiers). The model is **minimal-core-first**: the core holds only fields that are
   de-identified by design; richer fields live in optional modules an adopter switches on knowingly.

2. **Open-vocabulary binding layer (`vocab/`).** Permissible values bind **primarily to open code
   systems** — **Mondo** (CC-BY-4.0) and **Orphanet/ORDO** (CC-BY-4.0) for disease; **OncoTree** and
   **NCI Thesaurus** for oncology concepts; **HPO** for phenotype. Bindings to **restricted** systems
   (SNOMED CT, ICD-O-3, LOINC) are expressed as **crosswalk stubs** — a code-to-code mapping table with
   the *target codes referenced by identifier only*, never the proprietary descriptions/hierarchy — so
   an adopter with the appropriate licence can complete them. A licence linter blocks any commit that
   embeds restricted content.

3. **Interoperability crosswalks (`crosswalks/`).** Deterministic mappings CDE ⇄ **mCODE/FHIR**, CDE ⇄
   **OMOP CDM**, CDE ⇄ **GA4GH Phenopackets**, each with a round-trip conformance test on synthetic
   records. The crosswalk is the proof that "small registry built from our template" is automatically
   poolable into the big open-oncology ecosystems.

4. **Consent & governance toolkit (`governance/`).** Markdown + structured (YAML/FHIR Consent
   resource) templates: broad/tiered/**dynamic** consent, with explicit **data-use specification**
   (DUO codes), **revocation + downstream-propagation** language, return-of-results policy, and a
   **deceased-patient/memorial-registry** consent pattern (which carries its *own* ethics notes — see
   *Data, licensing & compliance* — rather than treating "deceased" as a consent-free shortcut). Plus a
   governance-charter template, a data-sharing-agreement template, and a FAIR + CARE checklist.

5. **Regulatory mapping (`compliance/`, HIGH-tier).** An informational crosswalk from the design to
   GDPR (Art. 9 special-category basis, Art. 89 research safeguards), HIPAA (Privacy Rule,
   §164.514(b) Safe Harbor + Expert Determination de-identification, and the §164.502(f) 50-year
   decedent rule), and the revised Common Rule (45 CFR 46, incl. §46.116(d) broad consent). Every
   claim primary-source-cited; every page labelled **"informational, not legal advice"**;
   bioethics/legal sign-off required before publish.

6. **Conformance tooling (`tools/`).** A CLI validator (`validate`) that checks a *synthetic* dataset
   against the CDE schema, the crosswalk round-trips, and a **governance-conformance checklist**
   (does the consent artifact specify revocation, data-use, lawful basis placeholder, provenance?).
   Plus the CI linters: provenance-completeness, licence deny-list, "no real data" scan, and
   staleness.

7. **Starter-kit docs site (`docs/`).** Generated from LinkML + the FHIR IG: the human-readable
   standard, the adopter onboarding guide, and the "how to run this consent-first" guidance — with the
   **standards-not-a-data-store** and **not-medical-advice** notices front-and-centre.

**Key decisions**

- **Standards, never custody.** The architecture has *no* data-ingestion or storage component by
  design; the only data in the repo is synthetic fixtures, enforced in CI.
- **Open vocabularies lead; restricted ones are stubs.** This keeps the whole bundle redistributable
  under open licences and pushes the licensing burden to the adopter who already holds the licence.
- **Authoritative reuse over invention.** We *assemble and specialise* mCODE/OMOP/Phenopackets/
  ontologies for rare cancers rather than inventing a rival standard; novelty is the rare-cancer
  consent-first packaging, not a new data model.
- **Consent is a first-class schema citizen,** not a PDF afterthought: consent metadata is in the CDE
  model and a FHIR Consent resource, with revocation modelled.
- **Agent-neutral, vendor-neutral** per Elyos core rules; no agent-specific logic in the artifacts.

---

## Data, licensing & compliance

**This section leads with the binding Elyos cancer guardrails because they are the design, not a
constraint bolted on afterward.**

**Cancer-data guardrails (binding, non-negotiable):**

1. **Open-access / aggregate / de-identified data only — synthetic-only in practice.** Controlled-
   access (dbGaP, EGA, individual-level biobanks) and **any identifiable patient data** are out of
   scope. Because we are a standards project, **every fixture/example/reference record is fully
   synthetic** and labelled as such; we cite only **published aggregate** statistics for context. We
   ingest **no** patient-level data, de-identified or otherwise. A CI "no real data" scan and a
   per-fixture synthetic-data attestation enforce this.
2. **Consent-first registries; deceased/aggregate for any demonstration.** The templates are designed
   for prospective, revocable, informed consent. Any *demonstration* instance uses aggregate or
   deceased-patient patterns only — and even those use synthetic stand-ins. Real living-patient data
   is always the adopter's, under the adopter's IRB/REC and consent.
3. **No medical advice.** Patient/family-facing content is education-only, carries a "not medical
   advice" notice, and is HIGH-tier (oncologist + advocate sign-off before publish).
4. **Provenance on every assertion.** Every CDE, code-system binding, and regulatory claim carries a
   resolvable provenance link with a verified source licence.

**Source standards and their licences (verified before use; recorded in `sources/allowlist.yml`):**

| Source | Use | Licence / status | Handling |
|---|---|---|---|
| **mCODE / HL7 FHIR** | CDE alignment, FHIR IG, Consent resource | HL7 IP, free to use; mCODE IG content openly licensed | Reference + profile; verify IG licence per version |
| **OMOP CDM (OHDSI)** | crosswalk target | CDM CC-BY-4.0; conventions Apache-2.0 | Map to; attribute |
| **OMOP Standardized Vocabularies** | concept mapping | **mixed per-vocab licences** (some require SNOMED/CPT licences) | Use **only** open member vocabs; restricted ones via stub |
| **GA4GH Phenopackets** | crosswalk target | Open (Apache-2.0-style) | Map to; attribute |
| **GA4GH Data Use Ontology (DUO)** | data-use tagging | CC-BY-4.0 | Reuse codes; attribute |
| **Mondo Disease Ontology** | disease vocab (primary) | CC-BY-4.0 | Bind; attribute |
| **Orphanet / ORDO** | rare-disease vocab | CC-BY-4.0 | Bind; attribute |
| **Human Phenotype Ontology (HPO)** | phenotype vocab | Permissive HPO licence (free) | Bind; preserve notice |
| **OncoTree** | oncology tumour types | Open (MSK) | Bind; attribute |
| **NCI Thesaurus (NCIt) / caDSR CDEs** | oncology concepts, CDE precedent | Open / public (NCI) | Reuse; attribute |
| **ISO/IEC 11179** | metadata-registry *discipline* | ISO copyright on the **standard text** | Apply the model/principles; **do not** copy standard text |
| **SNOMED CT** | clinical terminology | **Restricted** (SNOMED International affiliate licence; member-territory terms) | **Crosswalk stub only** — codes/IDs, never descriptions/hierarchy |
| **ICD-O-3** | tumour morphology/topography | **WHO copyright** (restricted redistribution) | **Crosswalk stub only** — codes referenced, content not embedded |
| **LOINC** | lab/observation codes | Regenstrief licence (free, **restricted redistribution** of the table) | **Crosswalk stub only**; preserve licence terms |
| **ICCR datasets** | pathology reporting alignment | typically **CC BY-NC-ND** (non-commercial, **no-derivatives**) | Reference/align by citation; **no derivative reproduction** |
| **RARECAREnet / published incidence** | rare-cancer definitions, context | aggregate published stats | Cite aggregate only; no microdata |

**The headline licensing gate.** Several oncology terminologies that adopters will *want* (SNOMED CT,
ICD-O-3, LOINC) and the ICCR pathology datasets carry **restrictive licences** (affiliate-only,
WHO copyright, no-derivatives). The project's hard rule: **lead with open vocabularies (Mondo, ORDO,
HPO, OncoTree, NCIt); express bindings to restricted systems as code-only crosswalk stubs; never
embed or re-publish restricted content.** A **licence deny-list linter** scans every commit for
embedded restricted descriptions/hierarchies and **fails the build** on a hit; no source is used
until its `sources/allowlist.yml` entry is `approved` by the **licence reviewer**.

**Provenance model.** Each administered item (CDE, value-set binding, crosswalk row, governance
clause, regulatory claim) is the **countable assertion unit** and must carry a `provenance` record:
source name + identifier/citation + retrieval date + URL + verified licence/legal status. A
provenance-completeness linter enforces 100% coverage; an item without provenance fails CI.

**Staleness is fail-safe, not best-effort.** Standards move (FHIR/mCODE versions, ontology releases,
de-identification guidance, and especially **regulatory text**). Every source carries `lastVerified`
and `validUntil` (per-source-type review interval — e.g., ontologies on each major release; the
regulatory mapping on a short cadence). At validation/build time, a binding or claim past its
`validUntil` is **auto-flagged** ("past review window — re-verify") and, for HIGH-tier regulatory
claims, **withheld** from the published mapping until a reviewer re-verifies and (for HIGH content)
an expert re-signs-off, which writes a fresh `lastVerified`/`validUntil`. Silent staleness becomes a
visible, gated event.

**Privacy / PII stance (conservative, and structurally trivial here).** The project **holds no
patient data**, so the classic PII threat surface is *engineered out* rather than mitigated. The
residual privacy responsibilities are: (a) the **CDE design** must be de-identified-by-construction
in the minimal core (no full DOB, no free-text identifiers, geographic generalisation), with any
re-identification-risky field quarantined in an optional, clearly-flagged module the adopter enables
knowingly; (b) **synthetic fixtures** must be provably synthetic (generated, attested, scanned) so no
real record can leak in via an "example"; (c) the **deceased-patient pattern** is *not* treated as a
privacy-free zone — see below.

**Deceased-patient / memorial-registry nuance (handled explicitly, not as a loophole).** "Deceased
only" lowers some legal barriers but does **not** remove ethical and some legal obligations: under
**HIPAA, decedent PHI remains protected for 50 years** after death (§164.502(f)); **GDPR** does not
apply to deceased persons (Recital 27) but **several member states legislate post-mortem data
protection**; and surviving-relative privacy, genetic-relatedness implications, and family/community
dignity persist regardless. The memorial-registry consent pattern therefore carries **explicit ethics
notes**, a **next-of-kin/authorised-representative consent path**, jurisdictional caveats, and the
same "informational, not legal advice" framing — and is reviewed by the bioethics reviewer.

**Output licensing.** Code/tooling/validators: **MIT** (or Apache-2.0 — open question, governance
decision). Schemas, CDE definitions, crosswalks, governance/consent templates, and docs:
**CC-BY-4.0** (attribution to source standards preserved; consent templates explicitly marked
adaptable). Where a CC-BY-SA upstream is incorporated, that artifact inherits **CC-BY-SA-4.0**.
**No** artifact may carry a more permissive licence than its most-restrictive embedded source allows
(the licence linter checks this); restricted terminologies are never embedded, so they never
contaminate the bundle licence.

**Attribution.** Every reused standard/ontology is attributed in a `CREDITS`/`NOTICE` file and inline
in provenance. Expert reviewers are credited (with consent) in a version-scoped reviewers ledger.

---

## Quality, review & risk gates

**Risk tier: MEDIUM** overall (domain-accurate, interoperable standards), with **two HIGH-tier
surfaces** that require credentialed sign-off before merge:

- **Patient/family-facing content** (consent forms, plain-language explainers): **HIGH** — requires
  **clinical-oncologist + patient-advocate** sign-off and a "not medical advice" notice.
- **Regulatory/legal mapping** (GDPR/HIPAA/Common Rule): **HIGH** — requires **bioethics/IRB and/or
  health-data-privacy attorney** sign-off; framed "informational, not legal advice."

Engineering/standards tasks (CDE model, crosswalks, tooling, CI) are **low–medium**; the consent
*architecture* (revocation/data-use modelling) is **medium** with a bioethics review touchpoint.

**Required reviews before a deed is "done":**

- **Maintainer** review on every PR (engineering quality, agent-neutral core, no real data, no
  embedded restricted terminology, CI green: `pnpm build && pnpm test && pnpm lint`).
- **Licence reviewer** sign-off on every source addition and on the licence-deny-list/“no real data”
  CI results before any vocabulary/crosswalk work.
- **Clinical-oncologist + patient-advocate** sign-off on any `patientFacing: true` artifact (HIGH).
- **Bioethics/IRB or health-data-privacy** sign-off on the consent toolkit's lawful-basis/data-use
  design and on the entire regulatory mapping (HIGH).
- **Standards/domain reviewer** check that crosswalks round-trip and bindings are correct.

**Every patient-facing surface is labelled "Education only — not medical advice"** and every
regulatory surface "Informational — not legal advice; consult your IRB/REC, DPO, and counsel."

**Definition of Shipped (project):** a real rare-cancer community adopts the standards to stand up or
harmonise a **consent-first** registry; the consent/governance artifacts and any patient-facing text
are independently reviewed and signed off; conformance (CDE + crosswalk round-trips + governance
checklist) is demonstrated on **synthetic** data; provenance is 100% and licences verified; and **no
patient data is ever held by this project**.

---

## Roadmap & milestones

Phased: open-standards spine and the no-data/licence guardrails first; CDEs + crosswalks next;
consent/governance + HIGH-tier regulatory mapping only once reviewers are secured; adoption last and
gated on a secured community.

- **M0 — Governance & standards spine (cold-start).**
  *Goal:* the no-data + licence guardrails and an agent-neutral standards skeleton exist before any
  content. *Exit:* the **standards-not-a-data-store constitution** + **synthetic-data-only policy**
  merged; `sources/allowlist.yml` schema + licence-gate policy merged with ≥ 5 candidate standards
  analysed and the open core (Mondo/ORDO/HPO/OncoTree/NCIt/mCODE/OMOP/Phenopackets/DUO) `approved`;
  the **countable assertion unit + provenance model** ratified; LinkML repo skeleton + CI gates live
  (provenance-completeness, **licence deny-list**, **"no real data" scan**); ISO/IEC 11179 *discipline*
  (not text) adopted. **Reviewer recruitment opened** (oncologist, advocate, bioethics/legal, licence
  reviewer) with status logged.

- **M1 — Core CDE set + interoperability crosswalks.**
  *Goal:* a minimal, de-identified-by-design rare-cancer core + poolability proof. *Exit:* minimal-core
  CDE set (demographics-minimal, diagnosis via Mondo/ORDO/OncoTree, histology, vital status, consent
  metadata) authored in LinkML with 100% provenance and open bindings; **crosswalks to mCODE/FHIR,
  OMOP, Phenopackets** with a **synthetic record round-tripping and validating against all three**;
  restricted-terminology crosswalk **stubs** present (codes only, licence linter green); validator CLI
  runs in CI. **Licence reviewer signs off** all bindings.

- **M2 — Consent & governance toolkit + regulatory mapping (HIGH-tier, reviewer-gated).**
  *Goal:* the consent-first governance suite and informational legal mapping. *Entry gate:* HIGH-tier
  reviewers secured (oncologist+advocate; bioethics/legal) — **if not, M2 does not start; hold at M1.**
  *Exit:* broad/tiered/dynamic consent templates with revocation + data-use (DUO) + return-of-results;
  governance-charter + data-sharing-agreement templates; deceased-patient/memorial pattern with ethics
  notes; FAIR+CARE checklist; the GDPR/HIPAA/Common-Rule mapping primary-source-cited and
  **bioethics/legal-signed-off**; all patient-facing text **oncologist+advocate-signed-off** and
  "not medical advice"-labelled; governance-conformance checklist in the validator.

- **M3 — Conformance, starter-kit & pilot adoption (the deed).**
  *Goal:* prove usability and get a real community to adopt. *Exit (Definition of Shipped):* FHIR
  ImplementationGuide + docs starter-kit published; synthetic conformance test-bank green; a **secured
  rare-cancer community adopts** the CDE set + consent/governance toolkit to stand up or harmonise a
  consent-first registry; artifacts independently reviewed; outcomes recorded. *(Gated on a secured
  partner — TO BE SECURED.)*

- **M4 — Sustain & standards stewardship (post-delivery).**
  *Goal:* durable maintenance and careful extension. *Exit:* maintenance/reviewer rotation + version
  policy + staleness cadence in force; a documented **per-disease extension pattern** validated on one
  exemplar subtype; upstream-contribution path (OHDSI/GA4GH/ERN) pursued; outcomes dashboard (adoptions,
  not downloads).

Dependencies flow **M0 → M1 → M2 → M3 → M4**. The **licence review (M0→M1)** gates all vocabulary
work; **HIGH-tier reviewer availability gates M2 entry**; a **secured adopting community gates M3**.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`**: ~18 tasks across M0–M4 plus a future
backlog, each mapped to the Elyos Task JSON schema, with per-task acceptance criteria for the most
important items, milestone Definitions of Done, and a complete example Task JSON for the first M0
task (the standards-not-a-data-store + synthetic-data-only constitution). The first build items are
the **constitution + licence-gate + provenance model**, reflecting that the no-data and licensing
guardrails are hard prerequisites; HIGH-tier consent/legal/patient-facing work is sequenced into M2
behind secured credentialed reviewers.

---

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns the standards architecture, the agent-neutral/no-data core, CI
  gates, version policy, and merge quality.
- **Licence reviewer:** approves every source in `sources/allowlist.yml`, audits the licence
  deny-list and "no real data" CI results; a **hard exit gate** — if the seat is empty, vocabulary
  work cannot proceed.
- **Standards/domain reviewer:** validates CDE definitions and crosswalk correctness/round-trips.
- **Credentialed HIGH-tier reviewers (TO BE SECURED):**
  - **Clinical oncologist** + **patient advocate** — sign off all patient/family-facing content
    (consent forms, explainers). Sign-off is **version-scoped** (attaches to a specific content
    version + citation set; later edits require re-sign-off, dovetailing with `validUntil` staleness).
  - **Bioethics/IRB professional and/or health-data-privacy attorney** — sign off the consent
    lawful-basis/data-use design and the entire regulatory mapping.
  - **Independence / COI:** a reviewer recuses from any matter touching a registry or organisation
    they govern or are funded by; COIs are disclosed in the reviewers ledger. **Name-use limits:** a
    reviewer's name/credentials may be published only for the versions they approved, with consent,
    and may not imply blanket endorsement. **Disagreement fallback:** on whether HIGH-tier content is
    ethically/legally safe to publish, the expert holds a **veto** the maintainer cannot override; a
    "do not publish" stands, is logged, and escalates to Elyos governance / a second reviewer.
- **Steward (last-mile owner): TO BE SECURED** — owns the adopting-community relationship and the
  adoption that constitutes shipping.
- **Partner / requestor: TO BE SECURED** — the rare-cancer patient organisation, registry effort, or
  reference network adopting the standards.
- **Community / board:** the MIT-vs-Apache and CC-BY-vs-CC-BY-SA licence decisions and any edge cases
  go through Elyos governance.

---

## Dependencies & integrations

- **Standards / vocabularies (sources):** mCODE/HL7 FHIR, OMOP CDM + open OMOP vocabs, GA4GH
  Phenopackets + DUO, Mondo, Orphanet/ORDO, HPO, OncoTree, NCIt/caDSR, ISO/IEC 11179 discipline;
  *restricted* (stub-only): SNOMED CT, ICD-O-3, LOINC, ICCR datasets; *context only*: RARECAREnet
  aggregate stats. Each gated through the licence reviewer + allow-list.
- **Tooling:** LinkML (CDE source-of-truth + JSON-Schema/SHACL/doc generation), HL7 FHIR IG
  publisher/Sushi (FHIR profiles), OMOP/Phenopackets validators, TypeScript/ESM + pnpm for the CLI
  validator and CI linters.
- **Elyos pieces:** `packages/schema` (Task JSON), `CLAUDE.md` work rules + refusal/cancer guardrails,
  `docs/good-deed-definition.md` (risk tiers), Elyos governance for licence/edge-case decisions.
- **Human/decision dependencies (critical path):** the **licence review** (gates all vocab/crosswalk
  work); **secured HIGH-tier reviewers** (gate M2); a **secured adopting community/steward** (gates
  M3 / Definition of Shipped).

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Project drifts into holding/handling real patient data | Low | Critical | No data-ingestion/storage component by design; CI "no real data" scan + per-fixture synthetic attestation; standards-not-a-data-store constitution as the first merged artifact; restated atop every template | Maintainer |
| Restricted terminology (SNOMED/ICD-O-3/LOINC/ICCR) embedded/redistributed | Medium | High | Lead with open vocabularies; restricted = code-only crosswalk **stubs**; licence deny-list linter fails the build on embedded content; licence-reviewer gate | Licence reviewer |
| Patient-facing consent/education text is inaccurate or feels like medical advice | Medium | High | HIGH-tier: oncologist + advocate sign-off before publish; "not medical advice" notice; plain-language review; version-scoped sign-off + staleness re-verification | Oncologist / Advocate |
| Regulatory mapping wrong or read as legal advice | Medium | High | Primary-source citations + bioethics/legal sign-off; "informational, not legal advice" on every page; staleness fail-safe withholds stale legal claims | Bioethics/legal reviewer |
| "Deceased-only" treated as a consent/ethics-free shortcut | Medium | High | Memorial pattern carries explicit ethics notes, next-of-kin consent path, HIPAA 50-yr decedent rule + GDPR member-state caveats; bioethics review | Bioethics reviewer |
| Standards go stale (FHIR/ontology/regulatory updates) | High | Medium | `lastVerified`/`validUntil` per source; staleness test auto-flags/withholds; review cadence; version policy | Maintainer |
| No adopting community secured → cannot reach Definition of Shipped | High | High | Honest `verifiedNeed:false`/`TO BE SECURED`; dated partner plan (≥3 in convo by 2026-08-31; community by 2027-01-31) + build-vs-mothball/pivot rule (~2027-03-31: contribute to a standards body or mothball) | Steward / Maintainer |
| No HIGH-tier reviewers secured → M2 blocked | Medium | High | Recruit via oncology societies, patient-advocacy orgs, bioethics/IRB networks; hard gate — do not start/ship HIGH content without sign-off; hold at M1 | Maintainer |
| Re-identification risk via a CDE field or a "synthetic" fixture that isn't | Medium | High | De-identified-by-design minimal core; risky fields quarantined in opt-in modules; synthetic-data generation + attestation + CI scan | Standards reviewer |
| Adopters misread templates as a hosted Elyos service / custody | Medium | Medium | Standards-not-a-data-store + responsibility-boundary notice atop every template and the docs site | Maintainer |
| Over-fitting to one disease, hurting reusability | Low | Medium | Minimal-core + optional modules; specialisation validated on ≤2 exemplar subtypes; extension pattern documented | Standards reviewer |
| Licence contamination of the bundle (share-alike/NC creep) | Medium | Medium | "No artifact more permissive than its most-restrictive embedded source" rule, checked by licence linter; restricted content never embedded | Licence reviewer |

---

## Security & privacy

**Threat surface (deliberately small).** Because the project holds **no patient data**, the dominant
risks of a registry (PII exposure, cross-tenant leakage, exfiltration, re-identification) are
**designed out** rather than defended. The residual surface is: (a) **accidental ingestion of real
data** into the repo (via a contributor pasting a "real example"); (b) **embedded restricted
terminology** (a licensing, not privacy, breach); (c) **re-identification-enabling CDE design** (a
field set that makes adopter registries unsafe); (d) **misuse of the templates** to build a
non-consensual or surveillance-style registry; (e) ordinary supply-chain/secrets hygiene for the
tooling.

**Controls.**
- **No-data architecture** as the top control: no storage/ingestion component exists; CI **"no real
  data" scan** + **per-fixture synthetic-data attestation** block real records from entering the repo.
- **Licence deny-list linter** blocks embedded SNOMED/ICD-O-3/LOINC/ICCR content (licensing + IP
  control).
- **De-identified-by-design CDE core**; re-identification-risky fields are quarantined in opt-in
  modules with explicit warnings, so an adopter cannot stumble into an unsafe schema.
- **Anti-misuse framing (refusal guardrails):** the templates and docs state that the standards are
  for **consensual, ethically-approved** registries; any request to adapt them for **non-consensual,
  surveillance, discriminatory, re-identification, or for-profit-primary** use is **refused and
  flagged** per `CLAUDE.md`. The consent-first design makes non-consensual use *harder by default*
  (no consent artifact → governance checklist fails).
- **Provenance + staleness** as integrity controls (every claim sourced; stale claims auto-flagged).
- **Secrets hygiene:** no secrets/tokens/PII in logs, receipts, or committed files (Elyos rule);
  dependency + secret scanning in CI for the tooling.

**Abuse/misuse prevention.** The refused-use set — non-consensual collection, surveillance/profiling,
re-identification, discrimination, redistribution of restricted terminologies, and primary benefit to
a for-profit — is stated, and the **consent-first + no-data** design structurally resists it. Where a
contribution or adoption request crosses these lines, the agent **stops, refuses, and surfaces the
concern** rather than proceeding.

---

## Sustainability & maintenance

After delivery a named **maintenance + reviewer rotation** owns the standard: a **version policy**
(semantic versioning of the CDE set, crosswalks, and templates), a **staleness cadence**
(`lastVerified`/`validUntil` driving auto-flag/withhold for ontology/FHIR/regulatory updates), and
**re-sign-off** for any material change to HIGH-tier patient-facing or regulatory content. Outcomes
tracked are **adoptions and patient-protection facts** (consent-first registries stood up,
conformance reports, upstream contributions), **not** downloads or stars, surfaced on an outcomes
dashboard. The preferred long-term home is **upstream**: contribute the rare-cancer specialisation
into an existing standards community (an OHDSI oncology workgroup, a GA4GH driver project, or a
rare-disease registry network) so the standard outlives this project. The licence allow-list and the
deny-list linter are maintained as living guards as terminology licences evolve.

---

## Open questions

- **Code licence: MIT vs Apache-2.0?** Apache-2.0 adds an explicit patent grant (useful around
  standards); MIT maximises simplicity/adoption. Governance decision. (Content: CC-BY-4.0, with
  CC-BY-SA inheritance where required.)
- **Which exemplar rare cancer(s) to specialise on** for the M4 extension pattern (e.g., a sarcoma
  subtype with mature open standards vs. an ultra-rare tumour with the greatest data-poverty)?
  Decided with the adopting community.
- **How are HIGH-tier reviewers compensated/credited** (volunteer vs. a future funded lane for review
  hours with a hard budget cap) without compromising independence?
- **CARE-principles operationalisation:** how far to specify Indigenous/community data governance in a
  general template vs. leaving it to the adopter's context?
- **Dynamic-consent technical depth:** ship the *template + FHIR Consent model* only, or also a
  reference (non-data-holding) consent-state schema? (Risk of scope creep toward "a service.")
- **Deceased-data jurisdictional matrix:** how many jurisdictions' post-mortem-data rules to map
  before the bioethics reviewer is comfortable publishing the memorial pattern broadly?
- **Lane:** donated by default; is a future funded lane warranted for expert-review hours or for the
  FHIR IG publishing toolchain, with a hard budget cap?

---

## References

- Proposal: *not yet written* — this PLAN/TASKS pair precedes a formal
  `governance/proposals/rare-cancer-registry-templates.md` (TO BE WRITTEN).
- Elyos work rules, refusal + cancer guardrails: `CLAUDE.md`
- Good-deed definition & risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Portfolio roadmap (Track 8 cancer guardrails): `planning/ROADMAP.md`
- House-style sibling plans: `planning/projects/public-official-guide/{PLAN,TASKS}.md`,
  `planning/projects/revolutionary-patriots-kg/{PLAN,TASKS}.md`
- Standards referenced: HL7 FHIR + mCODE; OHDSI OMOP CDM; GA4GH Phenopackets + Data Use Ontology;
  Mondo; Orphanet/ORDO; HPO; OncoTree; NCI Thesaurus / caDSR; ISO/IEC 11179; (restricted, stub-only)
  SNOMED CT, ICD-O-3, LOINC, ICCR datasets; RARECAREnet (aggregate context).
- Regulatory primary sources (informational mapping): GDPR Arts. 9, 89 + Recital 27; HIPAA Privacy
  Rule §164.514(b) (de-identification) and §164.502(f) (50-year decedent rule); revised Common Rule
  45 CFR 46 incl. §46.116(d) (broad consent); FAIR principles; CARE Principles for Indigenous Data
  Governance.

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and are **already applied**
in the body above (each notes where).

1. **Synthetic-only, not merely "de-identified."** Strengthened the guardrail from the portfolio's
   "open/aggregate/de-identified" to **fully synthetic fixtures + published-aggregate context only**,
   engineering re-identification risk to zero. *(Exec summary; Data §1; Security.)*
2. **Standards-not-a-data-store made the first merged artifact** (a "constitution"), not a tagline —
   with a CI **"no real data" scan** + per-fixture synthetic attestation. *(Scope; Architecture; M0;
   TASKS rcrt-gov-001.)*
3. **Licence deny-list linter** that fails the build on embedded SNOMED CT/ICD-O-3/LOINC/ICCR content,
   making the headline licensing gate mechanical. *(Data; Security; M0.)*
4. **Open vocabularies lead; restricted ones are code-only crosswalk stubs** — keeps the whole bundle
   redistributable and pushes the licence burden to the adopter who holds it. *(Architecture §2; Data.)*
5. **ICCR's CC BY-NC-ND captured specifically** (non-commercial **and** no-derivatives) so we align by
   citation and never reproduce a derivative. *(Data licence table.)*
6. **Deceased-patient nuance handled, not exploited:** HIPAA 50-year decedent rule (§164.502(f)),
   GDPR Recital 27 + member-state post-mortem laws, next-of-kin consent, surviving-relative/genetic
   implications. *(Data; Risks; Open questions.)*
7. **Consent modelled as a first-class schema citizen** (CDE consent module + FHIR Consent resource
   with revocation + DUO data-use), not a PDF afterthought. *(Architecture §4; Goals; Metrics.)*
8. **Revocation + downstream-propagation** required in every consent template and checked by the
   governance-conformance checklist. *(Architecture §6; Metrics; M2.)*
9. **Round-trip interoperability proof** (synthetic record ⇄ mCODE/OMOP/Phenopackets, validating
   against all three) as the concrete "poolability" success metric. *(Metrics; M1.)*
10. **ISO/IEC 11179 applied as discipline, not copied text** — avoiding ISO-copyright contamination
    while gaining metadata-registry rigor. *(Architecture; Data table.)*
11. **Countable assertion unit defined** (the administered item) so the 100%-provenance gate is
    mechanically checkable. *(Data provenance model; M0.)*
12. **Fail-safe staleness** with `lastVerified`/`validUntil`, including **withholding** stale HIGH-tier
    regulatory claims until re-sign-off. *(Data; Sustainability; Metrics.)*
13. **Two explicit HIGH surfaces split from a MEDIUM baseline** (patient-facing vs. regulatory),
    each with its *own* reviewer profile, rather than a blanket tier. *(Risk gates; Governance.)*
14. **HIGH-tier reviewer availability gates M2 entry** — content work cannot start without secured
    oncologist+advocate and bioethics/legal reviewers. *(Roadmap M2; Risks.)*
15. **Dated partner-acquisition plan + build-vs-mothball/pivot rule** instead of an open-ended TBD —
    with a named pivot (contribute upstream to OHDSI/GA4GH/ERN). *(Exec summary; Sustainability; Risks.)*
16. **Outcome metrics are beneficiary-centric** (consent-first registries adopted, artifacts reviewed,
    provenance/synthetic guarantees) — downloads/stars explicitly excluded. *(Metrics; Sustainability.)*
17. **"No artifact more permissive than its most-restrictive embedded source"** licence rule, linter-
    checked, preventing share-alike/NC contamination. *(Data; Risks.)*
18. **De-identified-by-design minimal core** with re-identification-risky fields quarantined in opt-in,
    clearly-flagged modules. *(Architecture §1; Security; Risks.)*
19. **Anti-misuse / refusal framing operationalised:** consent-first design *structurally* fails the
    governance checklist for non-consensual/surveillance use, beyond a written refusal. *(Security.)*
20. **Version-scoped expert sign-off + name-use limits + COI recusal + veto/disagreement fallback**
    for credentialed reviewers. *(Governance.)*
21. **Authoritative-reuse-over-invention** stance: we specialise mCODE/OMOP/Phenopackets/ontologies
    for rare cancers rather than inventing a rival standard. *(Architecture key decisions; Non-goals.)*
22. **FAIR + CARE alignment** (incl. Indigenous/community data governance) made an explicit toolkit
    deliverable and an open question, not assumed. *(Architecture §4; Open questions.)*
23. **Explicit responsibility-boundary notice** atop every template (adopter owns IRB/REC, consent,
    and custody) to prevent "Elyos holds our data" misreadings. *(Scope; Security; Risks.)*
24. **Honest "no proposal yet / no partner / no reviewers" disclosure** with `verifiedNeed:false`
    throughout, rather than inventing a partner. *(Exec summary; Problem; References; TASKS.)*
25. **Upstream-contribution sustainability path** (donate the standard to a standards body) so the
    deed outlives the project and isn't orphaned on a single repo. *(Sustainability; Risks; pivot rule.)*

---

## Review sign-off

**Reviewer:** drafting engineer/TPM (self-review pass) · **Date:** 2026-06-28 · **Against:**
`PLAN_SPEC.md` (17 sections), `CLAUDE.md` (rules + cancer guardrails), `docs/good-deed-definition.md`
(5 criteria + risk tiers), `packages/schema/src/schemas.ts`.

**Completeness check.** All 17 required H2 sections present and in order: Executive summary; Problem &
beneficiaries; Goals and non-goals; Success metrics (outcomes); Scope; Solution approach &
architecture; Data, licensing & compliance (leads with cancer guardrails ✔); Quality, review & risk
gates; Roadmap & milestones (M0 cold-start + measurable exits ✔); Work breakdown (points to TASKS ✔);
Governance, roles & stakeholders; Dependencies & integrations; Risks & mitigations (table ✔);
Security & privacy; Sustainability & maintenance; Open questions; References. Plus Appendix A (25
improvements applied) and this sign-off.

**Correctness check & fixes applied during review.**
- Verified the metadata header carries Status/Version/Date/Owner/Lane per `PLAN_SPEC.md` conventions,
  and added the risk-tier qualifier (MEDIUM baseline + named HIGH surfaces) so it matches the body —
  **fixed**.
- Verified every cancer guardrail in the task brief is reflected and *binding*: open/aggregate/
  de-identified→synthetic-only ✔; consent-first/deceased-aggregate ✔; not-a-data-store ✔; no medical
  advice + "not medical advice" + oncologist/advocate review (HIGH) ✔; per-source licence verification
  ✔; provenance on every assertion ✔.
- Confirmed risk tiers align with `good-deed-definition.md`: health/patient-facing + legal mapping →
  HIGH with credentialed sign-off before merge; standards engineering → low–medium. Corrected an
  earlier ambiguity where consent architecture was unlabelled — now **medium with a bioethics
  touchpoint** — **fixed**.
- Confirmed the five good-deed criteria hold: tangible public benefit (rare-cancer research
  enablement); freely available (CC-BY/MIT, open standards); not primarily for-profit (templates for
  patient communities); no harm/discrimination/misinformation (consent-first + refusal guardrails +
  expert review); specific enough for AI sessions with the required review gates.
- Confirmed honesty: no partner, steward, or reviewers invented; `verifiedNeed:false` and "TO BE
  SECURED" used throughout; the absence of a formal proposal file is disclosed in References.
- Cross-checked the licensing claims for conservativeness (SNOMED CT affiliate-only, ICD-O-3 WHO
  copyright, LOINC restricted redistribution, ICCR CC BY-NC-ND, ISO 11179 copyright, OMOP vocab mixed
  licences) and ensured each is handled as **stub/reference-only** — no embedded restricted content.
- Verified TASKS.md exists with schema-mapped tasks, a schema-valid example Task JSON
  (`verifiedNeed:false`), milestone tables, acceptance criteria, and DoDs aligned to this roadmap.

**Outstanding (human decisions required):** secure an adopting rare-cancer community/steward; secure
HIGH-tier reviewers (oncologist + advocate; bioethics/legal) and a licence reviewer; MIT-vs-Apache-2.0
licence decision; choice of exemplar rare cancer(s) for the M4 extension pattern; reviewer
compensation model. These are tracked in *Open questions* and as `TO BE SECURED` in the roadmap.

**Verdict:** spec-complete and internally consistent; safe to circulate as a Draft (v0.1.0). Not
"shipped" until the Definition of Shipped is met with a real adopter and independent review.
