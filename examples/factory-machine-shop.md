---
schema: "https://factoryschema.org/v0.4/factory.schema.json"
name: "Precision Works Manufacturing Co."
location:
  city: Shenzhen
  region: Guangdong
  country: CN
  postal_code: "518000"
  timezone: Asia/Shanghai
  coordinates:
    lat: 22.5431
    lng: 114.0579
vertical: machining
capabilities:
  - 5-axis CNC milling
  - CNC turning
  - wire EDM
  - sinker EDM
  - surface grinding
  - cylindrical grinding
  - jig boring
certifications:
  - "ISO 9001:2015"
  - AS9100D
  - "ISO 13485:2016"
  - ITAR Registered
website: "https://precisionworks.example.com"
email: sales@precisionworks.example.com
updated_at: "2026-05-12"
has_inventory: false
has_rfq: true
skills:
  - id: submit-rfq
    endpoint: https://precisionworks.argo.trade/agent/rfq
    auth: nda
    description: Submit a Request for Quote for CNC-machined parts.
    docs: https://precisionworks.argo.trade/docs/submit-rfq
  - id: quote-status
    endpoint: https://precisionworks.argo.trade/agent/quote-status
    auth: open
    description: Look up the status and quoted price of a previously submitted RFQ.
    docs: https://precisionworks.argo.trade/docs/quote-status
---

# Precision Works Manufacturing Co.

## Summary

High-precision CNC machining facility in Shenzhen specializing in aerospace and medical-grade components. Founded 2008, 120–150 employees, AS9100D and ISO 13485 certified, ITAR registered. We focus on low-to-mid-volume precision work where tight tolerances and full traceability matter.

## Capabilities

5-axis CNC milling, CNC turning, wire EDM, sinker EDM, surface grinding, cylindrical grinding, jig boring.

**Finishes:** anodizing Type II (color), anodizing Type III (hard coat), passivation (citric & nitric), electropolishing, bead blasting, powder coating, electroless nickel plating, chrome plating, black oxide, tumble deburring, vapor polishing (plastics).

**Secondary services:** mechanical assembly, insert installation (helicoil, PEM), heat treatment coordination, laser engraving/marking, custom packaging, kitting.

**Tolerances:**
- Standard: ±0.025 mm (ISO 2768-m); Precision: ±0.002 mm
- By process: 5-axis milling ±0.005 mm, turning ±0.005 mm, wire EDM ±0.003 mm, grinding ±0.002 mm, jig boring ±0.002 mm
- Surface finish: Ra 1.6 µm standard, Ra 0.2 µm best
- Min feature: 0.3 mm internal radius; Min wall: 0.5 mm (aluminum), 0.8 mm (steel)

**Capacity & constraints:** MOQ 1–10,000 pieces. Capacity 25,000 pieces/month. Max envelope 500 × 450 × 400 mm, max part weight 50 kg. Min order value $500 USD.

**Engineering services:** DFM review, prototyping, reverse engineering. Accepted CAD: STEP, IGES, DXF, DWG, Parasolid, SolidWorks (.sldprt), 3MF, PDF. In-house CAD/CAM: Mastercam, SolidWorks, AutoCAD.

## RFQ

- **Endpoint:** https://precisionworks.argo.trade/rfq
- **Required fields:** quantity, material, delivery date
- **Required files:** STEP + PDF drawing with GD&T callouts
- **Accepted types:** machined parts, prototypes, production runs
- **NDA required** before quoting
- **No auto-quote** — all RFQs are reviewed by engineering
- ITAR parts require a separate intake at sales@precisionworks.example.com
- Typical RFQ response time: within 24 hours

## Certifications & Compliance

| Certification | Body | Cert ID | Expires |
|---------------|------|---------|---------|
| ISO 9001:2015 | TUV SUD | 12-100-28456 | 2026-06-14 |
| AS9100D | BSI | FM 12345 | 2026-08-31 |
| ISO 13485:2016 | TUV SUD | 12-100-29001 | 2027-01-09 |
| ITAR Registered | U.S. DDTC | M30456 | — |

**IP protection:** NDA signed before quoting. Isolated production cells for sensitive projects. Access-controlled digital file servers with audit logging.
**Export controls:** ITAR, EAR.
**Environmental:** RoHS and REACH compliant; ISO 14001 in progress.
**Social:** SA8000 certified.

## Quality

100% first article inspection, SPC on production runs, final CMM report included with every shipment. In-process inspection at critical operations. Defect rate: 800 PPM. Full lot traceability from raw material to finished part — each part laser-marked with a serial number linked to inspection data.

**Testing:** CMM inspection, FAI (AS9102), surface roughness measurement, hardness (Rockwell, Vickers), material composition (XRF), thread gauging (go/no-go).

**Inspection equipment:**

