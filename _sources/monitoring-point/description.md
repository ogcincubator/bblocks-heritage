## Monitoring Point

A `MonitoringPoint` is a physical sensor or monitoring device installed at a heritage resource
for environmental and condition monitoring. It profiles
[`ogc.sosa.properties.sensor`](https://www.w3.org/TR/vocab-ssn/#SOSASensor) — the SOSA/SSN
Sensor entity representing a device that can generate observations pertaining to an observable
property — and adds a CIDOC-CRM **E22 Man-Made Object** identity layer.

The `type` field is set to `"MonitoringPoint"`, which the JSON-LD context maps to
`crmdig:D8_Digital_Device`, enabling SHACL targeting by class.

### HDTO correction (2026-09-16)

Previously this block mapped `type: "MonitoringPoint"` to `crm:E22_Man-Made_Object`, noted at
the time as a workaround since SOSA Sensor has no native `type` property. **This was corrected**
per HDTO's own OGC SensorThings API -> HDTO/CRM crosswalk table (D7.1 Table 1, p.28), which gives
an explicit target for each STA entity:

| STA entity | HDTO/CRM target |
|---|---|
| Thing | HC1 / crm:E70 Thing |
| Location | crm:E53 Place |
| ObservedProperty | crmsci:S9 / S15 |
| **Sensor** | **crmdig:D8 Digital Device** |
| Datastream | crm:E73 Information Object |
| Observation | crmsci:S4 |
| FeatureOfInterest | crm:E55 Type / S15 / E70 |

`MonitoringPoint` corresponds to STA's Sensor entity, so it now targets `crmdig:D8_Digital_Device`
instead of `crm:E22_Man-Made_Object` — a breaking change to the SHACL `sh:targetClass` (all
existing examples were re-validated against the new target; both still pass). The `crmdig:`
namespace used is `http://www.cidoc-crm.org/extensions/crmdig/` (CRMdig v5.0, per HDTO's own live
ontology page), matching the register-wide namespace decision recorded in `PLAN.md`'s "HDTO
alignment" section.

**Caveat inherited from the source material and preserved here deliberately:** `D8 Digital
Device` is *not* itself formally declared anywhere in D7.1 §5.5's own CRMdig class list (only D1,
D2, D9, D11 are) — it is cited only informally in this crosswalk table. This mapping is therefore
sourced from the upstream CRMdig v5.0 specification directly, not from an HDTO-internal class
declaration; treat it as directional guidance from HDTO rather than a class with its own
HDTO-confirmed scope note.

Properties added by this block:

| Property | CRM mapping | Notes |
|---|---|---|
| `type` (required) | `crmdig:D8_Digital_Device` | Fixed const; enables `sh:targetClass` |
| `identifier` (required) | `crm:P1_is_identified_by` | Local pilot inventory code |
| `sensorType` (required) | `crm:P2_has_type` (@id) | Getty AAT sensor/equipment category URI |
| `locatedIn` (required) | `crm:P53_has_former_or_current_location` (@id) | URI of containing `architectural-space`, `heritage-object`, or `place` |
| `position` (required) | `geojson:geometry` (@json) | GeoJSON Point; use 3D coordinates where known |
| `operationalStatus` | `crm:P44_has_condition` | "active", "decommissioned", "under maintenance" |
| `responsibleOrganisation` | `crm:P50_has_current_keeper` | Operating body name or URI |

SOSA properties inherited from `ogc.sosa.properties.sensor` (e.g. `sosa:observes`,
`sosa:isHostedBy`, `sosa:madeObservation`) may be used alongside these CRM properties.

### Observation integration

Individual sensor readings are represented using the [`ogc.heritage.observation`](../observation)
block (SOSA/SSN Observation). The `locatedIn` property on this block provides the static CRM
location anchor. `sosa:hasFeatureOfInterest` on the observation should reference the heritage
resource being monitored (the `architectural-space`, `heritage-object`, or `place` identified
in `locatedIn`), not the sensor itself.

### Use cases

- **REQ-009** — Physical sensor positions and MHS hotspots at Reggia di Venaria (microclimate
  monitoring in the Galleria Grande and other principal spaces)
- **REQ-010** — Anchor for `ogc.heritage.observation` records: `hasFeatureOfInterest` on an
  observation can reference the `locatedIn` resource from this block
