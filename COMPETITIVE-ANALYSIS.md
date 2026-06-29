# Competitive + Improvement Analysis — `rare-cancer-registry-templates`

**Analyst:** research agent · **Date:** 2026-06-29 · **Source plan:** `Elyos-Projects/rare-cancer-registry-templates/PLAN.md` (v0.1.0, MEDIUM risk, two HIGH surfaces) · **Sibling:** `Elyos-Projects/ewing-registry-standards` (disease-specific) · **Method:** web research (WebSearch/WebFetch) against the live standards landscape, plus repo cross-checks of Elyos-Projects org siblings.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous and internally consistent: all 17 PLAN_SPEC sections present, cancer guardrails lead the design (not bolted on), the "standards-not-a-data-store" boundary is encoded as CI policy (no-real-data scan + synthetic attestation), provenance is the countable assertion unit, the licence deny-list linter is mechanical, and the honest `verifiedNeed:false` / "TO BE SECURED" posture is maintained throughout with a dated partner plan and a build-vs-mothball/pivot rule. The risk-tier split (MEDIUM engineering baseline; HIGH on patient-facing + regulatory surfaces, each with its own reviewer profile and a reviewer veto) is correct and well-calibrated. The licensing analysis is conservative and accurate (SNOMED CT affiliate-only, ICD-O-3 WHO copyright, LOINC restricted-redistribution, ICCR CC BY-NC-ND, ISO/IEC 11179 copyright-on-text, OMOP mixed per-vocab licences). These are genuine strengths and should be preserved.

**However, the most important correctness finding is a standards-alignment gap that undercuts the project's core "don't reinvent" guardrail.** The plan's standards set (mCODE/FHIR, OMOP, GA4GH Phenopackets, Mondo/ORDO/HPO/OncoTree/NCIt, ISO/IEC 11179) is strong on *oncology* and *ontology* but **omits the single most directly-overlapping prior art for "rare-disease registry CDEs"**:

