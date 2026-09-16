## Architectural Space

An `ArchitecturalSpace` is an interior room, bay, hall, corridor, vault, opening or zone
within a building, modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object-feature`](../heritage-object-feature) and adds a containment
hierarchy, historical name crosswalk, floor/bay identifiers, and an optional IFC IfcSpace
reference (all nested under `properties`).

The space category is conveyed through `properties.objectType` pointing to a Getty AAT spatial
concept (e.g. `aat:300004829` *room*). `properties.choType` is fixed to `"HeritageObject"` from
the parent (a second alias to `@type`, alongside the fixed `type: "Feature"`).

Properties added by this block (all nested under `properties`):

| Property | CRM mapping | Notes |
|---|---|---|
| `parentBuilding` | `crm:P46i_forms_part_of` (@id) | URI of the containing `building` |
| `partOf` | `crm:P46i_forms_part_of` (@id) | URI of a broader space (multi-level hierarchy) |
| `historicalNames` | `crm:P1_is_identified_by` (@json) | Array of `{name, datePeriod?, source?}` objects |
| `floorLevel` | `crm:P1_is_identified_by` | Floor/storey identifier (e.g. "piano nobile") |
| `bayCode` | `crm:P1_is_identified_by` | Bay/module code from a survey or legacy inventory |
| `accessibilityStatus` | `crm:P44_has_condition` | Open/restricted/closed status string |
| `monitoringRelevance` | *(unmapped)* | Boolean flag — no CRM predicate; survives in JSON |
| `ifcSpaceRef` | `crm:P1_is_identified_by` | IFC IfcSpace GUID for HBIM linkage |

Geometry is inherited from `heritage-object-feature`: a top-level `geometry` (GeoJSON Polygon
floor plan or volumetric extent, or omitted entirely), or `geometry: null` with a `topology`
reference into a shared/topological geometry (`ogc.geo.topo.features.topo-feature`).

### Design notes

`historicalNames` stores legacy inventory codes (Guarini room numbers, 19th-century archival
labels) as an opaque JSON array (`@type: "@json"`), avoiding blank-node complexity while
preserving the full structured record for display and search.

`monitoringRelevance` is a boolean operational flag with no natural CRM predicate; it is
intentionally left unmapped in the JSON-LD context so it survives in the JSON payload without
polluting the RDF graph. [`monitoring-point`](../monitoring-point) records reference the space
they are deployed in.

### Use cases

- **REQ-003** — Galleria Grande bays as monitoring and historical location anchors (Venaria)
- **MT-02** — Interior rooms of Villa Portelli with dynamic attributes and oral history links

## HDTO alignment

`properties.hdtoType` (fixed `const` `hdto:HC3_Tangible_Heritage_Entity`) is inherited, and
therefore required, from [`heritage-object`](../heritage-object) via
[`heritage-object-feature`](../heritage-object-feature) — JSON Schema `allOf` composition means a
profile cannot selectively opt out of a requirement its parent block declares. This is a
**structural default, not a deliberate OGC classification decision**: whether an
`architectural-space` instance (a room, bay, or zone) is independently "the" heritage entity, or
just a component of its containing `building`, is still the partner (CRRS/Venaria) judgment call
noted above and in the integration plan — it isn't resolved by this default applying, and should
be revisited once that input arrives, potentially by overriding `hdtoType` per-instance or
narrowing this block's own parent profile if the answer turns out to be "no."

**Pending question, data-dependent:** this isn't just a one-time partner opinion to collect -- the
right answer plausibly depends on what real Venaria/Malta pilot data actually looks like once it
arrives (e.g. whether rooms are consistently catalogued/valued as standalone records in CRRS's own
systems, or only ever referenced through their containing building). Treat this as open until real
pilot data lands, not just until a partner gives a one-off answer in the abstract.
