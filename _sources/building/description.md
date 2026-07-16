## Building

A `Building` is a principal building or architectural unit — a palace wing, chapel,
outbuilding, or villa — modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object`](../heritage-object), inheriting the inventory identifier,
title, object type, materials, and provenance fields, and adds site containment, construction
history, operational status, an optional GeoJSON footprint, and an optional IFC GUID for
HBIM cross-referencing.

The building category is conveyed through `objectType` pointing to a Getty AAT architectural
concept (e.g. `aat:300007733` *palace*). The `type` field is fixed to `"HeritageObject"` from
the parent; overriding it would make the schema unsatisfiable.

Properties added by this block:

| Property | CRM / standard mapping | Notes |
|---|---|---|
| `parentSite` | `crm:P46i_forms_part_of` (@id) | URI of the containing `heritage-site` |
| `constructionPeriod` | `dct:created` | ISO 8601 date/range or free text |
| `currentStatus` | `crm:P44_has_condition` | Operational/conservation status string |
| `responsibleOrganisation` | `crm:P50_has_current_keeper` | Managing body name or URI |
| `ifcGlobalId` | `crm:P1_is_identified_by` | IFC IfcBuilding GUID for HBIM linkage |
| `footprint` | `geojson:geometry` (@json) | GeoJSON Polygon/MultiPolygon ground plan |

### Design notes

`footprint` holds a **generalised** ground-level geometry for mapping and spatial queries.
Detailed BIM geometry lives in the IFC model, linked via `ifcGlobalId`. Coordinates are stored
as `@type: "@json"` to prevent coordinate arrays from being misread as RDF lists.

[`architectural-space`](../architectural-space) records reference a `Building` via their
`parentBuilding` property (`crm:P46i_forms_part_of`).

### Use cases

- **REQ-002** — Galleria Grande and Sant'Uberto chapel at Reggia di Venaria
- **MT-01** — Villa Portelli main villa building with HBIM / Gaussian splat context