| Equipment | Type | Count | Key Specs |
|-----------|------|-------|-----------|
| Zeiss Contura | CMM | 2 | 1.8 µm accuracy, 700×1000×600 mm range |
| Mitutoyo SJ-410 | Surface roughness tester | 3 | — |
| Keyence IM-8000 | Instant measurement system | 1 | — |
| Olympus Vanta XRF | Material analyzer | 1 | — |

**Documentation:** FAI / AS9102, PPAP Level 3, CoC, mill certs / material certifications, dimensional inspection report, surface finish report, FAIR.

## Materials

**Aluminum:** 6061-T6, 7075-T6.
**Steel & stainless:** 304, 316L, 17-4 PH, tool steel A2, tool steel D2.
**Titanium & superalloys:** Ti-6Al-4V, Inconel 718.
**Copper alloys:** brass C360, copper C101.
**Plastics:** PEEK, Delrin/POM.

## Equipment

| Equipment | Type | Count | Key Specs |
|-----------|------|-------|-----------|
| DMG Mori DMU 50 | 5-axis CNC mill | 4 | 500×450×400 mm travel, 20k RPM, 60-tool changer |
| Mazak QTN-200 | CNC lathe | 6 | 300 mm turning dia, live tooling, 5k RPM |
| Sodick ALC600G | Wire EDM | 2 | 600×400×350 mm travel, 0.05 mm min wire |
| Okamoto ACC-6-18DX | Surface grinder | 2 | 150×450 mm table |

## Lead Times

- **Samples / first articles:** 5 calendar days
- **Prototypes (1–25 pcs):** 10–15 calendar days
- **Production runs:** 20–35 calendar days
- **Expedite:** available at +30% for samples and prototypes (subject to capacity)

## Shipping and Logistics

**Incoterms:** EXW, FOB Shenzhen, CIF, DDP.
**Methods:** air freight, sea freight (FCL/LCL), express courier (DHL, FedEx, UPS).
**Markets served:** US, DE, JP, GB, CA, AU, SG, KR.
**Packaging:** vacuum sealed, foam-cushioned, custom crating, VCI anti-corrosion wrap, individual part bags.
**Port of departure:** Yantian Port, Shenzhen.

**Payment terms:** 50% deposit / 50% before shipment; Net 30 (established accounts); 100% prepay (first order).
**Payment methods:** T/T (wire transfer), PayPal, L/C (letter of credit).
**Currencies:** USD, EUR, CNY.

## Agent Access

- **MCP server:** https://precisionworks.argo.trade/.well-known/mcp
- **Auth:** open for capability queries and quote-status lookups; NDA acceptance required before any task that uploads CAD or drawings.

### Submit RFQ

Submit a Request for Quote for CNC-machined parts. The skill validates that required files and fields are present, requires NDA acceptance, then routes the RFQ to engineering for quoting. Returns a `quote_id` the agent can use with the `Quote Status` skill below.

- **Skill ID:** `submit-rfq`
- **Input:** `model/step` (3D CAD, required) + `application/pdf` (drawing with GD&T, required) + JSON metadata: `{ "quantity": int, "material": str, "delivery_date": "YYYY-MM-DD" }`
- **Output:** `application/json` — `{ "quote_id": str, "status": "submitted" | "input-required", "missing": [str] }`
- **Endpoint:** `POST https://precisionworks.argo.trade/agent/rfq`
- **Auth:** NDA acceptance required before file upload. ITAR-controlled parts routed to sales@precisionworks.example.com instead.
- **Example:** "Quote 50 pieces of XYZ.step in Al 6061-T6 with GD&T per the attached PDF, delivery in 5 weeks"

### Quote Status

Look up the status and quoted price of a previously submitted RFQ.

- **Skill ID:** `quote-status`
- **Input:** `application/json` — `{ "quote_id": str }`
- **Output:** `application/json` — `{ "status": "submitted" | "working" | "input-required" | "completed" | "failed", "lead_time_days": int?, "unit_price_usd": number?, "moq": int?, "notes": str? }`
- **Endpoint:** `GET https://precisionworks.argo.trade/agent/quote/{quote_id}`
- **Auth:** open to the submitting party (quote_id acts as a bearer token).
- **Example:** "What's the status of quote Q-2026-04812?"

## Contacts

- **Sales / RFQ:** sales@precisionworks.example.com
- **Engineering / DFM:** engineering@precisionworks.example.com
- **Quality / NCR:** quality@precisionworks.example.com
- **Business hours:** Mon–Fri 08:00–18:00, Sat 08:00–12:00 (Asia/Shanghai)

## About

**Legal name:** Precision Works Manufacturing (Shenzhen) Co., Ltd.
**Industries served:** Aerospace, medical devices, defense, semiconductor equipment, robotics.
**Founded:** 2008.
**Employees:** 120–150.
**Languages:** Chinese (Mandarin, Cantonese), English.
