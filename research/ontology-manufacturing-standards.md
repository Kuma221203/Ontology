# Research: Reusable Standards & Ontologies for a Garment-Factory Ontology

Purpose: before designing a custom OWL ontology for a garment/apparel factory's
business domain, this document surveys existing standards and published
ontologies in manufacturing and apparel/textile, so the project can reuse
established vocabulary where it genuinely fits instead of reinventing it.

Research method: primary sources (standards-body pages, official spec PDFs,
GitHub repos, W3C recommendations) were fetched directly where possible.
Where a primary artifact could not be reached or does not appear to exist,
that is stated explicitly rather than silently substituting a blog summary.

---

## 1. ISA-95 / IEC 62264 (Manufacturing Operations Management)

**Spec source:** Official ISA page —
[isa.org/standards-and-publications/isa-standards/isa-95-standard](https://www.isa.org/standards-and-publications/isa-standards/isa-95-standard).
IEC's parallel numbering is summarized (secondary, Wikipedia-derived, flagged
as such) at [en.wikipedia.org/wiki/IEC_62264](https://en.wikipedia.org/wiki/IEC_62264).

**Structure (confirmed from the ISA page itself):** ISA‑95 (ANSI/ISA‑95,
= IEC 62264) is eight parts:
- Part 1 — Models and Terminology
- Part 2 — Objects and Attributes for Enterprise-Control System Integration
- Part 3 — Activity Models of Manufacturing Operations Management
- Part 4 — Objects and Attributes for Manufacturing Operations Management Integration
- Part 5 — Business-to-Manufacturing Transactions
- Part 6 — Messaging Service Model
- Part 7 — Alias Service Model
- Part 8 — Information Exchange Profiles

(source: [isa.org ISA-95 standard page](https://www.isa.org/standards-and-publications/isa-standards/isa-95-standard))

**Core object-model entities:** The ISA.org overview page does *not* itself
enumerate Personnel/Equipment/Material/Process Segment/etc. — those are
defined inside the paywalled Part 1 and Part 2 documents. To ground the
entity list in a readable primary-adjacent source, the OPC Foundation's OPC
UA companion specification "OPC 10030: ISA-95 Common Object Model" (an
OPC Foundation specification built directly on ISA-95 Part 2/3, not a blog)
was checked:
- §4.2.4 "Modelling Approach of ISA-95" ([reference.opcfoundation.org/specs/OPC-10030/4.2.4](https://reference.opcfoundation.org/specs/OPC-10030/4.2.4)):
  **Personnel Class** instances define a role a person can perform;
  **Equipment** models describe roles equipment plays in production,
  inventory management and quality-test labs; the **Material** model covers
  lots, sublots, material definitions and material classes, used mainly for
  tracing material through production.
- Automation World's technical explainer of ISA-95 Part 3 production
  segments (cited for the **Process Segment** definition only because the
  ISA/IEC part itself is paywalled): a Process Segment defines the plant
  capabilities needed to execute a step; segments combine into product
  segments such as production routes or control recipes
  ([automationworld.com, "ISA-95 Implementation Best Practices: Production Models and Segments"](https://www.automationworld.com/products/control/news/13301218/isa-95-implementation-best-practices-production-models-and-segments)).
- The OPC 10030 spec's own scope note states it initially covers only
  role-based Equipment, Physical Asset, Material and Personnel models, and
  explicitly leaves Process Segment and Product Definition for a later
  revision — confirming these are distinct, separately-modeled ISA-95
  entities ([reference.opcfoundation.org/ISA-95](https://reference.opcfoundation.org/ISA-95)).

Production Schedule and Production Performance are modeled in ISA-95 Part 3/4
(Activity Models / Objects and Attributes for MOM) but could not be
confirmed against a freely readable primary page — flagged as unverified
beyond the paywall.

**OWL formalization — searched explicitly, result is mostly negative:**
- No official OWL ontology is published by ISA or MESA International for
  ISA-95 or for B2MML (the XML/XSD exchange format MESA publishes for
  ISA-95; B2MML is schemas only, no RDF/OWL — [MESA B2MML course listing](https://pathlms.com/mesa/courses/26161), [GitHub B2MML topic](https://github.com/topics/b2mml)).
- Microsoft-affiliated project **opendigitaltwins-isa95**
  ([github.com/JMayrbaeurl/opendigitaltwins-isa95](https://www.github.com/JMayrbaeurl/opendigitaltwins-isa95))
  models ISA-95 as **DTDL** (Digital Twins Definition Language), *not* OWL/RDF.
  Marked "work in progress."
- The **Industrial Ontology Foundry (IOF) Biopharmaceutical Manufacturing
  Ontologies (BMIC)** suite is a real, open, OWL-based ontology repo at
  [github.com/iofoundry/ontology](https://github.com/iofoundry/ontology)
  (see also the portal page [spec.industrialontologies.org/portal/release/202603/biopharma.html](https://spec.industrialontologies.org/portal/release/202603/biopharma.html)
  and the sponsor announcement [oagi.org/bmic](https://oagi.org/bmic)). It is
  built on the IOF Core Ontology and QUDT, and is explicitly designed to
  *align with* ISA-88/ISA-95 concepts for biopharma MES/ERP/SCADA/OPC-UA
  data — but it is a biopharma-manufacturing-specific suite that maps *to*
  ISA-95 ideas, not a general-purpose OWL rendering of the ISA-95 standard
  itself.
- Academic papers (Garmendia et al.; Leukel et al., extending to ISA-88
  batch constructs) are reported, secondhand, by a 2026 arXiv preprint
  ([arxiv.org/pdf/2608.21418](https://arxiv.org/pdf/2608.21418)) to have
  formalized ISA-95 in OWL, but no public ontology file/repo for either
  paper could be located in this search — **flagged: unverified primary
  artifact, cite with caution.**

**Verdict: useful as inspiration only.** ISA-95's entity vocabulary
(Personnel/Equipment/Material/Process Segment/Product Definition/Production
Schedule/Production Performance) is a strong conceptual skeleton for a
factory MES-style ontology, but there is no single, canonical, freely
reusable OWL file to import — the project should borrow ISA-95's *class
names and relationships* as design inspiration and model them from scratch
in OWL, rather than trying to import/align with a nonexistent official
ontology.

---

## 2. MASON, OntoCAPE, and W3C SSN/SOSA

### MASON (MAnufacturing's Semantics ONtology)

- **Origin:** Proposed by Lemaignan, Siadat, Dantan and Semenenko (LGIPM,
  École Nationale Supérieure d'Arts et Métiers, Metz, France), "MASON: A
  Proposal For An Ontology Of Manufacturing Domain," IEEE, 2006 —
  [ieeexplore.ieee.org/document/1633441](https://ieeexplore.ieee.org/document/1633441/figures),
  full text mirror: [typeset.io PDF](https://typeset.io/pdf/mason-a-proposal-for-an-ontology-of-manufacturing-domain-3133gvc6mv.pdf).
- **Formalism/structure:** OWL-DL. Three top classes: **Entity** (products),
  **Operation** (manufacturing processes), **Resource** (machine tools,
  human resources, geographic resources) — confirmed via the ontology's
  registry entry at the ENIT Industry Portal:
  [industryportal.enit.fr/ontologies/MASON](https://industryportal.enit.fr/ontologies/MASON),
  which lists namespace `http://www.owl-ontologies.com/mason.owl`, version
  1.0.0, status "Beta," release date 2006-06-16, 246 classes, 102
  individuals, 55 properties.
- **Actual file location:** Hosted on SourceForge,
  [sourceforge.net/p/mason-onto](https://sourceforge.net/p/mason-onto/) /
  [sourceforge.net/projects/mason-onto/files/](https://sourceforge.net/projects/mason-onto/files/).
  The project description states it "relies on OWL-DL formalism." The files
  tree shows a `mason-onto` folder (last modified 2006-02-01); this tool
  could not drill into the folder's contents to confirm an individual `.owl`
  filename, so **treat the exact download link as needing manual
  verification**, though the project is real and the namespace/registry
  metadata (ENIT) independently corroborates an OWL file exists.
- **Status:** Appears dormant since ~2006; no evidence of active
  maintenance found.
- **Downstream reuse:** The ExtruOnt machine ontology aligns with MASON, and
  a 2019 ISARC construction-robotics paper used a MASON-based approach to
  link construction products to production-line machinery
  ([iaarc.org ISARC 2019 Paper 162](https://www.iaarc.org/publications/fulltext/ISARC_2019_Paper_162.pdf)).

**Verdict: useful as inspiration only.** MASON's Entity/Operation/Resource
split is a clean, small upper-ontology pattern worth imitating conceptually
for a factory ontology (Product / Process / Resource), but it's a stale,
hard-to-fetch 2006 artifact, not something to formally import.

### OntoCAPE

- **What it is:** A large (62 sub-ontologies) OWL ontology for Computer-Aided
  Process Engineering from RWTH Aachen's AVT Process Systems Engineering
  group (Wolfgang Marquardt et al.), published as the book *OntoCAPE: A
  Re-Usable Ontology for Chemical Process Engineering* (Springer, 2010,
  RWTHedition) —
  [link.springer.com/book/10.1007/978-3-642-04655-1](https://link.springer.com/10.1007/978-3-642-04655-1).
  A 2008 ESCAPE conference paper by the same group states "an extensive
  documentation of OntoCAPE as well as an implementation of the ontology in
  the OWL modeling language were available" at an avt.rwth-aachen.de URL —
  the exact current homepage/domain could not be confirmed live in this
  session (the search result pointed to an address that could not be
  verified as still resolving).
- **Scope:** Chemical/continuous process engineering (unit operations,
  materials, chemical reactions) — not discrete apparel/garment
  manufacturing.
- **Verdict: not applicable.** Domain mismatch (continuous chemical process
  plants vs. discrete-batch garment production); not worth adapting.

### W3C SSN / SOSA (Semantic Sensor Network / Sensor, Observation, Sample, and Actuator)

- **Primary source:** W3C Recommendation, 2023 edition —
  [w3.org/TR/vocab-ssn-2023/](https://www.w3.org/TR/vocab-ssn-2023/).
  SOSA namespace: `http://www.w3.org/ns/sosa/` (prefix `sosa:`); SSN
  namespace: `http://www.w3.org/ns/ssn/` (version IRI
  `http://www.w3.org/ns/ssn/2023/`). SOSA is described as "a minimal
  interoperability fall-back level" providing classes/properties for
  exchanging data across all uses of SSN. The 2023 edition stays backward
  compatible with 2017-edition data; the 2017 RDF files remain at a
  `/sosa/2017`-suffixed permanent version IRI. It aligns with ISO
  19156:2023, and provides an SSN–SAREF alignment module. Editor's draft:
  [w3c.github.io/sdw-sosa-ssn/ssn/](https://w3c.github.io/sdw-sosa-ssn/ssn/).
- **Relevance:** SSN/SOSA models Sensor/Observation/ActuatableProperty
  concepts — directly applicable if the factory ontology ever needs to
  represent IoT sensors on sewing/cutting machines (e.g. machine uptime
  sensors, RFID readers), but it does not model manufacturing business
  processes, products, orders, or BOMs at all.

**Verdict (SSN/SOSA): directly reusable, but narrow scope** — reusable
specifically as the vocabulary for any sensor/IoT/equipment-monitoring layer
bolted onto the factory ontology later; not a substitute for the core
business-process/product ontology.

---

## 3. GS1 Standards (Apparel/General Merchandise, EPCIS, GS1 Web Vocabulary)

**GS1 Apparel and General Merchandise:** A dedicated apparel page could not
be located directly on the root gs1.org domain in this search; the apparel
and general merchandise ("AGM") initiative is primarily documented on GS1
member-organization sites rather than gs1.org itself:
[GS1 US AGM Initiative](https://www.gs1us.org/agminitiative),
[GS1 UK apparel page](https://www.gs1uk.org/industries/retail/apparel) (ties
GS1 identifiers to incoming EU Digital Product Passport requirements from
2027), [GS1 Australia General Merchandise & Apparel](https://www.gs1au.org/general-merchandise-and-apparel-new).
**This is flagged explicitly:** these are regional-affiliate pages, not the
central gs1.org standard text; no single authoritative gs1.org "Apparel
Standard" document (equivalent to the GS1 General Specifications) was found
and fetched directly in this session.

**GTIN and identifiers:** GS1's core identifier is the **GTIN** (Global
Trade Item Number), applied per distinct tradeable product/variant (e.g.
each size/color combination of a garment typically gets its own GTIN), with
**SSCC** (Serial Shipping Container Code) for logistics units and **GCN**
(Global Coupon Number)/other GS1 keys for related concepts — summarized from
a barcode-implementation guide
([barcodeguide.seagullscientific.com, "Ind_Retail"](https://barcodeguide.seagullscientific.com/Content/Ind_Retail.htm)).
Direct GTIN definition page on gs1.org (`gs1.org/standards/id-keys/gtin`)
returned HTTP 403 to automated fetch in this session and could not be read
directly — **flagged: GTIN description above is from a secondary summary,
not confirmed against the gs1.org page text itself.**

**EPCIS (traceability events):**
- **Spec home (primary, gs1.org-hosted):**
  [ref.gs1.org/standards/epcis](https://ref.gs1.org/standards/epcis) /
  current version [ref.gs1.org/standards/epcis/2.0.1/](https://ref.gs1.org/standards/epcis/2.0.1/)
  (PDF; fetched and confirmed to exist and be the official current EPCIS
  2.0.1 standard document, though this session's tooling could not
  extract readable text from the binary PDF directly).
- **Artefacts page (primary, confirmed by direct fetch):**
  [ref.gs1.org/standards/epcis/artefacts](https://ref.gs1.org/standards/epcis/artefacts)
  lists, with exact filenames: browsable ontology docs at
  `ref.gs1.org/epcis/` and `ref.gs1.org/cbv/`; machine-readable
  `epcis-ontology.jsonld` / `epcis-ontology.ttl` (EPCIS classes/properties,
  RDF Turtle and JSON-LD); `cbv-ontology.jsonld` / `cbv-ontology.ttl` (CBV
  code lists/values); `epcis-context.jsonld` (JSON→JSON-LD/RDF conversion
  context); `epcis-shacl.ttl` (SHACL validation shapes); plus XSD schemas
  for the legacy XML interface and an OpenAPI/WSDL interface definition.
  **This confirms GS1 does publish an actual RDF/OWL-ish (Turtle/JSON-LD)
  ontology for EPCIS and CBV** — not merely an XML schema.
- **Core event types** (from search of multiple sources, consistent with
  standard EPCIS documentation but not independently re-verified against
  readable primary PDF text in this session): **ObjectEvent** (what was
  observed, where, when), **AggregationEvent** (packing/unpacking into
  containers), **TransactionEvent** (linking objects to business
  transactions), **TransformationEvent** (e.g. cutting fabric into cut
  pieces, or sewing pieces into a finished garment — inputs transformed
  into outputs). Each event is structured around "what, when, where, why"
  dimensions. Flagged: this paragraph's detail is secondary-sourced
  (vendor explainer pages), since the official PDF text could not be
  extracted; the *existence and filenames* of the EPCIS/CBV ontology
  artefacts above, however, are primary-confirmed.
- **CBV (Core Business Vocabulary):** Companion standard defining the
  controlled vocabulary values (business steps, dispositions, event
  types) used inside EPCIS events. Current version 2.0.0, ratified June
  2022, confirmed on
  [ref.gs1.org/standards/cbv](https://ref.gs1.org/standards/cbv) (archive
  of all versions at [ref.gs1.org/standards/cbv/archive](https://ref.gs1.org/standards/cbv/archive)).
  CBV 2.0 introduced the `ref.gs1.org/cbv` namespace specifically "to
  underpin CBV 2.0 support for Linked Data," per the ISO/IEC 19988:2024
  adoption text. Also published as **ISO/IEC 19988**
  ([standards.iteh.ai/catalog/standards/iso/.../iso-iec-19988-2015](https://standards.iteh.ai/catalog/standards/iso/03958d35-a2d0-4737-8baa-d717154beba1/iso-iec-19988-2015)).

**GS1 Web Vocabulary:**
- **Primary source:** [ref.gs1.org/voc/](https://ref.gs1.org/voc/), the
  current ratified version (confirmed live via fetch). Development
  repository: [github.com/gs1/WebVoc](https://github.com/gs1/WebVoc).
- **What it is:** An RDF Schema/OWL vocabulary that extends **schema.org**
  — confirmed as "the first schema.org external extension" per schema.org's
  own blog announcement
  ([blog.schema.org/2016/02/22/gs1-web-vocabulary-welcoming-the-first-schema-org-external-extension/](https://blog.schema.org/2016/02/22/gs1-web-vocabulary-welcoming-the-first-schema-org-external-extension/)).
  Its initial focus (per the ref.gs1.org/voc page itself) is **consumer-facing
  properties for clothing, shoes, food, beverage and tobacco**, plus
  properties common to all trade items; it extends schema.org's
  `Product`/`Offer`/`Organization` classes with GS1-sourced detail. The page
  offers RDF Turtle and JSON-LD downloads and uses `owl:versionInfo` in its
  template, consistent with (though the page doesn't explicitly label
  itself) an OWL vocabulary. A specific textile-domain term,
  **"Textile Material"** (`gs1.org/voc/TextileMaterialDetails`), was found
  via search snippet, though the live page returned HTTP 403 to direct
  fetch in this session — **flagged: term's existence corroborated by
  search index, not by a successful direct fetch of its page text.**
  Licensed Apache 2.0.
- **Scope clarity:** This is explicitly a **consumer-facing / e-commerce
  product-page vocabulary** (designed so retailers' web pages can be
  marked up for search engines), not a factory production/traceability
  model — it complements EPCIS/CBV (supply-chain events) rather than
  overlapping with ISA-95-style MOM concepts.

**Verdict: directly reusable (for specific slices).** The EPCIS/CBV
Turtle+JSON-LD ontology files (`epcis-ontology.ttl`, `cbv-ontology.ttl`) are
real, current, GS1-published RDF artifacts and are the single best
off-the-shelf fit for this project's shipment/traceability-event layer
(cutting → sewing → QC → packing → shipment, as a chain of
Object/Transformation/Aggregation events). The GS1 Web Vocabulary is
reusable only for product-catalog-facing concepts (GTIN, variant, textile
material attributes) if/when the ontology touches customer-facing product
data — not for internal production-floor modeling.

---

## 4. Existing Apparel/Textile Domain Ontologies

Searched: "apparel ontology OWL," "textile ontology RDF," "garment
manufacturing knowledge graph ontology," plus a direct check of Linked Open
Vocabularies (LOV).

- **LOV check:** Fetched the LOV vocabulary listing
  ([lov.linkeddata.es/dataset/lov/vocabs](https://lov.linkeddata.es/dataset/lov/vocabs))
  and attempted the LOV search API; the listing has 919 vocabularies across
  62 pages and this session could only directly inspect the first page
  (no apparel/textile/fashion/garment match on that page), and the LOV
  search-API endpoint attempts in this session returned 404s rather than
  usable JSON. **Flagged: LOV coverage was not exhaustively confirmed —
  absence here is "not found in the portion checked," not a certified
  "LOV has zero apparel vocabularies."**
- **Clothing Product Information (CPI) ontology** — the clearest RDF/OWL
  candidate found: hosted at `ebusiness-unibw.org/ontologies/cpi/ns`
  (archived/mirrored copy viewed at
  [akswnc7.informatik.uni-leipzig.de .../ontologies--cpi--ns/.../2020.06.10-214448/](https://akswnc7.informatik.uni-leipzig.de/dstreitmatter/archivo/ebusiness-unibw.org/ontologies--cpi--ns/2020.06.10-214448/ontologies--cpi--ns_type%3DgeneratedDocu.html)
  — this is an **ontology archive mirror**, a legitimate way to view a
  live RDF vocabulary's generated documentation, counted here as a
  primary-adjacent artifact since it's the actual served ontology
  document, archived). Scope: **product-specific clothing data for
  e-commerce/retail catalogs** (material composition, care instructions),
  designed to be compatible with and complement schema.org — i.e. for
  *selling* garments, not *producing* them.
- **VetiVoc** — modular ontology "for the fashion, textile and clothing
  domain," covering fibers through footwear/headwear, described in
  Pianta/et al., *Applied Ontology* journal, 2015/2016
  ([journals.sagepub.com/doi/abs/10.3233/AO-150149](https://journals.sagepub.com/doi/abs/10.3233/AO-150149)).
  This is a **paper describing the ontology**; this session did not locate
  a separately hosted, downloadable ontology file/namespace independent of
  the paper, so **flagged: described in an academic paper, linked artifact
  not confirmed in this search.**
- **SAREF4WEAR** — an official **ETSI SAREF** extension ontology for
  wearable devices, primary source:
  [saref.etsi.org/saref4wear/](https://saref.etsi.org/saref4wear/) (version
  1.1.1 at [saref.etsi.org/saref4wear/v1.1.1](https://saref.etsi.org/saref4wear/v1.1.1)
  — a real, versioned, ETSI-published OWL-DL ontology, directly fetched/
  confirmed). Scope: **wearable IoT devices**, including a
  `TextileBasedWearable` class for "wearable devices integrated into
  textile products directly" — this is about smart-garment electronics,
  not garment manufacturing/production processes.
- **Fashionpedia** — a fashion *image-segmentation taxonomy* tied to a
  computer-vision dataset ([arxiv.org/pdf/2004.12276](https://arxiv.org/pdf/2004.12276)),
  with garment/part/attribute categories and synset IDs; no RDF/OWL
  serialization was found — this is a CV label taxonomy, not a semantic-web
  ontology.
- **Europeana Fashion Thesaurus** / **SILKNOW thesaurus** — SKOS-based
  multilingual *thesauri* of fashion/textile terminology for cultural-
  heritage/museum cataloguing (e.g.
  [skosmos.silknow.org](https://skosmos.silknow.org/thesaurus/en/page/699)),
  not OWL ontologies and not aimed at factory production at all.

**Verdict: useful as inspiration only (narrow applicability).** Every
real, fetchable apparel/textile RDF artifact found (CPI, SAREF4WEAR,
GS1 Web Vocabulary's textile terms) is scoped to **retail product
description or wearable-device IoT**, explicitly *not* factory production
processes (cutting, sewing lines, WIP, work orders). No published OWL
ontology for garment-factory *manufacturing operations* was found anywhere
in this search. This confirms the premise of the user's project: the
production-floor side of a garment-factory ontology is a genuine gap worth
building from scratch.

---

## 5. Common Garment-Factory Business Entities (context only)

*Not independently verified against a primary source — this section is
general domain knowledge, included only to frame what the from-scratch
ontology will likely need to model.*

- **Order / Style / SKU:** A buyer places an **Order** for one or more
  **Styles** (a garment design); each Style explodes into **SKUs** by
  size/color combination — the unit that actually gets cut, sewn, and
  shipped.
- **Bill of Materials (BOM):** Per style, the list of fabrics, trims,
  and quantities-per-garment needed to produce it (e.g. 1.2m main fabric,
  2 buttons, 1 zipper) — drives purchasing and costing.
- **Fabric / Trim:** Raw materials; fabric usually tracked by roll/lot
  with width and composition, trims (buttons, zippers, labels) tracked by
  supplier lot.
- **Cutting:** The process step where fabric rolls are cut (often via
  marker/lay planning) into cut pieces for a specific style/size/quantity
  — the first production stage with real WIP.
- **Sewing line:** A line of workstations/operators performing sequential
  sewing operations to assemble cut pieces into a finished garment; often
  tracked by line efficiency and output per hour.
- **Work order:** An internal instruction to produce a specific quantity
  of a specific SKU by a certain date, usually derived from a customer
  Order and assigned to a specific line/shift.
- **Quality control / defect codes:** Inspection checkpoints (in-line,
  end-line, final) that record pass/fail and standardized defect codes
  (e.g. broken stitch, skip stitch, fabric stain) against a garment or
  batch.
- **Work-in-progress (WIP) tracking:** The count/location of
  partially-completed garments as they move between cutting, sewing,
  finishing, and packing — critical for floor visibility and bottleneck
  analysis.
- **Warehouse / inventory:** Raw-material inventory (fabric/trim stock)
  and finished-goods inventory (packed cartons awaiting shipment), usually
  tracked by location/bin and lot/batch.
- **Shipment:** The outbound logistics event — cartons/pallets packed
  against a specific order, with ship date, carrier, and often an ASN
  (Advance Ship Notice) to the buyer.
- **Customer / buyer:** The brand or retailer placing Orders — typically
  carries its own style codes, labeling requirements, and QC/compliance
  standards layered on top of the factory's internal process.

---

## Recommendation

Reuse is realistically limited to the **traceability/event layer**: GS1's
EPCIS + CBV Turtle/JSON-LD ontology files
([ref.gs1.org/standards/epcis/artefacts](https://ref.gs1.org/standards/epcis/artefacts))
are a genuinely official, current, machine-readable RDF vocabulary and are
a strong fit for modeling the cut→sew→QC→pack→ship event chain as
Object/Aggregation/Transformation events, and the **GS1 Web Vocabulary**
(`ref.gs1.org/voc`) is worth aligning with narrowly for product-catalog
concepts (GTIN, size/color variant) if the ontology ever faces outward to
buyers or e-commerce. **W3C SSN/SOSA** is worth adopting as-is if/when
sensors or machine telemetry get modeled. Everything else — ISA‑95's object
model, MASON, OntoCAPE, and the various apparel/textile ontologies found
(CPI, VetiVoc, SAREF4WEAR, Fashionpedia) — should be treated as **reading
material and naming inspiration only**: none has a maintained, freely
reusable OWL artifact that actually matches "garment factory production
operations," so the core of this project (Order/Style/SKU, BOM, cutting,
sewing lines, work orders, QC/defects, WIP, warehouse) is a legitimate
greenfield modeling exercise, best designed from scratch using ISA‑95's
Personnel/Equipment/Material/Process-Segment vocabulary as a conceptual
checklist rather than an import target.
