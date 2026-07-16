# Monitoring Threshold

A **Monitoring Threshold** records a conservation threshold rule: a bounded acceptable value range
for a specific environmental parameter at a heritage monitoring point or zone. Threshold records are
linked to `monitoring-point` or `heritage-site` instances and drive conservation alerts when sensor
readings breach the defined limits.

## Semantic anchor

The block is modelled as a **SOSA ObservableProperty** (`sosa:ObservableProperty`), representing the
parameterised environmental quantity together with its acceptable bounds and severity classification.
QUDT unit URIs express the unit of measurement; CIDOC-CRM predicates record type classification,
severity and spatial linkage.

## Property table

| Property | Required | CRM / standard mapping | Description |
|---|---|---|---|
| `id` | no | `@id` | Persistent URI for the threshold rule |
| `type` | yes | `@type` → `sosa:ObservableProperty` | Fixed token `MonitoringThreshold` |
| `parameter` | yes | `sosa:observes` (@id) | URI of the observable parameter (GEOSS ECV or QUDT QuantityKind) |
| `thresholdType` | yes | `crm:P2_has_type` | Constraint kind: `minValue`, `maxValue`, `range`, `rateOfChange` |
| `minValue` | cond. | *(unmapped — JSON payload)* | Lower bound; required when `thresholdType` is `minValue` or `range` |
| `maxValue` | cond. | *(unmapped — JSON payload)* | Upper bound; required when `thresholdType` is `maxValue` or `range` |
| `rateValue` | cond. | *(unmapped — JSON payload)* | Rate limit; required when `thresholdType` is `rateOfChange` |
| `unit` | no | `qudt:unit` (@id) | QUDT unit URI (e.g. `qudt:unit/DEG_C`, `qudt:unit/PERCENT`) |
| `severity` | yes | `crm:P44_has_condition` | Alert level: `advisory`, `warning`, `alarm`, `critical` |
| `appliesTo` | yes | `crm:P53_has_former_or_current_location` (@id) | URI of the monitoring point or heritage zone |
| `activePeriod` | no | `crm:P4_has_time-span` (@json) | `{ "start": "YYYY-MM-DD", "end": "YYYY-MM-DD" }` |
| `source` | yes | `dct:source` | Standard, protocol or expert opinion URI or label |
| `rationale` | no | `crm:P3_has_note` | Free-text justification for the threshold value |
| `reviewDate` | no | `dct:modified` (`xsd:date`) | Scheduled review date (ISO 8601) |

## Design notes

- `minValue`, `maxValue` and `rateValue` are left unmapped in the JSON-LD context: they carry
  numeric payload that has no suitable CRM predicate and are validated by JSON Schema alone.
- `thresholdType` uses `crm:P2_has_type` with a literal value; this is a pragmatic simplification
  (CRM strictly expects an `E55 Type` object) that keeps the JSON flat.
- `sh:targetClass sosa:ObservableProperty` is safe: no other block in the register instantiates
  that class.
- `activePeriod` is mapped as `@type: "@json"` to keep the start/end pair opaque in RDF.

## Standard alignments

- **SOSA/SSN** (● mandatory): `sosa:ObservableProperty` as the semantic anchor; `sosa:observes`
  links the threshold to the measured parameter.
- **QUDT** (● mandatory): `qudt:unit` for the physical unit; parameter URIs from QUDT QuantityKind.
- **GEOSS Essential Climate Variables**: preferred URI set for the `parameter` field.
- **CIDOC-CRM**: P2 type classification, P44 condition/severity, P53 spatial linkage, P4 time-span,
  P3 note/rationale.
- **DCTerms**: `dct:source` for protocol provenance; `dct:modified` for review date.
- **schema.org ActionAccessSpecification** (○ optional): can be used as an extended rule-semantics
  overlay alongside this block where rule-engine integration is required.

## Use cases

- Reggia di Venaria (CRRS-011): define RH and temperature thresholds per gallery zone, linked to
  `monitoring-point` instances installed in the Galleria Grande.
- Villa Portelli Malta: define temperature and light-level thresholds for gallery rooms, with
  seasonal `activePeriod` for winter/summer profiles.
