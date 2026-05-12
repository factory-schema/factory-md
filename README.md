# Factory.md

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Spec License: CC BY 4.0](https://img.shields.io/badge/Spec-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![Version](https://img.shields.io/badge/spec-v0.3%20(beta)-orange.svg)](SPEC.md)

**Factory.md** is an open standard (v0.3, beta) that makes manufacturing facilities machine-readable. A single Markdown file with YAML frontmatter describes a factory's capabilities, certifications, and endpoints — readable by humans, AI agents, and procurement platforms alike. Structured frontmatter for discovery and filtering, rich Markdown body for everything else.

## Minimal Example

```markdown
---
schema: "https://factoryschema.org/v0.3/factory.schema.json"
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
has_agent_capabilities: true
---

# Midwest Sheet Metal Supply

Sheet metal supply and cut-to-size service for fabricators across the Midwest.

## Capabilities

Cold-rolled and hot-rolled steel, aluminum, stainless. Shearing to ±0.5 mm, slitting to ±0.25 mm.
```

Four fields are required: `schema`, `name`, `location`, and `vertical`. Everything else — capabilities, certifications, contact, last-updated date, and the `has_*` capability flags — is recommended. The Markdown body contains rich descriptions of quality processes, equipment, tolerances, compliance, and more.

## Quick Start

1. **Create** a `factory.md` file using the [minimal example](#minimal-example) above
2. **Describe** your factory with recommended frontmatter fields and Markdown body sections
3. **Validate** the frontmatter against the [JSON Schema](schema/v0.3/factory.schema.json)
4. **Host** it at `/.well-known/factory.md` on your domain ([RFC 8615](https://www.rfc-editor.org/rfc/rfc8615))

```
https://yourfactory.com/.well-known/factory.md
```

This well-known URI makes your factory discoverable by AI agents and platforms without any prior knowledge of your site structure.

## Status

Factory.md is in **beta**. The format is stable enough for early adoption, but minor version bumps in the `0.x` series MAY include breaking changes as we iterate toward 1.0. Pin the `schema` field to a specific version URI so consumers can detect format changes deterministically.

## Documentation

| Document | Description |
|----------|-------------|
| [SPEC.md](SPEC.md) | Formal specification with field reference and recommended body sections (RFC 2119 language) |
| [schema/v0.3/factory.schema.json](schema/v0.3/factory.schema.json) | JSON Schema v0.3 (Draft 2020-12) — validates YAML frontmatter |
| [examples/](examples/) | Real-world example profiles across manufacturing verticals |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to propose changes |
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [GOVERNANCE.md](GOVERNANCE.md) | Decision-making process |

## Examples

- [CNC Machine Shop](examples/factory-machine-shop.md) — Aerospace-grade precision machining in Shenzhen
- [PCB Fabricator](examples/factory-pcb-fab.md) — Multilayer and HDI board fabrication in Taoyuan
- [Textile Factory](examples/factory-textile.md) — Organic cotton knit garments in Tirupur
- [Agent Card](examples/agent-card.json) — A2A Agent Card for the machine shop example

## License

- **Schema and code**: [Apache License 2.0](LICENSE)
- **Specification document** (SPEC.md): [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)

## Links

- Schema: `https://factoryschema.org/v0.3/factory.schema.json`
- Website: [factoryjson.org](https://factoryjson.org)
