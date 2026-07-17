
# Building (Schema)

`ogc.heritage.building` *v0.1*

A principal building or architectural unit as a CIDOC-CRM E22 Man-Made Object with spatial footprint and optional IFC/BIM reference. Profile of heritage-object.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

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

## Examples

### Galleria Grande, Reggia di Venaria (REQ-002)
The Galleria Grande as a building: linked to the Venaria heritage site, construction period from Garove to Juvara, footprint polygon, and Getty AAT building type. Covers REQ-002 (principal building / architectural unit at Venaria).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/building/galleria-grande",
  "type": "HeritageObject",
  "identifier": "RV-BLD-GG",
  "title": "Galleria Grande, Reggia di Venaria Reale",
  "description": "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara.",
  "objectType": "http://vocab.getty.edu/aat/300007733",
  "parentSite": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "constructionPeriod": "1699/1733",
  "currentStatus": "in use",
  "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.627, 45.134],
        [7.630, 45.134],
        [7.630, 45.136],
        [7.627, 45.136],
        [7.627, 45.134]
      ]
    ]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/building/galleria-grande",
  "type": "HeritageObject",
  "identifier": "RV-BLD-GG",
  "title": "Galleria Grande, Reggia di Venaria Reale",
  "description": "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara.",
  "objectType": "http://vocab.getty.edu/aat/300007733",
  "parentSite": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "constructionPeriod": "1699/1733",
  "currentStatus": "in use",
  "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.627,
          45.134
        ],
        [
          7.63,
          45.134
        ],
        [
          7.63,
          45.136
        ],
        [
          7.627,
          45.136
        ],
        [
          7.627,
          45.134
        ]
      ]
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise-eccch.eu/resource/building/galleria-grande> a crm:E22_Man-Made_Object ;
    dct:created "1699/1733" ;
    crm:P102_has_title "Galleria Grande, Reggia di Venaria Reale" ;
    crm:P1_is_identified_by "RV-BLD-GG" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300007733> ;
    crm:P3_has_note "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara." ;
    crm:P44_has_condition "in use" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/site/reggia-di-venaria> ;
    crm:P50_has_current_keeper "Consorzio delle Residenze Reali Sabaude" ;
    geojson:geometry "{\"coordinates\":[[[7.627,45.134],[7.63,45.134],[7.63,45.136],[7.627,45.136],[7.627,45.134]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```


### Villa Portelli main building with IFC reference (MT-01)
The main villa building at Villa Portelli, Malta: linked to the garden heritage site, under-restoration status, and an optional IFC Global ID for HBIM cross-reference. Covers MT-01 (Villa Portelli main building). Note — Gaussian splat 3D representation would be a linked digital-representation record with dct:format pointing to the appropriate media type, not an attribute on the building itself.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "type": "HeritageObject",
  "identifier": "MT-BLD-VP-01",
  "title": "Villa Portelli — Main Villa Building",
  "description": "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta.",
  "objectType": "http://vocab.getty.edu/aat/300005433",
  "parentSite": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
  "constructionPeriod": "18th century",
  "currentStatus": "under restoration",
  "responsibleOrganisation": "Heritage Malta",
  "ifcGlobalId": "2WrR4Z9bT8RvKsJMlNpQxA",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [14.513, 35.896],
        [14.515, 35.896],
        [14.515, 35.897],
        [14.513, 35.897],
        [14.513, 35.896]
      ]
    ]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "type": "HeritageObject",
  "identifier": "MT-BLD-VP-01",
  "title": "Villa Portelli \u2014 Main Villa Building",
  "description": "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta.",
  "objectType": "http://vocab.getty.edu/aat/300005433",
  "parentSite": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
  "constructionPeriod": "18th century",
  "currentStatus": "under restoration",
  "responsibleOrganisation": "Heritage Malta",
  "ifcGlobalId": "2WrR4Z9bT8RvKsJMlNpQxA",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          14.513,
          35.896
        ],
        [
          14.515,
          35.896
        ],
        [
          14.515,
          35.897
        ],
        [
          14.513,
          35.897
        ],
        [
          14.513,
          35.896
        ]
      ]
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise-eccch.eu/resource/building/villa-portelli-main> a crm:E22_Man-Made_Object ;
    dct:created "18th century" ;
    crm:P102_has_title "Villa Portelli — Main Villa Building" ;
    crm:P1_is_identified_by "2WrR4Z9bT8RvKsJMlNpQxA",
        "MT-BLD-VP-01" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300005433> ;
    crm:P3_has_note "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta." ;
    crm:P44_has_condition "under restoration" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/site/villa-portelli-garden> ;
    crm:P50_has_current_keeper "Heritage Malta" ;
    geojson:geometry "{\"coordinates\":[[[14.513,35.896],[14.515,35.896],[14.515,35.897],[14.513,35.897],[14.513,35.896]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Building