- **EU JRC "Set of Common Data Elements for Rare Diseases Registration" / ERDRI-CDS** — the 16 mandatory CDEs that *every* RD registry in the EU is steered to register, plus the domain-specific CDE (DCDE) extension initiative and the `ERDRI.mdr` metadata repository ([JMIR 2022](https://medinform.jmir.org/2022/5/e32158); [EU RD Platform](https://eu-rd-platform.jrc.ec.europa.eu/)). A rare-cancer CDE set that does not declare conformance/crosswalk to ERDRI-CDS is *not* aligned to "existing standards" in the place that matters most for rare disease.
- **RD-CDM v2.0.0** — an ontology-based, FHIR + Phenopackets + IPS-compatible model that **explicitly extends ERDRI-CDS** (78 elements; +62) and is the closest existing artifact to what this project proposes ([Nature Sci Data 2025](https://www.nature.com/articles/s41597-025-04558-z); [BIH-CEI/rd-cdm](https://github.com/BIH-CEI/rd-cdm)). The plan reinvents the harmonisation layer RD-CDM already built.
- **NIH/NCATS GRDR CDEs** (75 elements) and the **RaDaR toolkit** — the US analogue, explicitly aimed at helping patient groups build registries ([RaDaR](https://registries.ncats.nih.gov/about-radar/); [GRDR CDEs, PMC4450118](https://pmc.ncbi.nlm.nih.gov/articles/PMC4450118/)).

This is a **completeness defect, not a fatal one**: the fix is additive (add ERDRI-CDS, RD-CDM, GRDR, caDSR-CDE as named upstream sources in `sources/allowlist.yml`, position the rare-cancer core as a *profile/specialisation* of ERDRI-CDS + a rare-cancer overlay on mCODE, and add ERDRI-CDS conformance to the crosswalk round-trip test). It should be raised to an M0/M1 must-fix because it is exactly the "reinvent vs. align" guardrail.

**Second-most-important finding — sibling differentiation is underspecified and risks duplication.** `ewing-registry-standards` (drafted the same day, 2026-06-28) is a *disease-specific* CDE/consent/interoperability standard whose crosswalk targets include **CDISC and REDCap** in addition to FHIR/mCODE/OMOP. `rare-cancer-registry-templates` is the *generalized template* but (a) dropped CDISC and REDCap from scope without justifying the divergence (REDCap is the dominant registry-build substrate and is the basis of RareLink — see §2), and (b) references Ewing only as a hypothetical "ewing module… extension pattern," with **no stated structural relationship** (parent abstract ↔ conformant profile) to the actual sibling repo. Two same-day, same-author plans covering "rare-cancer registry CDEs + consent + FHIR/OMOP/Phenopackets" will diverge and duplicate unless one is declared the canonical core and the other a conformance profile of it. This must be resolved in governance before M1.

**Other smaller correctness notes (all additive):**
- **FAIR is asserted but CARE/FAIR is treated as a checklist deliverable** without referencing the existing, citable "FAIR data stewardship for RD registries" guidance (CODATA DSJ 2023; ELIXIR RDMkit RD domain) — cite these so the FAIR claims are sourced, not invented.
- The **HIPAA 50-year decedent rule (§164.502(f))** and **GDPR Recital 27 + member-state post-mortem variation** are correctly stated; good. Keep these gated behind the bioethics reviewer as written.
- The plan says "OMOP CDM CC-BY-4.0" — accurate for the CDM/spec, but the **Oncology extension and per-vocab licensing** nuance (already noted) should explicitly name the OHDSI Oncology WG as the upstream-contribution venue (it is named generically as "an OHDSI oncology workgroup").
- **No explicit ISO 11179 alternative-of-record:** OSSE and ERDRI.mdr both already implement ISO/IEC 11179 MDRs for RD CDEs — the plan should cite them as the discipline-precedent rather than implying it is assembling 11179 rigor for the first time.

Net: **the plan is spec-complete and safe to circulate as a Draft, but its "align, don't reinvent" promise is currently only half-kept** because the EU/RD-registration standards layer (ERDRI-CDS, RD-CDM, GRDR) is missing from an otherwise excellent oncology/ontology alignment.

---

## 2. Competitive landscape

| Effort | What it is | Strengths | Weaknesses / gap for rare cancer |
|---|---|---|---|
| **EU JRC Set of CDEs for RD Registration / ERDRI-CDS** ([JMIR](https://medinform.jmir.org/2022/5/e32158), [EU RD Platform](https://eu-rd-platform.jrc.ec.europa.eu/)) | 16 mandatory RD-registration CDEs + domain-specific CDE (DCDE) extensions + `ERDRI.mdr` metadata repo (ISO/IEC 11179) | Authoritative, EU-endorsed, mandated baseline; ontology-coded; the de-facto European interoperability floor | Generic RD, **not oncology-specialised**; PDF/MDR distribution, weak machine artifacts; no consent toolkit bundled; no FHIR/OMOP/Phenopackets round-trip out of the box |
| **RD-CDM v2.0.0 + RareLink** ([Nature SciData](https://www.nature.com/articles/s41597-025-04558-z), [npj Genom Med](https://www.nature.com/articles/s41525-025-00534-z), [rd-cdm](https://github.com/BIH-CEI/rd-cdm)) | Ontology-based RD common data model extending ERDRI-CDS; RareLink = REDCap framework auto-exporting to FHIR + Phenopackets | **Already does the FHIR↔Phenopackets harmonisation** this plan proposes; real-world tested in 4 German hospitals; open; 2025-current | Generic RD (not cancer); REDCap-coupled (RareLink); **no rare-cancer molecular/staging depth, no oncology vocab (OncoTree), no consent-template suite** |
| **HL7 FHIR mCODE + CodeX** ([mCODE IG](https://hl7.org/fhir/us/mcode/)) | Minimal common oncology data elements; CodeX accelerator pilots (incl. cancer-registry reporting) | The oncology "lingua franca"; explicitly designed to be **extended by cancer-/cohort-specific IGs**; CDC-aligned for registry reporting | US-centric; **not rare-disease-aware** (no Orphanet/ERDRI link); no consent/governance toolkit; no FAIR/CARE layer |
| **OHDSI OMOP CDM (+ Oncology WG)** ([caDSR/OMOP](https://www.cancer.gov/about-nci/organization/cbiit/vocabulary/metadata)) | Analytics-grade common data model + Oncology extension; caDSR has curated OMOP CDEs | Huge analytics ecosystem; federated network studies; mature vocab tooling | Analysis-CDM not a *registration* standard; oncology extension still maturing; **no consent/ethics artifacts**; vocab licensing complexity (SNOMED/CPT) |
| **GA4GH Phenopackets (+ DUO)** ([GA4GH](https://www.ga4gh.org/product/phenopackets/)) | ISO-standard phenotypic exchange format; DUO for data-use tagging | Genomics-grade interop; reused by RD-Connect, RD-CDM, RareLink; community meetings active 2025 | An *exchange schema*, not a registry CDE set or governance toolkit; needs a CDE layer on top |
| **OSSE — Open Source Registry System for RD** ([osse-register.de](https://en.osse-register.de/en/), [PMC4249653](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4249653/)) | Open-source RD registry *software* with ISO/IEC 11179 MDR, EUCERD-aligned CDEs, **consent-form templates + a generic data-protection concept**, decentralised search | **Closest functional overlap on consent + governance + CDE-MDR for patient orgs**; German BMG-funded; production deployments (e.g., ESID) | Software product (not a portable standards bundle); EU/German legal framing; not oncology-specialised; not FHIR/mCODE/Phenopackets-native |
| **NCATS RaDaR / GRDR CDEs** ([RaDaR](https://registries.ncats.nih.gov/about-radar/), [GRDR CDEs](https://pmc.ncbi.nlm.nih.gov/articles/PMC4450118/)) | Educational toolkit for patient groups building registries; legacy GRDR 75 CDEs | Patient-group-facing; best-practice + governance guidance; US-authoritative | GRDR repository retired 2017; toolkit is guidance/PDF, **not machine-readable CDEs or crosswalks**; not cancer-specific |
| **RD-Connect GPAP** ([GA4GH](https://www.ga4gh.org/product/phenopackets/)) | RD genomics-phenotype analysis platform, GA4GH-standardised, pseudonymised | Real federated platform; Phenopacket-native | A platform/data custodian, not a reusable template kit; genomics-centric |
| **EURACAN / ERN registries** ([EURACAN](https://euracan.ern-net.eu/about/), [registry.euracan.eu](https://registry.euracan.eu/)) | ERN for rare adult solid cancers; 10 domains; disease-specific prospective registries; RARECAREnet definitions | **The natural rare-cancer adopter/partner**; clinical authority; RARECAREnet incidence basis | Builds disease-specific registries, not a shared open template; each domain reinvents CRFs — i.e., **the exact pain this project targets** |
| **REDCap** (de-facto registry substrate) | Ubiquitous academic data-capture; basis of RareLink | Universally available; low barrier; RareLink proves the FHIR/Phenopacket export path | Not a standard; per-project free-text drift is the *cause* of the fragmentation problem |

**Landscape conclusion:** the building blocks are mature and 2025-current, but **no one has assembled an oncology-specialised, rare-cancer, consent-first, machine-readable, copy-and-adapt standards bundle that bridges the RD-registration world (ERDRI/RD-CDM) and the oncology world (mCODE/OncoTree)** while shipping the consent/governance toolkit alongside. RD-CDM/RareLink own RD↔FHIR↔Phenopackets; mCODE owns oncology; OSSE owns consent-templates+MDR; ERDRI owns the EU baseline — but they are siloed, and none is rare-cancer-specific with a consent suite attached.

---

## 3. Gaps we can fill

1. **The oncology × rare-disease bridge.** ERDRI-CDS/RD-CDM stop at generic RD; mCODE stops at oncology and isn't RD-aware. A rare-cancer core that is **simultaneously an ERDRI-CDS profile and an mCODE overlay**, with OncoTree/ICD-O staging/molecular depth, is unoccupied territory.
2. **Consent/governance as a first-class, machine-checkable bundle.** Only OSSE ships consent templates, and as software, not a portable, license-clear, FHIR-Consent-modelled toolkit with revocation + downstream-propagation + DUO data-use tagging. This is a clear differentiating gap.
3. **A four-way round-trip conformance proof** (CDE ⇄ mCODE/FHIR, OMOP, Phenopackets — **plus ERDRI-CDS**) on synthetic data. RD-CDM proves three; adding ERDRI-CDS and OncoTree-aware oncology fields closes the loop for rare cancer.
4. **Deceased/memorial-registry consent pattern** with HIPAA 50-yr decedent + GDPR member-state post-mortem nuance — essentially absent from all competitors and uniquely valuable for ultra-rare/lethal subtypes.
5. **Patient-org-grade "starter kit"** (the RaDaR audience) but with *machine-readable* CDEs and crosswalks RaDaR lacks — i.e., RaDaR's accessibility + RD-CDM's rigor.
6. **License-clean redistributability** (open vocab leads; restricted = code-only stubs; deny-list linter), so a small patient org can legally reuse the whole bundle — something neither SNOMED-bound nor ICCR-derived assets allow.
7. **FAIR + CARE operationalisation** as a checklist tied to CDEs — CARE/Indigenous governance is essentially untouched by the EU/US registry standards.

---

## 4. Differentiators to win

1. **The bridge no one else builds:** rare-cancer-specific *and* dual-conformant to **both** the RD-registration world (ERDRI-CDS/RD-CDM) **and** the oncology world (mCODE/OncoTree/OMOP/Phenopackets). That intersection is the whole moat.
2. **Consent-first by construction, shipped as artifacts** (FHIR Consent + DUO + revocation propagation + memorial pattern), not retrofitted PDFs — beating OSSE's software-bound templates and mCODE/RD-CDM's silence on consent.
3. **Standards-never-custody** with CI-enforced no-real-data + synthetic-only attestation — a trust posture no platform competitor (RD-Connect, OSSE deployments) can match because they *hold* data.
4. **License-clean, fully open redistributable bundle** (deny-list linter; "no artifact more permissive than its most-restrictive embedded source") — directly reusable by an under-resourced patient org, unlike SNOMED/ICCR-entangled assets.
5. **vs. `ewing-registry-standards` (the critical internal differentiator):** this project is the **generalized abstract core / template kit**; Ewing should be its **first concrete conformance profile** (proving the extension pattern on one mature subtype). The win condition is *shared infrastructure*: one LinkML core + one CI/licence/provenance harness + one consent toolkit, with disease repos as thin profiles. If instead they remain two parallel full builds, both lose to duplication. **Recommendation: declare rare-cancer-registry-templates the canonical core, refactor Ewing to `profiles/ewing` conformance, and have Ewing's CDISC/REDCap targets feed back as optional modules in the general core.**

---

## 5. Claude API leverage (and the hard limits)

**Where Claude is strong (drafting + mapping under human gate):**
1. **CDE crosswalk drafting** — generate first-pass CDE ⇄ mCODE/FHIR, OMOP, Phenopackets, **and ERDRI-CDS/RD-CDM** mapping tables in LinkML, with provenance stubs, for a domain reviewer to verify and the round-trip test to validate. High-volume, well-bounded, verifiable.
2. **Governance/consent document drafting** — broad/tiered/dynamic consent templates, governance charter, data-sharing agreement, FAIR+CARE checklist, deceased/memorial pattern — as *reviewer-ready drafts* with "informational/not medical/legal advice" framing pre-inserted, every clause carrying a provenance placeholder for the bioethics reviewer.
3. **Standards reconnaissance + provenance assembly** — survey ERDRI DCDEs, RD-CDM, mCODE, caDSR/GRDR CDEs and produce a sourced comparison + `sources/allowlist.yml` candidate entries with licence notes for the licence reviewer; draft the GDPR/HIPAA/Common-Rule informational mapping with primary-source citations for legal sign-off.
4. **Conformance tooling + synthetic fixtures** — author the LinkML model, JSON-Schema/SHACL generation, the validator CLI, the CI linters (provenance/licence-deny-list/no-real-data/staleness), and **provably-synthetic** test records.

**Where Claude must NOT decide (escalate to credentialed humans):**
- **Consent, ethics, and governance validity** — lawful-basis selection, revocation adequacy, memorial/decedent ethics, IRB framing: **bioethics/IRB or health-data-privacy attorney decides**; Claude drafts only.
- **Clinical-element validity** — whether a CDE/value-set is oncologically correct and de-identified-by-design, and all patient-facing language: **clinical oncologist + patient advocate decide** (HIGH-tier, version-scoped sign-off, reviewer veto).
- **No fabricated standards or codes.** Claude must cite real ERDRI-CDS/mCODE/OncoTree/Mondo identifiers and **never invent a CDE, code, or regulatory citation**; unverifiable mappings are flagged, not asserted. The provenance-completeness linter is the backstop.
- **Licence determinations are human-verified.** Claude proposes a licence note; the **licence reviewer** approves before any source is used or any artifact ships. Restricted-terminology content is never embedded by the model.

---

## 6. Ten concrete optimizations

1. **Add ERDRI-CDS + RD-CDM + GRDR/caDSR-CDE to `sources/allowlist.yml` and make the rare-cancer core a declared *profile* of ERDRI-CDS** plus an mCODE overlay. (Fixes the §1 alignment gap; is the highest-priority change.)
2. **Add ERDRI-CDS conformance to the round-trip test** (make it a *four-way* proof: mCODE/FHIR, OMOP, Phenopackets, ERDRI-CDS) so EU-registration interoperability is mechanically guaranteed.
3. **Declare the sibling relationship in governance:** this repo = canonical core; `ewing-registry-standards` = `profiles/ewing` conformance exemplar; share the CI/licence/provenance harness and consent toolkit. Reconcile before M1 to prevent duplication.
4. **Reinstate REDCap and CDISC as optional crosswalk targets** (or explicitly justify omission): REDCap is the dominant registry substrate and RareLink's basis; CDISC is trial-sponsor-required and is in the Ewing sibling. Demonstrating a REDCap data-dictionary export would massively lower adopter barrier.
5. **Adopt RareLink/RD-CDM as a reuse baseline, not a competitor** — fork its ERDRI↔FHIR↔Phenopacket mappings (open-licensed) and *specialise* for oncology, satisfying "assemble, don't reinvent" and saving months.
6. **Ship a REDCap/OSSE-importable artifact** of the CDE set so the two largest patient-org registry substrates can adopt with one file — turns "a standard" into "a thing you can load Tuesday."
7. **Pre-build a synthetic conformance test-bank generator** (seeded, attested) as an M0 deliverable so every later CDE/crosswalk PR is testable immediately and the no-real-data guarantee is structural.
8. **Cite the existing FAIR-for-RD-registries guidance** (CODATA Data Science Journal 2023; ELIXIR RDMkit RD domain) in the FAIR+CARE checklist so FAIR claims are sourced, and add CARE references explicitly rather than as an open question only.
9. **Target an ERN/EURACAN domain as the lead adopter** (its 10 domains each hand-roll CRFs — the exact pain): scope the M3 pilot to *harmonising one EURACAN domain registry's* CDEs against the standard, a concrete, reachable "delivered" outcome.
10. **Make `validUntil` cadences source-type-specific and auto-PR** — ontology releases (Mondo/HPO/ORDO ship dated releases), FHIR/mCODE versions, and regulatory text each get a distinct review interval; a staleness job opens a re-verify PR rather than only flagging, so HIGH-tier withholding is actioned.

---

## 7. Parallel & perpendicular spin-offs

- **Shared rare-disease registry-standard toolkit (perpendicular core):** extract the LinkML core + CI harness (provenance/licence-deny-list/no-real-data/staleness linters) + synthetic-fixture generator + consent toolkit as a **standalone, disease-agnostic toolkit** that any Elyos registry-standard project (Ewing, future GIST/sarcoma) instantiates. This is the single highest-leverage spin-off — it converts N parallel registry projects into 1 core + N thin profiles.
- **`ewing-registry-standards` (parallel, existing sibling):** refactor to the *first conformance profile* of this core; it already carries CDISC/REDCap targets that become optional core modules. Demonstrates the extension pattern (M4) on a real, mature subtype.
- **`ewing-outcomes-harmonization` (parallel, existing sibling):** its endpoint/event definitions (OS/EFS comparability) are a natural **outcomes module** of the rare-cancer CDE set — harmonised outcome CDEs + crosswalk, so registries built on the template are outcome-comparable by construction.
- **`open-cohort-catalog` (parallel, existing sibling):** registries built to this standard become *catalogable* cohorts; align the catalog's metadata schema to the CDE set so conformant registries auto-surface in the catalog (FAIR "Findable").
- **mCODE/Phenopackets alignment contribution (upstream perpendicular):** package the rare-cancer overlay as a candidate mCODE child-IG and a Phenopackets rare-cancer profile, contributed to CodeX / a GA4GH driver project — the plan's named sustainability path, made concrete.
- **MCP server (perpendicular tooling):** a **registry-standards MCP server** exposing CDE lookup, crosswalk translation (CDE→FHIR/OMOP/Phenopackets/ERDRI), licence-status checks, and consent-conformance validation as tools an agent/registry-builder can call — turning the static standard into a queryable service (non-data-holding, so guardrail-safe). Per Elyos `CLAUDE.md`, MCP/agent-specific logic lives in `adapters/`, keeping the core neutral.

---

## 8. Open questions

1. **Is ERDRI-CDS conformance a hard requirement or a crosswalk?** (Determines whether the EU-registration mandate is satisfied by-construction or by-mapping — affects M1 scope.)
2. **Core-vs-profile governance with the Ewing sibling:** who owns the canonical core, and what is the contribution/versioning contract between the general template and disease profiles?
3. **Build on RareLink/RD-CDM or alongside?** Forking RD-CDM's ERDRI↔FHIR↔Phenopacket mappings could save months — is that the preferred "assemble" strategy, and is its licence compatible?
4. **REDCap/CDISC in or out?** The plan dropped both vs. the Ewing sibling — justified scope-control, or a barrier to adoption that should be reversed (at least an optional REDCap export)?
5. **Lead-adopter shape:** a EURACAN/ERN domain (clinical authority, but heavyweight governance) vs. a single disease-specific patient foundation (nimble, but smaller) — which best meets the dated 2027-01-31 community-secured gate?
6. **DUO/consent technical depth** — template + FHIR Consent model only, or a reference (non-data-holding) consent-state schema, without drifting toward "a service"? (The plan's own open question; the MCP spin-off intensifies it.)
7. **CARE-principles depth** in a general template vs. left to adopter context — and which existing FAIR-for-RD guidance to normatively cite.
8. **Compensation/independence of HIGH-tier reviewers** (volunteer vs. funded-lane review hours with a hard cap) without compromising the reviewer veto.

---

## Summary (top-line)

`rare-cancer-registry-templates` is a rigorous, guardrail-leading plan whose "standards-never-custody," synthetic-only, CI-enforced provenance/licence posture is genuinely strong and should be preserved. Its competitive position is sound because the precise intersection it targets — an oncology-specialised, rare-cancer, consent-first, machine-readable, license-clean standards bundle — is **unoccupied**.

**Top 3 competitors:** (1) **EU JRC ERDRI-CDS / RD-CDM + RareLink** — the RD-registration baseline and the existing ERDRI↔FHIR↔Phenopacket harmonisation (2025-current, REDCap-based); (2) **HL7 FHIR mCODE / CodeX** — the oncology lingua franca built to be extended by cancer-specific IGs; (3) **OSSE** — the open-source RD registry software that already ships ISO 11179 CDEs + consent-form templates + a data-protection concept. (Honourable mentions: NCATS RaDaR/GRDR, OHDSI OMOP, GA4GH Phenopackets, EURACAN/ERN as the natural adopter.)

**Single strongest differentiator:** it is the only effort bridging the **rare-disease-registration world (ERDRI-CDS/RD-CDM) and the oncology world (mCODE/OncoTree)** in one rare-cancer, consent-first, fully-open, redistributable bundle — and, internally, the **generalized shared-infrastructure core** from which `ewing-registry-standards` becomes a thin conformance profile.

**Top 3 Claude-API ideas:** (1) draft CDE crosswalks (incl. ERDRI-CDS/RD-CDM) in LinkML with provenance for human verification; (2) generate reviewer-ready consent/governance/FAIR-CARE templates and the GDPR/HIPAA/Common-Rule informational mapping with primary-source citations; (3) author the conformance tooling — validator, CI linters, and provably-synthetic fixtures. In all three, Claude drafts; clinical/ethics/legal/licence validity and any fabricated standard/code are hard-gated to credentialed humans.

**Two most important plan-correctness findings:** (1) the plan's "align, don't reinvent" guardrail is only half-kept — it **omits ERDRI-CDS, RD-CDM, and GRDR/caDSR-CDE**, the most directly-overlapping rare-disease-registration prior art; add them as upstream sources, position the core as an ERDRI-CDS profile + mCODE overlay, and make the round-trip a four-way proof. (2) **Sibling differentiation is underspecified:** `ewing-registry-standards` was drafted the same day with overlapping scope (and extra CDISC/REDCap targets); declare this repo the canonical core, refactor Ewing to a conformance profile, and share one CI/licence/consent harness to avoid duplication.
