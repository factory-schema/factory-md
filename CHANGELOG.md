# Changelog

All notable changes to the Factory.md specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html). Versions in the `0.x` series are pre-1.0 beta releases; minor version bumps MAY include breaking changes while we iterate toward 1.0.

## [0.3.0] - 2026-05-12

Initial public beta of Factory.md.

### Added

- Two-layer document format: YAML frontmatter (validated by JSON Schema) plus a free-form Markdown body for rich descriptions
- Minimal frontmatter (12 fields): `schema`, `name`, `location`, `vertical`, `capabilities`, `certifications`, `website`, `email`, `updated_at`, `has_inventory`, `has_rfq`, `has_agent_capabilities`. Required: `schema`, `name`, `location`, `vertical`
- `certifications` is a flat array of strings (detailed cert metadata lives in the Markdown body)
- Indexing signal flags (`has_inventory`, `has_rfq`, `has_agent_capabilities`) so consumers can filter without fetching the body
- v0.3 JSON Schema at `schema/v0.3/factory.schema.json` (Draft 2020-12, validates frontmatter only)
- Discovery via the well-known URI `/.well-known/factory.md` ([RFC 8615](https://www.rfc-editor.org/rfc/rfc8615))
- Recommended Markdown body sections with RFC 2119 language (Capabilities, Certifications, Quality, Engineering, Shipping & Logistics, Compliance, RFQ Requirements, About)
- Three example profiles across manufacturing verticals (CNC machine shop, PCB fabricator, textile factory)
- A2A (Agent-to-Agent) protocol interoperability: extension URI `https://factoryschema.org/a2a-extension/v1`, semantic mapping, industrial task extension for RFQs, lean Agent Card guidance
- Reference A2A Agent Card example (`examples/agent-card.json`)
- Non-normative Appendix A with common vertical and capability values
- YAML parsing security guidance (quote country codes, certification types)
