
# Route (Schema)

`ogc.heritage.route` *v0.1*

A road, visitor path, internal track or circulation route within or between heritage zones, typed as CIDOC-CRM E26 Physical Feature. Profiles 'place', inheriting JSON-FG geometry and core CRM place attributes, and adding route type, surface, width, accessibility and permitted-use fields.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

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

## Examples

### Venaria visitor path (CRRS-017)
A gravel visitor path through the gardens of the Reggia di Venaria Reale, classified as a pedestrian thoroughfare (Getty AAT), linked to the Venaria heritage site, with surface, width and accessibility metadata. Covers CRRS-017 (route / visitor path / internal track).
#### json
```json
{
  "id": "https://example.org/heritalise/route/venaria-main-path",
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [7.619, 45.132],
      [7.623, 45.135],
      [7.628, 45.137],
      [7.633, 45.138]
    ]
  },
  "properties": {
    "identifier": "RV-ROUTE-001",
    "name": "Viale centrale dei Giardini",
    "routeType": "http://vocab.getty.edu/aat/300178825",
    "surface": "gravel",
    "width": 4.5,
    "accessibilityRating": "partially accessible",
    "permittedUse": "pedestrian only",
    "parentSite": "https://example.org/heritalise/site/reggia-di-venaria"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/route/context.jsonld",
  "id": "https://example.org/heritalise/route/venaria-main-path",
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [
        7.619,
        45.132
      ],
      [
        7.623,
        45.135
      ],
      [
        7.628,
        45.137
      ],
      [
        7.633,
        45.138
      ]
    ]
  },
  "properties": {
    "identifier": "RV-ROUTE-001",
    "name": "Viale centrale dei Giardini",
    "routeType": "http://vocab.getty.edu/aat/300178825",
    "surface": "gravel",
    "width": 4.5,
    "accessibilityRating": "partially accessible",
    "permittedUse": "pedestrian only",
    "parentSite": "https://example.org/heritalise/site/reggia-di-venaria"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://example.org/heritalise/route/venaria-main-path> a geojson:Feature ;
    crm:P1_is_identified_by "RV-ROUTE-001" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300178825> ;
    crm:P44_has_condition "partially accessible" ;
    crm:P87_is_identified_by "Viale centrale dei Giardini" ;
    crm:P89_falls_within <https://example.org/heritalise/site/reggia-di-venaria> ;
    geojson:geometry [ a geojson:LineString ;
            geojson:coordinates ( ( 7.619e+00 4.5132e+01 ) ( 7.623e+00 4.5135e+01 ) ( 7.628e+00 4.5137e+01 ) ( 7.633e+00 4.5138e+01 ) ) ] .


```


### Villa Portelli garden trail (Malta)
A stone-paved garden trail at Villa Portelli (Malta pilot), classified as a heritage trail (Getty AAT) and linked to the Villa Portelli site. Demonstrates reuse of the route block for a different pilot's circulation infrastructure, with a simpler attribute set (no width recorded).
#### json
```json
{
  "id": "https://example.org/heritalise/route/malta-garden-trail",
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [14.4421, 35.9023],
      [14.4425, 35.9027],
      [14.4430, 35.9031]
    ]
  },
  "properties": {
    "identifier": "MT-ROUTE-001",
    "name": "Villa Portelli Garden Trail",
    "routeType": "http://vocab.getty.edu/aat/300006958",
    "surface": "stone paving",
    "accessibilityRating": "fully accessible",
    "permittedUse": "pedestrian only",
    "parentSite": "https://example.org/heritalise/site/villa-portelli"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/route/context.jsonld",
  "id": "https://example.org/heritalise/route/malta-garden-trail",
  "type": "Feature",
  "geometry": {
    "type": "LineString",
    "coordinates": [
      [
        14.4421,
        35.9023
      ],
      [
        14.4425,
        35.9027
      ],
      [
        14.443,
        35.9031
      ]
    ]
  },
  "properties": {
    "identifier": "MT-ROUTE-001",
    "name": "Villa Portelli Garden Trail",
    "routeType": "http://vocab.getty.edu/aat/300006958",
    "surface": "stone paving",
    "accessibilityRating": "fully accessible",
    "permittedUse": "pedestrian only",
    "parentSite": "https://example.org/heritalise/site/villa-portelli"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://example.org/heritalise/route/malta-garden-trail> a geojson:Feature ;
    crm:P1_is_identified_by "MT-ROUTE-001" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300006958> ;
    crm:P44_has_condition "fully accessible" ;
    crm:P87_is_identified_by "Villa Portelli Garden Trail" ;
    crm:P89_falls_within <https://example.org/heritalise/site/villa-portelli> ;
    geojson:geometry [ a geojson:LineString ;
            geojson:coordinates ( ( 1.44421e+01 3.59023e+01 ) ( 1.44425e+01 3.59027e+01 ) ( 1.4443e+01 3.59031e+01 ) ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Route
description: 'A road, visitor path, internal track or circulation route within or
  between heritage zones,

  typed as CIDOC-CRM E26 Physical Feature (a subclass of E53 Place). Profiles

  ogc.heritage.place, inheriting JSON-FG geometry and core CIDOC-CRM place attributes,

  and adding route type, surface condition, width, accessibility rating, permitted
  use

  and a parent-site reference.


  Geometry is mandatory (centreline polyline or bounding polygon per JSON-FG).

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/schema.yaml
- type: object
  properties:
    properties:
      type: object
      properties:
        routeType:
          type: string
          format: uri
          description: 'Route classification as a controlled vocabulary URI (typically
            Getty AAT). Examples: visitor path, access road, pedestrian thoroughfare,
            service track. Maps to crm:P2_has_type.'
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
          x-jsonld-type: '@id'
        surface:
          type: string
          description: Surface material or condition description (e.g. "gravel", "cobblestone",
            "tarmac", "unpaved"). Informational; no controlled vocabulary required.
        width:
          type: number
          minimum: 0
          description: Approximate path or road width in metres.
        accessibilityRating:
          type: string
          description: Accessibility classification for visitors with reduced mobility
            (e.g. "fully accessible", "partially accessible", "not accessible"). Maps
            to crm:P44_has_condition.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
        permittedUse:
          type: string
          description: Permitted use categories for this route (e.g. "pedestrian only",
            "vehicles permitted", "service access only"). Informational.
        parentSite:
          type: string
          format: uri
          description: URI of the heritage site (ogc.heritage.heritage-site) within
            which this route lies or which it connects. Maps to crm:P89_falls_within.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P89_falls_within
          x-jsonld-type: '@id'
      required:
      - routeType
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/route/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/route/schema.yaml)


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
    "routeType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "accessibilityRating": "crm:P44_has_condition",
    "parentSite": {
      "@id": "crm:P89_falls_within",
      "@type": "@id"
    },
    "geojson": "https://purl.org/geojson/vocab#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "oa": "http://www.w3.org/ns/oa#",
    "dct": "http://purl.org/dc/terms/",
    "owlTime": "http://www.w3.org/2006/time#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "owl": "http://www.w3.org/2002/07/owl#",
    "identifier": "crm:P1_is_identified_by",
    "name": "crm:P87_is_identified_by",
    "placeType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "partOf": {
      "@id": "crm:P89_falls_within",
      "@type": "@id"
    },
    "sameAs": {
      "@id": "owl:sameAs",
      "@type": "@id"
    },
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/route/context.jsonld)

## Sources

* [CIDOC-CRM E26 Physical Feature](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E26)
* [HERITALISE D8.2 CRRS-017](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/route`

