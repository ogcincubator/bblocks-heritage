
# Building (Schema)

`ogc.heritage.building` *v0.1*

A principal building or architectural unit as a CIDOC-CRM E22 Man-Made Object with spatial footprint and optional IFC/BIM reference. Profile of heritage-object.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Building

A `Building` is a principal building or architectural unit — a palace wing, chapel,
outbuilding, or villa — modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object-feature`](../heritage-object-feature), inheriting the inventory
identifier, title, object type, materials and provenance fields (nested under `properties`),
and adds site containment, construction history, operational status, and an optional IFC GUID
for HBIM cross-referencing.

The building category is conveyed through `properties.objectType` pointing to a Getty AAT
architectural concept (e.g. `aat:300007733` *palace*). `properties.choType` is fixed to
`"HeritageObject"` from the parent (a second alias to `@type`, alongside the fixed
`type: "Feature"`); overriding it would make the schema unsatisfiable.

Properties added by this block (all nested under `properties`):

| Property | CRM / standard mapping | Notes |
|---|---|---|
| `parentSite` | `crm:P46i_forms_part_of` (@id) | URI of the containing `heritage-site` |
| `constructionPeriod` | `dct:created` | ISO 8601 date/range or free text |
| `currentStatus` | `crm:P44_has_condition` | Operational/conservation status string |
| `responsibleOrganisation` | `crm:P50_has_current_keeper` | Managing body name or URI |
| `ifcGlobalId` | `crm:P1_is_identified_by` | IFC IfcBuilding GUID for HBIM linkage |

Geometry is inherited from `heritage-object-feature`: a top-level `geometry` (GeoJSON
Polygon/MultiPolygon ground plan, or omitted entirely), or `geometry: null` with a `topology`
reference into a shared/topological geometry (`ogc.geo.topo.features.topo-feature`).

### Design notes

The top-level `geometry` holds a **generalised** ground-level geometry for mapping and spatial
queries. Detailed BIM geometry lives in the IFC model, linked via `ifcGlobalId`.

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
  "type": "Feature",
  "geometry": {
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
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-BLD-GG",
    "title": "Galleria Grande, Reggia di Venaria Reale",
    "description": "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara.",
    "objectType": "http://vocab.getty.edu/aat/300007733",
    "parentSite": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
    "constructionPeriod": "1699/1733",
    "currentStatus": "in use",
    "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/building/galleria-grande",
  "type": "Feature",
  "geometry": {
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
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-BLD-GG",
    "title": "Galleria Grande, Reggia di Venaria Reale",
    "description": "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara.",
    "objectType": "http://vocab.getty.edu/aat/300007733",
    "parentSite": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
    "constructionPeriod": "1699/1733",
    "currentStatus": "in use",
    "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/building/galleria-grande> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    dct:created "1699/1733" ;
    crm:P102_has_title "Galleria Grande, Reggia di Venaria Reale" ;
    crm:P1_is_identified_by "RV-BLD-GG" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300007733> ;
    crm:P3_has_note "The main ceremonial gallery of the Reggia di Venaria, designed by Michelangelo Garove and completed by Filippo Juvara." ;
    crm:P44_has_condition "in use" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/site/reggia-di-venaria> ;
    crm:P50_has_current_keeper "Consorzio delle Residenze Reali Sabaude" ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 7.627e+00 4.5134e+01 ) ( 7.63e+00 4.5134e+01 ) ( 7.63e+00 4.5136e+01 ) ( 7.627e+00 4.5136e+01 ) ( 7.627e+00 4.5134e+01 ) ) ) ] .


```


