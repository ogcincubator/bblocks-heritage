# Route

A road, visitor path, internal track or circulation route within or between heritage zones,
typed as **CIDOC-CRM E26 Physical Feature** — a subclass of E53 Place that represents a
spatially bounded physical feature of the landscape or built environment rather than a
manufactured object. Routes are used for access management, visitor navigation, traffic
analysis and infrastructure documentation at heritage sites.

This block profiles `ogc.heritage.place`, inheriting JSON-FG geometry encoding, coordinate
reference system support and core CRM attributes (`identifier`, `name`, `partOf`, `sameAs`),
and adds route-specific fields for type, surface, dimensions, accessibility and site linkage.

## Semantic model

| JSON property | RDF predicate | Note |
|---|---|---|
| `identifier` (inherited) | `crm:P1_is_identified_by` | Local inventory or GIS identifier |
| `name` (inherited) | `crm:P1_is_identified_by` (appellation) | Display name of the route |
| `routeType` | `crm:P2_has_type` | Getty AAT URI classifying the route (required) |
| `accessibilityRating` | `crm:P44_has_condition` | Accessibility classification string |
| `parentSite` | `crm:P89_falls_within` | URI of the containing heritage site |
| `partOf` (inherited) | `crm:P89_falls_within` | URI of a broader place or zone |
| `surface` | *(unmapped)* | Surface material — JSON payload only |
| `width` | *(unmapped)* | Path width in metres — JSON payload only |
| `permittedUse` | *(unmapped)* | Permitted use classification — JSON payload only |
| geometry (JSON-FG) | `geojson:geometry` | Centreline polyline or bounding polygon (required) |

`surface`, `width` and `permittedUse` have no direct CIDOC-CRM predicate; they are retained
as JSON-only operational attributes for GIS and access-management use.

## SHACL constraints

Shapes target subjects of `crm:P2_has_type` (i.e. any node carrying a `routeType` triple),
consistent with the JSON-FG profiling pattern used across the place-profile family. Required
field validation (routeType, geometry) is handled by JSON Schema; SHACL adds IRI-kind checks
on the link properties.

## Design notes

- E26 Physical Feature is a superclass of E25 Man-Made Feature and E27 Site in CIDOC-CRM.
  Using E26 directly is appropriate for a route, which may span built and natural surfaces
  and is not itself a manufactured artefact or a designated site.
- Geometry is inherited as a mandatory JSON-FG `geometry` field (centreline as LineString
  recommended; Polygon acceptable for wide tracks with a meaningful boundary extent).
- `parentSite` maps to `crm:P89_falls_within`, the same predicate as the inherited `partOf`
  field. Use `parentSite` when the semantic intent is "this route lies within/between heritage
  sites"; use `partOf` for nesting within a sub-zone or garden sector.

## Use cases

- **CRRS-017 (Reggia di Venaria):** Visitor paths, service tracks and access roads within the
  palace garden and park, linked to the Reggia di Venaria heritage site and typed via Getty AAT.
- **General:** Internal circulation routes at any heritage site; visitor trails connecting
  separate buildings or garden zones; service access roads for conservation vehicles.
