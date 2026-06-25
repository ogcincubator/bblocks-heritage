## Place

A `Place` is a spatial location relevant to cultural heritage — a site, building, room or
landscape feature — modelled as a CIDOC-CRM `E53_Place`.

Rather than reinventing a geometry encoding, this block profiles
[`ogc.geo.json-fg.feature-lenient`](https://opengeospatial.github.io/bblocks/register/bblock/ogc.geo.json-fg.feature-lenient):
a `Place` *is* an OGC JSON-FG Feature. Geometry stays at the top level (`geometry`, and
optionally `place`/`time` for non-WGS84 CRS or temporal extent), while CIDOC-CRM attributes
(`identifier`, `name`, `placeType`, `partOf`, `sameAs`) are nested under `properties`, per
JSON-FG/GeoJSON convention.

One consequence of reusing JSON-FG: the `type` member is fixed to the literal `"Feature"` (a
JSON-FG/GeoJSON requirement), so the uplifted RDF graph does not carry an explicit
`crm:E53_Place` class triple — the SHACL shapes here target nodes by the CIDOC-CRM predicates
they use (`sh:targetSubjectsOf`) rather than by `sh:targetClass`.

`Place` hierarchies are expressed with `partOf` (CIDOC-CRM `P89_falls_within`), e.g. a room
within a building within a site. Other blocks reference a `Place` by its `id`:

- [`heritage-object`](../heritage-object)'s `currentLocation` points to a `Place`
- a SensorThings API `Datastream`'s `FeatureOfInterest` can point to a `Place` for monitoring
  (D8.2 profile A)
- GeoDCAT/Records dataset metadata can use a `Place`'s geometry for spatial extent
