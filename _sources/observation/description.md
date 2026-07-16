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

### Integration with SensorThings API

D8.2 profile A mandates OGC SensorThings API (STA) as the operational monitoring API. The
`madeBySensor` property should reference an `ogc.api.sta.Sensor` resource; the
`hasFeatureOfInterest` should reference the `ogc.api.sta.FeatureOfInterest` or, for the
heritage domain link, a [`place`](../place) or [`heritage-object`](../heritage-object) URI.
Actual sensor readings (values, timestamps, units) live in STA `Observation` resources; this
block is the CRM-anchored representation linking them back to the heritage graph.

For observations that need an independent geometry (a sensor not co-located with any existing
`place`), wrap the properties from this block inside `ogc.sosa.features.observation` instead.

### Use cases

- **Profile A** — environmental monitoring (temperature, humidity, light) at Venaria and Malta
- **REQ-010** — sensor reading linked to a `monitoring-point` via `hasFeatureOfInterest`
