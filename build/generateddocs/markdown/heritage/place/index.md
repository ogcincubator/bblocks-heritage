
# Place (Schema)

`ogc.heritage.place` *v0.1*

A spatial location relevant to cultural heritage — a site, building, room or landscape feature — modelled as a CIDOC-CRM E53 Place and encoded as an OGC JSON-FG Feature.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

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

## Examples

### A room within a site, linked to a broader place and to Getty TGN
The Great Gallery room within the Reggia di Venaria, typed with a Getty AAT concept, nested under its parent site via `partOf`, and matched to a Getty TGN authority record.
#### json
```json
{
  "id": "https://example.org/heritalise/place/great-gallery",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [7.628, 45.135]
  },
  "properties": {
    "identifier": "RV-ROOM-GG",
    "name": "Great Gallery, Reggia di Venaria",
    "placeType": "http://vocab.getty.edu/aat/300004826",
    "partOf": "https://example.org/heritalise/place/reggia-di-venaria",
    "sameAs": "http://vocab.getty.edu/tgn/7274089"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/context.jsonld",
  "id": "https://example.org/heritalise/place/great-gallery",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      7.628,
      45.135
    ]
  },
  "properties": {
    "identifier": "RV-ROOM-GG",
    "name": "Great Gallery, Reggia di Venaria",
    "placeType": "http://vocab.getty.edu/aat/300004826",
    "partOf": "https://example.org/heritalise/place/reggia-di-venaria",
    "sameAs": "http://vocab.getty.edu/tgn/7274089"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://example.org/heritalise/place/great-gallery> a geojson:Feature ;
    crm:P1_is_identified_by "RV-ROOM-GG" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004826> ;
    crm:P87_is_identified_by "Great Gallery, Reggia di Venaria" ;
    crm:P89_falls_within <https://example.org/heritalise/place/reggia-di-venaria> ;
    owl:sameAs <http://vocab.getty.edu/tgn/7274089> ;
    geojson:geometry [ a geojson:Point ;
            geojson:coordinates ( 7.628e+00 4.5135e+01 ) ] .


```


### A minimal record with only the required properties
Only `geometry`, `identifier` and `name` are required — type, broader-place links and authority matches can be added incrementally, matching the legacy-archive migration scenario in UC-V-3.
#### json
```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [7.6285, 45.1338]
  },
  "properties": {
    "identifier": "RV-SITE",
    "name": "Reggia di Venaria"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/context.jsonld",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      7.6285,
      45.1338
    ]
  },
  "properties": {
    "identifier": "RV-SITE",
    "name": "Reggia di Venaria"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

[] a geojson:Feature ;
    crm:P1_is_identified_by "RV-SITE" ;
    crm:P87_is_identified_by "Reggia di Venaria" ;
    geojson:geometry [ a geojson:Point ;
            geojson:coordinates ( 7.6285e+00 4.51338e+01 ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Place
description: 'A spatial location relevant to cultural heritage (site, building, room,
  landscape feature),

  modelled as a CIDOC-CRM E53 Place and encoded as an OGC JSON-FG Feature: geometry
  stays at

  the top level (JSON-FG `geometry`/`place`/`time` members), while CIDOC-CRM attributes
  are

  nested under `properties`, per JSON-FG/GeoJSON convention.

  '
allOf:
- $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/geo/json-fg/feature-lenient/schema.yaml
- type: object
  properties:
    properties:
      type: object
      description: CIDOC-CRM E53 Place attributes, nested per JSON-FG/GeoJSON convention.
      properties:
        identifier:
          type: string
          description: A local gazetteer or inventory identifier (CIDOC-CRM P1_is_identified_by).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
        name:
          type: string
          description: The place's name (CIDOC-CRM P87_is_identified_by, via an implied
            E44 Place Appellation).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P87_is_identified_by
        placeType:
          type: string
          format: uri
          description: The type of place, typically a Getty AAT concept URI (CIDOC-CRM
            P2_has_type).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
          x-jsonld-type: '@id'
        partOf:
          type: string
          format: uri
          description: URI of a broader place this place falls within, e.g. a room
            within a building within a site (CIDOC-CRM P89_falls_within).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P89_falls_within
          x-jsonld-type: '@id'
        sameAs:
          type: string
          format: uri
          description: URI of an equivalent record in an external authority, typically
            Getty TGN (owl:sameAs).
          x-jsonld-id: http://www.w3.org/2002/07/owl#sameAs
          x-jsonld-type: '@id'
      required:
      - identifier
      - name
  required:
  - properties
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  owl: http://www.w3.org/2002/07/owl#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/schema.yaml)


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
    "geojson": "https://purl.org/geojson/vocab#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "oa": "http://www.w3.org/ns/oa#",
    "dct": "http://purl.org/dc/terms/",
    "owlTime": "http://www.w3.org/2006/time#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "owl": "http://www.w3.org/2002/07/owl#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/context.jsonld)

## Sources

* [CIDOC-CRM](https://www.cidoc-crm.org/)
* [OGC Testbed-17: OGC Features and Geometries JSON (JSON-FG) Engineering Report](http://docs.ogc.org/per/21-017r1.html)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/place`

