# Factory.md Specification v0.4

## Abstract

This document defines **Factory.md**, a human- and machine-readable format for describing manufacturing facilities. A factory.md file is a Markdown document with YAML frontmatter. The frontmatter contains a small set of structured fields for discovery and indexing. The Markdown body contains rich, free-form descriptions of the facility's capabilities, quality processes, engineering support, compliance posture, and other details.

The format is designed to be hosted at a well-known URI, enabling automated discovery by AI agents, procurement platforms, and search engines. The Markdown body is directly consumable by large language models without parsing, while the YAML frontmatter supports programmatic filtering and search indexing.

## Status

This is version **0.4** of the Factory.md specification, published by Factory Schema. Factory.md is currently in **beta**: the format is implementable and stable enough for early adoption, but minor version bumps in the `0.x` series MAY introduce breaking changes as we iterate toward 1.0. See [CHANGELOG.md](CHANGELOG.md) for release history.

## Table of Contents

- [1. Terminology](#1-terminology)
- [2. Discovery Mechanism](#2-discovery-mechanism)
- [3. Document Structure](#3-document-structure)
- [4. Frontmatter Fields](#4-frontmatter-fields)
- [5. Markdown Body](#5-markdown-body)
- [6. Media Type](#6-media-type)
- [7. Versioning Policy](#7-versioning-policy)
- [8. Security Considerations](#8-security-considerations)
- [9. Examples](#9-examples)
- [Appendix A. Common Values](#appendix-a-common-values-non-normative)

---

## 1. Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

Additional terms:

- **factory.md file**: A Markdown document with YAML frontmatter conforming to this specification.
- **Frontmatter**: The YAML block delimited by `---` at the beginning of a factory.md file. Validated against the Factory.md JSON Schema.
- **Body**: The Markdown content following the frontmatter closing `---` delimiter.
- **Publisher**: An entity that creates and hosts a factory.md file.
- **Consumer**: A software agent, platform, or service that reads and processes a factory.md file.

## 2. Discovery Mechanism

### 2.1 Well-Known URI

A factory.md file SHOULD be hosted at the following well-known URI on the publisher's domain, per [RFC 8615](https://www.rfc-editor.org/rfc/rfc8615):

```
https://{domain}/.well-known/factory.md
```

### 2.2 Transport

The file MUST be served over HTTPS. Consumers SHOULD NOT fetch factory.md files over plain HTTP.

### 2.3 Content-Type

The server MUST serve the file with a `Content-Type` header of `text/markdown; charset=utf-8`.

### 2.4 Caching

Publishers SHOULD set appropriate `Cache-Control` headers. A `max-age` of 86400 (24 hours) is RECOMMENDED for production files.

### 2.5 Alternative Locations

A factory.md file MAY also be hosted at other paths (e.g., `/factory.md`, `/about/factory.md`). However, consumers SHOULD check `/.well-known/factory.md` first.

## 3. Document Structure

A factory.md file consists of two parts:

1. **YAML frontmatter** — a structured block delimited by `---` at the start of the file. The opening `---` MUST be the first line. The closing `---` MUST appear on its own line. The frontmatter is validated against the JSON Schema (Draft 2020-12) located at:

   ```
   https://factoryschema.org/v0.4/factory.schema.json
   ```

2. **Markdown body** — free-form content following the frontmatter. The body uses standard Markdown (CommonMark) and SHOULD use the recommended sections described in [Section 5](#5-markdown-body). The body is NOT validated by the schema.

### 3.1 Required Fields

A valid factory.md file MUST include the following frontmatter fields:

| Field | Type | Description |
|-------|------|-------------|
| `schema` | `string` | Schema URI for validation (MUST equal the v0.4 URI) |
| `name` | `string` | Trade name of the factory (MUST be non-empty) |
| `location` | `string` or `object` | Factory location |
| `vertical` | `string` | Primary industry vertical (MUST be non-empty) |

### 3.2 Recommended Fields

The following frontmatter fields are RECOMMENDED:

| Field | Type | Description |
|-------|------|-------------|
| `capabilities` | `string[]` | Manufacturing processes offered |
| `certifications` | `string[]` | Industry certifications |
| `website` | `string` | Factory website URL |
| `email` | `string` | Contact email address |
| `updated_at` | `string` (date) | Date the profile was last updated (YYYY-MM-DD) |
| `has_inventory` | `boolean` | True if the factory exposes inventory data |
| `has_rfq` | `boolean` | True if the factory accepts RFQ submissions |
| `skills` | `object[]` | Agent-callable skill endpoints exposed by the factory |

### 3.3 Design Rationale

The frontmatter contains the fields needed for programmatic discovery, filtering, indexing, and skill invocation. Rich, descriptive content — equipment details, quality processes, tolerances, compliance narratives, shipping logistics, and detailed skill behavior — belongs in the Markdown body where it is readable by both humans and AI agents without structured parsing.

## 4. Frontmatter Fields

### 4.1 `schema` (REQUIRED)

- **Type**: `string` (const)
- **Value**: MUST be `"https://factoryschema.org/v0.4/factory.schema.json"`.
- **Description**: Identifies the document as a factory.md file and pins it to this schema version.

### 4.2 `name` (REQUIRED)

- **Type**: `string` (minLength: 1)
- **Description**: Trade name of the factory. This is the name commonly used in business, not necessarily the registered legal name.

### 4.3 `location` (REQUIRED)

- **Type**: `string` OR `object`
- **Description**: Factory location.

**String form** (shorthand): A simple string such as `"Shenzhen, CN"`. MUST be non-empty.

**Object form** (structured):

| Sub-field | Type | Required | Description |
|-----------|------|----------|-------------|
| `city` | `string` | Yes | City name |
| `region` | `string` | No | State, province, or region |
| `country` | `string` | Yes | ISO 3166-1 alpha-2 country code (e.g. `"CN"`, `"US"`) |
| `postal_code` | `string` | No | Postal or ZIP code |
| `timezone` | `string` | No | IANA timezone identifier (e.g. `"Asia/Shanghai"`) |
| `coordinates` | `object` | No | `{ "lat": number, "lng": number }` — WGS 84 |

### 4.4 `vertical` (REQUIRED)

- **Type**: `string` (minLength: 1)
- **Description**: Primary industry vertical. Free-form, e.g. `"machining"`, `"pcb"`, `"textiles"`, `"sheet-metal"`, `"injection-molding"`. See [Appendix A](#appendix-a-common-values-non-normative) for common values.

### 4.5 `capabilities`

- **Type**: `array` of `string` (minItems: 1)
- **Description**: Manufacturing processes offered, as free-form strings. See [Appendix A](#appendix-a-common-values-non-normative) for common values. RECOMMENDED.

This field is a flat list of process names for indexing and search. Detailed capability descriptions (materials, equipment, tolerances, finishes) belong in the Markdown body's [Capabilities](#52-capabilities-recommended) section.

### 4.6 `certifications`

- **Type**: `array` of `string`
- **Description**: Industry certifications and compliance credentials, as free-form strings (e.g. `"ISO 9001:2015"`, `"GOTS"`, `"UL Listed"`). RECOMMENDED.

This field is a flat list of certification names for indexing and search. Additional detail (issuing body, certificate ID, issue/expiry dates, verification URL) belongs in the Markdown body's [Certifications & Compliance](#55-certifications--compliance-recommended) section.

### 4.7 `website`

- **Type**: `string` (format: URI)
- **Description**: Factory website URL. RECOMMENDED.

### 4.8 `email`

- **Type**: `string`
- **Description**: Contact email address. RECOMMENDED.

### 4.9 `updated_at`

- **Type**: `string` (format: date, `YYYY-MM-DD`)
- **Description**: Date the profile was last updated. RECOMMENDED — helps consumers detect stale data and prioritize re-fetching.

### 4.10 `has_inventory`

- **Type**: `boolean`
- **Description**: Indicates whether the factory exposes inventory data (e.g. on-hand stock, available materials). When `true`, publishers SHOULD describe the inventory feed in the Markdown body. RECOMMENDED.

### 4.11 `has_rfq`

- **Type**: `boolean`
- **Description**: Indicates whether the factory accepts RFQ (Request for Quote) submissions. When `true`, publishers SHOULD include an RFQ Requirements section in the Markdown body with the submission endpoint and any required fields/files. RECOMMENDED.

### 4.12 `skills`

- **Type**: `array` of `object`
- **Description**: Agent-callable skill endpoints exposed by the factory. RECOMMENDED when the factory offers API, MCP, webhook, or other machine-callable workflows.

Each `skills` item MUST include:

| Sub-field | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | `string` | Yes | Stable kebab-case identifier, e.g. `order-status` |
| `endpoint` | `string` (URI) | Yes | HTTPS endpoint agents can call for this skill |
| `auth` | `string` | Yes | Authentication method — one of `open`, `api-key`, `oauth2`, `mtls`, `nda` |
| `version` | `integer` | No | Skill version (≥ 1). Defaults to `1`; omit unless greater |
| `description` | `string` | No | Short human-readable description of what the skill does |
| `docs` | `string` (URI) | No | Documentation URL for inputs, outputs, examples, rate limits, and error handling |

Recommended form:

```yaml
skills:
  - id: order-status
    endpoint: https://acme.com/skills/order-status
    auth: oauth2
    description: Look up current state and expected ship date for a PO.
    docs: https://skills.argotrade.io/order-status
```

Detailed input/output schemas, examples, rate limits, and operational caveats SHOULD be documented at the `docs` URL or in the Markdown body's [Agent Access](#511-agent-access-recommended-if-skills-is-present) section.

### 4.13 Additional Properties

The frontmatter schema sets `additionalProperties: false`. Fields not listed above MUST NOT appear in the frontmatter. All other information belongs in the Markdown body.

## 5. Markdown Body

The Markdown body MAY contain any of the following sections. These sections are all OPTIONAL and RECOMMENDED — none are validated by the schema. Publishers MAY include additional sections, omit any, or reorder them. The section headings below are conventions; publishers MAY use alternative headings that convey the same meaning.

The factory's name SHOULD be the H1 (`#`) at the top of the body. Each recommended section below uses an H2 (`##`).

### 5.1 Summary (RECOMMENDED)

A short paragraph (1–3 sentences) describing what the factory does, who it serves, and any distinguishing positioning (e.g. precision tier, geography, supply-vs-fabrication model). Consumers and LLMs use this as the headline answer to "what is this factory?"

### 5.2 Capabilities (RECOMMENDED)

Detailed descriptions of manufacturing processes offered, including:

- Process narratives (what each capability listed in the frontmatter actually involves)
- Finishes and post-processing
- Secondary services (assembly, heat treatment, packaging, kitting)
- Tolerances, by process if applicable
- Capacity and MOQ guidance
- Vertical-specific specs (e.g. max PCB layers, GSM range for textiles)

Engineering services that don't fit elsewhere — accepted file formats, DFM review, prototyping, CAD/CAM tooling — SHOULD also go here.

### 5.3 Inventory (RECOMMENDED if `has_inventory` is `true`)

When `has_inventory: true` in the frontmatter, publishers SHOULD describe:

- What is stocked (materials, sizes, grades)
- How inventory is exposed (live feed URL, CSV/JSON endpoint, contact for stock check)
- Update cadence and freshness guarantees

Publishers MAY omit this section if `has_inventory: false`.

### 5.4 RFQ (RECOMMENDED if `has_rfq` is `true`)

When `has_rfq: true` in the frontmatter, publishers SHOULD describe:

- RFQ submission endpoint (URL or email)
- Required fields and files (CAD formats, drawings, specs)
- Accepted part/product types
- NDA requirements before submission
- Auto-quote availability and typical response time
- Special instructions or caveats

### 5.5 Certifications & Compliance (RECOMMENDED)

Additional detail for each certification listed in the frontmatter — scope, issuing body, certificate ID, issue/expiry dates, verification URL. Plus broader compliance posture:

- IP protection (NDA policies, access controls, audit logging)
- Export controls (ITAR, EAR, or other regimes)
- Environmental practices and certifications
- Social and labor compliance

### 5.6 Quality (RECOMMENDED)

- QC process and workflow
- Testing methods and equipment
- Inspection equipment (tables are RECOMMENDED)
- Documentation provided with shipments (CoCs, FAI reports, traceability records)
- Defect rates
- Material and process traceability

### 5.7 Materials (RECOMMENDED)

Materials the factory can work with, organized by family if helpful (metals, plastics, composites, textiles). Include grades, standards, and any sourcing notes.

### 5.8 Equipment (RECOMMENDED)

Major equipment with model names, counts, and key specifications. Tables are RECOMMENDED:

```markdown
| Equipment | Type | Count | Key Specs |
|-----------|------|-------|-----------|
| DMG Mori DMU 50 | 5-axis CNC mill | 4 | 500×450×400 mm, 20k RPM |
```

### 5.9 Lead Times (RECOMMENDED)

Typical lead times broken down by order type (sample, prototype, production), with expedite options if available. State the units explicitly (calendar days vs business days).

### 5.10 Shipping and Logistics (OPTIONAL)

- Supported Incoterms
- Shipping methods and carriers
- Markets served (countries/regions)
- Packaging capabilities
- Payment terms, methods, and accepted currencies

### 5.11 Agent Access (RECOMMENDED if `skills` is present)

When `skills` is present in the frontmatter, publishers SHOULD describe how agents can interact with the factory. The frontmatter `skills` array is the machine-readable index of callable skills; this body section provides richer operational detail.

A typical Agent Access section contains:

- Public endpoints (MCP server URI, REST base URL, webhook URL, or other service URI)
- Authentication or NDA preconditions
- One H3 sub-section per skill the factory exposes

#### Skill sub-sections

Each frontmatter skill MAY have a matching level-3 heading (`### Skill Name`) followed by a one-paragraph description and a bullet list of structured fields. The following fields are RECOMMENDED:

| Field | Required | Description |
|-------|----------|-------------|
| `Skill ID` | Yes | Stable, kebab-case identifier (e.g. `rfq-submit`). Used by agents to invoke the skill. |
| `Input` | Yes | What the skill accepts — free-form, but MIME types are RECOMMENDED for file artifacts (e.g. `model/step`, `application/pdf`). |
| `Output` | Yes | What the skill returns. |
| `Endpoint` | Recommended | HTTP method + URI, or a reference to the MCP or service endpoint above if the skill is invoked through it. |
| `Auth` | Recommended | Authentication or NDA requirements. `open` means no auth. |
| `Example` | Optional | A short natural-language prompt or sample request that illustrates the skill in use. |

Publishers MAY add additional fields (e.g. `Rate limit`, `Async`, `Webhook`) as needed.

#### Worked example

```markdown
### Check Stock

Look up real-time on-hand inventory for a specific material grade and form
factor. Returns the current quantity, warehouse, and the timestamp the
inventory snapshot was last refreshed. Backed by the factory's internal ERP —
numbers are authoritative, not estimates.

- **Skill ID:** `check-stock`
- **Input:** `application/json` — `{ "material": "Al 6061-T6", "form": "sheet", "thickness_mm": 3.0, "min_quantity": 10 }`
- **Output:** `application/json` — `{ "on_hand_units": 142, "uom": "sheets (4x8 ft)", "warehouse": "Chicago, IL", "updated_at": "2026-05-12T08:00:00-05:00", "lead_time_if_oos_days": 7 }`
- **Endpoint:** `POST https://example.com/agent/inventory/check`
- **Auth:** API key, issued after NDA. Rate-limited to 60 requests/minute.
- **Example:** "Do you have at least 100 sheets of 3 mm 6061-T6 aluminum in stock today?"
```

### 5.12 Contacts (OPTIONAL)

Human contact points beyond the top-level `email` and `website` frontmatter:

- Sales / RFQ contact
- Engineering / DFM contact
- Quality / NCR contact
- Business hours and response time

### 5.13 About (OPTIONAL)

- Legal name
- Industries served
- Founded year
- Employee count
- Languages spoken
- Ownership / certifications-of-origin where relevant

## 6. Media Type

A factory.md file MUST be served with the media type `text/markdown; charset=utf-8`, per [RFC 7763](https://www.rfc-editor.org/rfc/rfc7763).

Consumers SHOULD identify factory.md files by the presence of YAML frontmatter containing a `name` field and a `location` field, or by the `schema` field with the value `"https://factoryschema.org/v0.4/factory.schema.json"`.

## 7. Versioning Policy

The Factory.md specification follows [Semantic Versioning 2.0.0](https://semver.org/), with the following caveat for the pre-1.0 beta period:

- **Beta (`0.x`)**: Factory.md is currently in beta. Minor version bumps in the `0.x` series (e.g. 0.3 → 0.4) MAY include breaking changes as the format is refined. Patch bumps (e.g. 0.3.0 → 0.3.1) will not.
- **Stable (`1.0`+)**: Once Factory.md reaches 1.0, standard SemVer applies:
  - **Patch** (e.g. 1.0.1): Clarifications, typo fixes, documentation improvements. No schema changes.
  - **Minor** (e.g. 1.1.0): New optional frontmatter fields or new recommended body sections. Existing valid documents remain valid.
  - **Major** (e.g. 2.0.0): Breaking changes. Previously valid documents MAY become invalid.

Publishers SHOULD pin the `schema` field to a specific version URI so consumers can detect format changes deterministically.

## 8. Security Considerations

### 8.1 Sensitive Data

Publishers SHOULD carefully consider what data to include. The factory.md file is intended to be public. Do NOT include:

- Internal pricing or cost structures
- Employee personal information
- Security credentials or API keys
- Confidential customer lists

### 8.2 HTTPS

The file MUST be served over HTTPS to prevent tampering in transit.

### 8.3 Data Accuracy

Consumers SHOULD NOT treat factory.md data as verified or audited. Certifications, capabilities, and other claims are self-reported by the publisher. Consumers requiring verified data SHOULD implement additional validation workflows.

### 8.4 Rate Limiting

Publishers MAY implement rate limiting on the `/.well-known/factory.md` endpoint to prevent excessive crawling.

### 8.5 CORS

Publishers SHOULD set appropriate CORS headers (`Access-Control-Allow-Origin`) if the file is intended to be fetched by browser-based applications.

### 8.6 YAML Parsing

Consumers MUST use a safe YAML parser that does not execute arbitrary code. Publishers SHOULD quote values that could be misinterpreted by YAML parsers — in particular, country codes like `NO` (Norway) and `AT` (Austria) which some parsers interpret as boolean values. Quoting all string values in the frontmatter is RECOMMENDED.

## 9. Examples

### 9.1 Minimal

```markdown
---
schema: "https://factoryschema.org/v0.4/factory.schema.json"
name: "Midwest Sheet Metal Supply"
location: "Chicago, IL, US"
vertical: sheet-metal
---

# Midwest Sheet Metal Supply

Sheet metal supply and cut-to-size service for fabricators across the Midwest.
```

### 9.2 Recommended

```yaml
---
schema: "https://factoryschema.org/v0.4/factory.schema.json"
name: "Midwest Sheet Metal Supply"
location: "Chicago, IL, US"
vertical: sheet-metal
capabilities:
  - Sheet metal supply
  - Cut-to-size blanks
  - Shearing
  - Slitting
certifications:
  - ISO 9001:2015
website: "https://midwestsheet.example.com"
email: sales@midwestsheet.example.com
updated_at: "2026-05-12"
has_inventory: true
has_rfq: true
skills:
  - id: order-status
    endpoint: https://acme.com/skills/order-status
    auth: oauth2
    description: Look up current state and expected ship date for a PO.
    docs: https://skills.argotrade.io/order-status
---
```

### 9.3 Full Examples

See the [examples/](examples/) directory for complete, real-world profiles:

- [factory-machine-shop.md](examples/factory-machine-shop.md) — CNC machine shop (Shenzhen, China)
- [factory-pcb-fab.md](examples/factory-pcb-fab.md) — PCB fabrication facility (Taoyuan, Taiwan)
- [factory-textile.md](examples/factory-textile.md) — Textile/garment manufacturer (Tirupur, India)

---

## Appendix A. Common Values (Non-Normative)

This appendix lists commonly used values for frontmatter fields. These values are **not** a controlled vocabulary — publishers MAY use any value. This list exists to promote convergence and reduce fragmentation.

### Common `vertical` Values

`machining`, `pcb`, `sheet-metal`, `injection-molding`, `die-casting`, `textiles`, `3d-printing`, `electronics-assembly`, `stamping`, `forging`, `extrusion`, `woodworking`, `ceramics`, `glass`, `composites`, `packaging`

### Common `capabilities` Values

**Machining**: `CNC milling`, `5-axis CNC milling`, `CNC turning`, `wire EDM`, `sinker EDM`, `surface grinding`, `cylindrical grinding`, `jig boring`, `Swiss turning`, `gun drilling`

**PCB**: `multilayer PCB fabrication`, `HDI`, `flex-rigid PCB`, `impedance control`, `blind/buried vias`, `heavy copper`, `RF/microwave PCB`

**Sheet Metal**: `sheet metal supply`, `cut-to-size blanks`, `shearing`, `slitting`, `laser cutting`, `CNC punching`, `press brake forming`, `welding (TIG/MIG)`, `spot welding`, `powder coating`, `sheet metal assembly`

**Injection Molding**: `injection molding`, `overmolding`, `insert molding`, `blow molding`, `rotational molding`, `thermoforming`

**Textiles**: `knitting`, `weaving`, `dyeing`, `cut-and-sew`, `embroidery`, `screen printing`, `sublimation printing`, `garment washing`

**Electronics**: `SMT assembly`, `through-hole assembly`, `wave soldering`, `reflow soldering`, `conformal coating`, `box build`, `cable assembly`

---

## References

- [JSON Schema Draft 2020-12](https://json-schema.org/draft/2020-12/json-schema-core)
- [RFC 2119 — Key words for use in RFCs](https://www.rfc-editor.org/rfc/rfc2119)
- [RFC 7763 — The text/markdown Media Type](https://www.rfc-editor.org/rfc/rfc7763)
- [RFC 8615 — Well-Known URIs](https://www.rfc-editor.org/rfc/rfc8615)
- [ISO 3166-1 alpha-2 — Country codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
- [Semantic Versioning 2.0.0](https://semver.org/)
- [CommonMark Specification](https://spec.commonmark.org/)

---

This specification is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The JSON Schema is licensed under [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