### Villa Portelli main building with IFC reference (MT-01)
The main villa building at Villa Portelli, Malta: linked to the garden heritage site, under-restoration status, and an optional IFC Global ID for HBIM cross-reference. Covers MT-01 (Villa Portelli main building). Note — Gaussian splat 3D representation would be a linked digital-representation record with dct:format pointing to the appropriate media type, not an attribute on the building itself.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "type": "Feature",
  "geometry": {
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
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "MT-BLD-VP-01",
    "title": "Villa Portelli — Main Villa Building",
    "description": "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta.",
    "objectType": "http://vocab.getty.edu/aat/300005433",
    "parentSite": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
    "constructionPeriod": "18th century",
    "currentStatus": "under restoration",
    "responsibleOrganisation": "Heritage Malta",
    "ifcGlobalId": "2WrR4Z9bT8RvKsJMlNpQxA"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "type": "Feature",
  "geometry": {
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
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "MT-BLD-VP-01",
    "title": "Villa Portelli \u2014 Main Villa Building",
    "description": "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta.",
    "objectType": "http://vocab.getty.edu/aat/300005433",
    "parentSite": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
    "constructionPeriod": "18th century",
    "currentStatus": "under restoration",
    "responsibleOrganisation": "Heritage Malta",
    "ifcGlobalId": "2WrR4Z9bT8RvKsJMlNpQxA"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/building/villa-portelli-main> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    dct:created "18th century" ;
    crm:P102_has_title "Villa Portelli — Main Villa Building" ;
    crm:P1_is_identified_by "2WrR4Z9bT8RvKsJMlNpQxA",
        "MT-BLD-VP-01" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300005433> ;
    crm:P3_has_note "The principal residential building of Villa Portelli, a historic Baroque-era villa in Malta." ;
    crm:P44_has_condition "under restoration" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/site/villa-portelli-garden> ;
    crm:P50_has_current_keeper "Heritage Malta" ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 1.4513e+01 3.5896e+01 ) ( 1.4515e+01 3.5896e+01 ) ( 1.4515e+01 3.5897e+01 ) ( 1.4513e+01 3.5897e+01 ) ( 1.4513e+01 3.5896e+01 ) ) ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Building
