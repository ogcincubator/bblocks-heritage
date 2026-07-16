## Architectural Space

An `ArchitecturalSpace` is an interior room, bay, hall, corridor, vault, opening or zone
within a building, modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object`](../heritage-object) and adds a containment hierarchy,
historical name crosswalk, floor/bay identifiers, an optional IFC IfcSpace reference, and an
optional GeoJSON footprint for the floor plan or volumetric extent.

The space category is conveyed through `objectType` pointing to a Getty AAT spatial concept
(e.g. `aat:300004829` *room*). The `type` field is fixed to `"HeritageObject"` from the parent.

Properties added by this block:

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
| `footprint` | `geojson:geometry` (@json) | GeoJSON Polygon floor plan or volumetric extent |

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
