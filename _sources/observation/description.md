## Observation

An `Observation` is a scientific measurement or environmental reading taken of a heritage
object or place — microclimate monitoring, condition assessment, conservation science — using
the W3C SOSA/SSN vocabulary as its primary standard.

This block profiles
[`ogc.sosa.properties.observation`](https://opengeospatial.github.io/ogcapi-sosa/bblock/ogc.sosa.properties.observation),
inheriting `observedProperty`, `madeBySensor`, `hasFeatureOfInterest`,
`hasResult`/`hasSimpleResult`, `resultTime`, and `phenomenonTime`. The only additions are:

- `type` is fixed to `"Observation"` (mapped to `sosa:Observation` in the context) so that
  uplifted RDF is properly typed — the profiled block's own schema does not set this.
- `hasFeatureOfInterest` is tightened from optional to **required**: every reading in this
  register is expected to link back to the `heritage-object` or `place` it was taken of or at.

This narrowing reflects the CIDOC-CRM/CRMsci *scientific observation context*: the observation
is meaningful only in relation to the heritage resource being studied or monitored.

### Sensor linkage

This register profiles the W3C SOSA/SSN vocabulary directly rather than the OGC SensorThings API
(STA is an API protocol built on SOSA/SSN, not a separate model — no confirmed partner requirement
for STA API endpoints specifically). `madeBySensor` should reference an
[`ogc.heritage.monitoring-point`](../monitoring-point) record (a SOSA/SSN `Sensor`);
`hasFeatureOfInterest` should reference the [`place`](../place) or
[`heritage-object`](../heritage-object) the reading was taken of/at.

For observations that need an independent geometry (a sensor not co-located with any existing
`place`), wrap the properties from this block inside `ogc.sosa.features.observation` instead.

### Use cases

- **Profile A** — environmental monitoring (temperature, humidity, light) at Venaria and Malta
- **REQ-010** — sensor reading linked to a `monitoring-point` via `hasFeatureOfInterest`

## HDTO alignment

Every instance requires `crmsciType`, a fixed `const` of `crmsci:S4`, mapping via `context.jsonld`
directly to `rdf:type` — asserted on a plain JSON-LD parse, no post-processing step needed. Per
D7.1's own OGC SensorThings API crosswalk table (Table 1, p.28), an STA `Observation` (the same
concept SOSA's `sosa:Observation`, which this block profiles, already models) targets
`crmsci:S4`. D7.1 cites S4 only as a superclass reference (its own full scope note is a gap in the
source — see `eccch-integration/hdto/07-referenced-crmsci-crmdig-crminf.md`), but it is at least a
class D7.1's own declarations actually use, unlike `crmsci:S9` (see `monitoring-threshold`'s own
HDTO alignment note for why that one uses `crmsci:S15` instead).