description: 'A principal building or architectural unit (palace wing, chapel, outbuilding,
  villa)

  modelled as a CIDOC-CRM E22 Man-Made Object. Profiles ogc.heritage.heritage-object-feature

  and adds site containment, construction history, operational status and an optional
  IFC

  cross-reference. Geometry (footprint) is inherited from heritage-object-feature:
  embedded

  directly, given by reference/topology, or omitted entirely.


  Instances carry `properties.choType: "HeritageObject"` (inherited); the building
  category

  is conveyed through `objectType` pointing to a Getty AAT architectural concept.

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/schema.yaml
- type: object
  properties:
    properties:
      type: object
      properties:
        parentSite:
          type: string
          format: uri
          description: URI of the heritage-site this building belongs to (crm:P46i_forms_part_of).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P46i_forms_part_of
          x-jsonld-type: '@id'
        constructionPeriod:
          type: string
          description: Date or date range of original construction, ISO 8601 or free
            text (e.g. "1675-1690"). Maps to dct:created.
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
          description: IFC Global ID (GUID) of the corresponding IfcBuilding element
            in an HBIM model, when available (crm:P1_is_identified_by).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/building/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "Feature": "geojson:Feature",
    "FeatureCollection": "geojson:FeatureCollection",
    "GeometryCollection": "geojson:GeometryCollection",
    "LineString": "geojson:LineString",
    "MultiLineString": "geojson:MultiLineString",
    "MultiPoint": "geojson:MultiPoint",
    "MultiPolygon": "geojson:MultiPolygon",
    "Point": "geojson:Point",
    "Polygon": "geojson:Polygon",
    "features": {
      "@container": "@set",
      "@id": "geojson:features"
    },
    "type": "@type",
    "id": "@id",
    "properties": "@nest",
    "geometry": "geojson:geometry",
    "bbox": {
      "@container": "@list",
      "@id": "geojson:bbox"
    },
    "links": {
      "@context": {
        "href": {
          "@type": "@id",
          "@id": "oa:hasTarget"
        },
        "rel": {
          "@context": {
            "@base": "http://www.iana.org/assignments/relation/"
          },
          "@id": "http://www.iana.org/assignments/relation",
          "@type": "@id"
        },
        "type": "dct:type",
        "hreflang": "dct:language",
        "title": "rdfs:label",
        "length": "dct:extent"
      },
      "@id": "rdfs:seeAlso"
    },
    "featureType": "@type",
    "time": {
      "@context": {
        "date": {
          "@id": "owlTime:hasTime",
          "@type": "xsd:date"
        },
        "timestamp": {
          "@id": "owlTime:hasTime",
          "@type": "xsd:dateTime"
        },
        "interval": {
          "@id": "owlTime:hasTime",
          "@container": "@list"
        }
      },
      "@id": "dct:time"
    },
    "coordRefSys": "http://www.opengis.net/def/glossary/term/CoordinateReferenceSystemCRS",
    "place": "dct:spatial",
    "Polyhedron": "geojson:Polyhedron",
    "MultiPolyhedron": "geojson:MultiPolyhedron",
    "Prism": {
      "@id": "geojson:Prism",
      "@context": {
        "base": "geojson:prismBase",
        "lower": "geojson:prismLower",
        "upper": "geojson:prismUpper"
      }
    },
    "MultiPrism": {
      "@id": "geojson:MultiPrism",
      "@context": {
        "prisms": "geojson:prisms"
      }
    },
    "coordinates": {
      "@container": "@list",
      "@id": "geojson:coordinates"
    },
    "geometries": {
      "@id": "geojson:geometry",
      "@container": "@list"
    },
    "topology": {
      "@context": {
        "references": {
          "@id": "topo:relatedFeatures",
          "@type": "@id",
          "@container": "@list"
        },
        "directed_references": {
          "@context": {
            "ref": {
              "@type": "@id",
              "@id": "topo:ref"
            }
          },
          "@id": "topo:directedReferences",
          "@container": "@list"
        },
        "relationships": {
          "@context": {
            "href": {
              "@type": "@id",
              "@id": "oa:hasTarget"
            },
            "rel": {
              "@context": {
                "@base": "http://www.iana.org/assignments/relation/"
              },
              "@id": "http://www.iana.org/assignments/relation",
              "@type": "@id"
            },
            "type": "dct:type",
            "hreflang": "dct:language",
            "title": "rdfs:label",
            "length": "dct:extent",
            "role": {
              "@id": "prof:hasRole",
              "@type": "@id"
            },
            "conformsTo": {
              "@id": "dct:conformsTo",
              "@type": "@id"
            }
          },
          "@id": "topo:relatedFeatures",
          "@type": "@id",
          "@container": "@list"
        }
      },
      "@type": "@id",
      "@id": "geojson:topology"
    },
    "HeritageObject": "crm:E22_Man-Made_Object",
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
    "parentSite": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "constructionPeriod": "dct:created",
    "currentStatus": "crm:P44_has_condition",
    "responsibleOrganisation": "crm:P50_has_current_keeper",
    "ifcGlobalId": "crm:P1_is_identified_by",
    "Arc": "geojson:Arc",
    "ArcWithCenter": "geojson:ArcWithCenter",
    "ArcByChord": "geojson:ArcByChord",
    "CircleByCenter": "geojson:CircleByCenter",
    "CubicSpline": "geojson:CubicSpline",
    "radius": "geojson:radius",
    "arcLength": "geojson:arcLength",
    "startTangentVector": "geojson:startTangentVector",
    "endTangentVector": "geojson:endTangentVector",
    "ref": "topo:ref",
    "orientation": "topo:orientation",
    "Edge": "topo:Edge",
    "Face": "topo:Face",
    "Ring": "topo:Ring",
    "Shell": "topo:Shell",
    "Solid": "topo:Solid",
    "rings": {
      "@id": "topo:rings",
      "@container": "@list"
    },
    "shells": {
      "@id": "topo:shells",
      "@container": "@list"
    },
    "faces": {
      "@id": "topo:faces",
      "@container": "@list"
    },
    "parentSpace": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "movementHistory": {
      "@id": "prov:wasUsedBy",
      "@type": "@id",
      "@container": "@set"
    },
    "geojson": "https://purl.org/geojson/vocab#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "oa": "http://www.w3.org/ns/oa#",
    "dct": "http://purl.org/dc/terms/",
    "owlTime": "http://www.w3.org/2006/time#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "topo": "https://purl.org/geojson/topo#",
    "prof": "http://www.w3.org/ns/dx/prof/",
    "prov": "http://www.w3.org/ns/prov#",
    "choType": "@type",
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

