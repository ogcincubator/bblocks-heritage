## Monitoring Point

A `MonitoringPoint` is a physical sensor or monitoring device installed at a heritage resource
for environmental and condition monitoring. It profiles
[`ogc.sosa.properties.sensor`](https://www.w3.org/TR/vocab-ssn/#SOSASensor) — the SOSA/SSN
Sensor entity representing a device that can generate observations pertaining to an observable
property — and adds a CIDOC-CRM **E22 Man-Made Object** identity layer.

The `type` field is set to `"MonitoringPoint"`, which the JSON-LD context maps to
`crm:E22_Man-Made_Object`, enabling SHACL targeting by class.

Properties added by this block:

| Property | CRM mapping | Notes |
|---|---|---|
| `type` (required) | `crm:E22_Man-Made_Object` | Fixed const; enables `sh:targetClass` |
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