description: 'A principal building or architectural unit (palace wing, chapel, outbuilding,
  villa)

  modelled as a CIDOC-CRM E22 Man-Made Object. Profiles ogc.heritage.heritage-object
  and

  adds site containment, construction history, operational status, optional IFC cross-reference

  and an optional GeoJSON footprint geometry.


  Instances have type "HeritageObject" (inherited); the building category is conveyed
  through

  objectType pointing to a Getty AAT architectural concept.

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.yaml
- type: object
  oneOf:
  - type: object
    description: Building with its own embedded footprint geometry (or none at all).
    not:
      required:
      - references
  - $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/ogc-utils/topology/schema.yaml
  properties:
    parentSite:
      type: string
      format: uri
      description: URI of the heritage-site this building belongs to (crm:P46i_forms_part_of).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P46i_forms_part_of
      x-jsonld-type: '@id'
    constructionPeriod:
      type: string
      description: Date or date range of original construction, ISO 8601 or free text
        (e.g. "1675-1690"). Maps to dct:created.
      x-jsonld-id: http://purl.org/dc/terms/created
    currentStatus:
      type: string
      description: Operational or conservation status (e.g. "in use", "under restoration",
        "closed"). Maps to crm:P44_has_condition.
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
    responsibleOrganisation:
      type: string
      description: Name or URI of the body responsible for managing this building
        (crm:P50_has_current_keeper).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P50_has_current_keeper
    ifcGlobalId:
      type: string
      description: IFC Global ID (GUID) of the corresponding IfcBuilding element in
        an HBIM model, when available (crm:P1_is_identified_by).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    footprint:
      type: object
      description: Optional GeoJSON geometry representing the building's ground-level
        footprint or spatial extent.
      properties:
        type:
          type: string
          enum:
          - Point
          - Polygon
          - MultiPolygon
        coordinates:
          type: array
      required:
      - type
      - coordinates
      x-jsonld-id: https://purl.org/geojson/vocab#geometry
      x-jsonld-type: '@json'
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/
  geojson: https://purl.org/geojson/vocab#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritageObject": "crm:E22_Man-Made_Object",
    "parentSpace": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "movementHistory": {
      "@id": "prov:wasUsedBy",
      "@type": "@id",
      "@container": "@set"
    },
    "id": "@id",
    "type": "@type",
    "identifier": "crm:P1_is_identified_by",
    "title": "crm:P102_has_title",
    "description": "crm:P3_has_note",
    "objectType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "material": {
      "@id": "crm:P45_consists_of",
      "@type": "@id",
      "@container": "@set"
    },
    "currentLocation": {
      "@id": "crm:P53_has_former_or_current_location",
      "@type": "@id"
    },
    "persistentIdentifier": {
      "@id": "crm:P1_is_identified_by",
      "@type": "@id"
    },
    "LineString": "geojson:LineString",
    "references": {
      "@id": "geojson:relatedFeatures",
      "@type": "@id",
      "@container": "@list"
    },
    "parentSite": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "constructionPeriod": "dct:created",
    "currentStatus": "crm:P44_has_condition",
    "responsibleOrganisation": "crm:P50_has_current_keeper",
    "ifcGlobalId": "crm:P1_is_identified_by",
    "footprint": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "prov": "http://www.w3.org/ns/prov#",
    "dct": "http://purl.org/dc/terms/",
    "geojson": "https://purl.org/geojson/vocab#",
    "csdm": "https://linked.data.gov.au/def/csdm/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/context.jsonld)

## Sources

* [CIDOC-CRM E22 Man-Made Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E22)
* [HERITALISE D8.2 REQ-002](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/building`

