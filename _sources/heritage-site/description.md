## Heritage Site

A `HeritageSite` is a designated cultural heritage site or pilot study area, modelled as
CIDOC-CRM **E27 Site** — a subclass of E53 Place that carries the semantic notion of a
purposefully established or recognised location (a palace complex, garden, protected
landscape, archaeological zone).

This block profiles [`ogc.heritage.place`](../place): a `HeritageSite` *is* a JSON-FG
Feature. Geometry (a bounding polygon or representative point) stays at the top level under
`geometry`; CIDOC-CRM attributes are nested under `properties`. The same JSON-FG consequence
applies: `type` is fixed to `"Feature"`, so the uplifted RDF graph carries no explicit E27
class triple — SHACL shapes target by `sh:targetSubjectsOf crm:P2_has_type`.

Properties added by this block (beyond those inherited from `place`):

| Property | CRM mapping | Notes |
|---|---|---|
| `siteType` (required) | `crm:P2_has_type` (@id) | Controlled vocabulary URI, typically Getty AAT |
| `designation` | `crm:P1_is_identified_by` | Formal designation code or label (e.g. UNESCO list number) |
| `responsibleOrganisation` | `crm:P50_has_current_keeper` | Name or URI of the managing body |
| `rights` | `dct:rights` | Access/reuse rights URI (Creative Commons, rightsstatements.org) |

### Design note

Sub-zones within a site (garden partitions, protected buffer zones) are modelled as
[`ogc.heritage.place`](../place) records linked back to the site via `partOf`
(`crm:P89_falls_within`), not as nested `HeritageSite` records. The site itself is the
top-level boundary; the place hierarchy handles subdivision.

### Use cases

- **REQ-001** — Reggia di Venaria Reale overall pilot boundary (UNESCO WHC no. 823)
- **MT-03** — Villa Portelli garden as a named national monument (Heritage Malta)
- **REQ-018** — Venaria garden as a site-level entity (sub-zone linkage via `place`)
