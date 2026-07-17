## Condition Assessment

A `ConditionAssessment` records a qualitative conservation inspection of a heritage resource
at a specific point in time. It is anchored to **CRMsci E14 Condition Assessment**, which
specialises CRM E13 Attribute Assignment for the specific task of evaluating the physical
state of a heritage object or place.

This block is distinct from [`ogc.heritage.observation`](../observation), which covers
continuous sensor-based measurements (CRMsci E16 Measurement / SOSA Observation). A condition
assessment is a human-led inspection event that produces a qualitative finding (E3 Condition
State), not a numeric sensor reading.

Encoded as a GeoJSON/JSON-FG Feature: CIDOC-CRM/CRMsci attributes nest under `properties`, and
`properties.choType` carries the CRMsci class as a second alias to `@type`, alongside the fixed
`type: "Feature"`. Localisation is optional — `geometry` may be `null` (no spatial
localisation), an embedded GeoJSON geometry, or replaced by a `topology` reference
(`ogc.geo.topo.features.topo-feature`) into geometry already recorded on the affected object.

### Properties (all nested under `properties`)

| Property | CRM / PROV mapping | Required | Notes |
|---|---|---|---|
| `choType` | `sci:E14_Condition_Assessment` (via second `@type` alias) | yes | Fixed const; enables `sh:targetClass` |
| `affectedObject` | `crm:P140_assigned_attribute_to` (@id) | yes | URI of the `heritage-object`, `architectural-space` or `place` being assessed |
| `conditionState` | `crm:P35_has_identified` (@json) | yes | E3 Condition State sub-object: `conditionType` (Getty AAT URI), `severity` (minor/moderate/severe/critical), `extent` (text) |
| `assessmentDate` | `crm:P4_has_time-span` (xsd:date) | yes | ISO 8601 date |
| `assessor` | `prov:wasAttributedTo` (@id) | no | URI of agent (person or organisation) |
| `method` | `crm:P33_used_specific_technique` | no | Inspection technique label or URI |
| `confidence` | *(unmapped)* | no | low / medium / high |
| `evidence` | `crm:P16_used_specific_object` (@id, set) | no | URIs of supporting `digital-representation` or `archival-document` records |
| `recommendation` | `crm:P3_has_note` | no | Free-text conservation recommendation |
| `reviewStatus` | *(unmapped)* | no | Workflow status string |

`confidence` and `reviewStatus` are workflow/operational fields with no natural CRM predicate;
they are retained in JSON but not mapped to RDF.

`conditionState` is treated as an opaque JSON value (`@type: "@json"`) rather than a blank
node, keeping the graph simple while preserving the structured payload for JSON consumers.

### Use cases

- **REQ-006** — Localised deterioration, damage, moisture or cracking recorded by Venaria
  conservators (pathology-area layer of the MHS)
- Any pilot partner performing periodic visual or instrument-assisted inspections of heritage
  objects, surfaces or spaces
