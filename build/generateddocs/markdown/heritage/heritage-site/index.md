
# Heritage Site (Schema)

`ogc.heritage.heritage-site` *v0.1*

A designated heritage site or pilot area, typed as CIDOC-CRM E27 Site. Extends 'place' with site-level designation, responsible organisation and rights fields.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

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

## Examples

### Reggia di Venaria Reale pilot site (REQ-001)
The Reggia di Venaria pilot area as a heritage site: UNESCO World Heritage designation, responsible consortium, CC-BY rights, boundary polygon, and Getty TGN authority match. Covers REQ-001 (Venaria overall pilot area).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.617, 45.130],
        [7.642, 45.130],
        [7.642, 45.145],
        [7.617, 45.145],
        [7.617, 45.130]
      ]
    ]
  },
  "properties": {
    "identifier": "RV-SITE-001",
    "name": "Reggia di Venaria Reale",
    "siteType": "http://vocab.getty.edu/aat/300006915",
    "designation": "UNESCO World Heritage List no. 823",
    "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
    "rights": "https://creativecommons.org/licenses/by/4.0/",
    "sameAs": "http://vocab.getty.edu/tgn/7011057"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-site/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.617,
          45.13
        ],
        [
          7.642,
          45.13
        ],
        [
          7.642,
          45.145
        ],
        [
          7.617,
          45.145
        ],
        [
          7.617,
          45.13
        ]
      ]
    ]
  },
  "properties": {
    "identifier": "RV-SITE-001",
    "name": "Reggia di Venaria Reale",
    "siteType": "http://vocab.getty.edu/aat/300006915",
    "designation": "UNESCO World Heritage List no. 823",
    "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
    "rights": "https://creativecommons.org/licenses/by/4.0/",
    "sameAs": "http://vocab.getty.edu/tgn/7011057"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/site/reggia-di-venaria> a geojson:Feature ;
    dct:rights "https://creativecommons.org/licenses/by/4.0/" ;
    crm:P1_is_identified_by "RV-SITE-001",
        "UNESCO World Heritage List no. 823" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300006915> ;
    crm:P50_has_current_keeper "Consorzio delle Residenze Reali Sabaude" ;
    crm:P87_is_identified_by "Reggia di Venaria Reale" ;
    owl:sameAs <http://vocab.getty.edu/tgn/7011057> ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 7.617e+00 4.513e+01 ) ( 7.642e+00 4.513e+01 ) ( 7.642e+00 4.5145e+01 ) ( 7.617e+00 4.5145e+01 ) ( 7.617e+00 4.513e+01 ) ) ) ] .


```


### Villa Portelli garden as a heritage site (MT-03)
The Villa Portelli garden (Malta pilot) as a heritage site: national monument designation, Heritage Malta as responsible organisation, boundary polygon. Demonstrates reuse of the same block for a different pilot's garden-level entity (MT-03), distinguished only by siteType and designation values.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [14.512, 35.895],
        [14.516, 35.895],
        [14.516, 35.898],
        [14.512, 35.898],
        [14.512, 35.895]
      ]
    ]
  },
  "properties": {
    "identifier": "MT-SITE-GARDEN-01",
    "name": "Villa Portelli Garden",
    "siteType": "http://vocab.getty.edu/aat/300008012",
    "designation": "Grade 1 Scheduled Monument (Malta)",
    "responsibleOrganisation": "Heritage Malta",
    "rights": "https://creativecommons.org/licenses/by-nc/4.0/"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-site/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/site/villa-portelli-garden",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          14.512,
          35.895
        ],
        [
          14.516,
          35.895
        ],
        [
          14.516,
          35.898
        ],
        [
          14.512,
          35.898
        ],
        [
          14.512,
          35.895
        ]
      ]
    ]
  },
  "properties": {
    "identifier": "MT-SITE-GARDEN-01",
    "name": "Villa Portelli Garden",
    "siteType": "http://vocab.getty.edu/aat/300008012",
    "designation": "Grade 1 Scheduled Monument (Malta)",
    "responsibleOrganisation": "Heritage Malta",
    "rights": "https://creativecommons.org/licenses/by-nc/4.0/"
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

<https://heritalise-eccch.eu/resource/site/villa-portelli-garden> a geojson:Feature ;
    dct:rights "https://creativecommons.org/licenses/by-nc/4.0/" ;
    crm:P1_is_identified_by "Grade 1 Scheduled Monument (Malta)",
        "MT-SITE-GARDEN-01" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300008012> ;
    crm:P50_has_current_keeper "Heritage Malta" ;
    crm:P87_is_identified_by "Villa Portelli Garden" ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 1.4512e+01 3.5895e+01 ) ( 1.4516e+01 3.5895e+01 ) ( 1.4516e+01 3.5898e+01 ) ( 1.4512e+01 3.5898e+01 ) ( 1.4512e+01 3.5895e+01 ) ) ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Heritage Site
description: 'A designated cultural heritage site or pilot study area, typed as CIDOC-CRM
  E27 Site

  (a subclass of E53 Place and E26 Physical Feature). Profiles ogc.heritage.place,

  inheriting JSON-FG geometry and core CIDOC-CRM place attributes, and adding site-level

  designation, responsible organisation and rights fields.


  A garden, palace complex or protected landscape is modelled as a heritage-site with
  an

  appropriate siteType; sub-zones within the site are modelled as ogc.heritage.place
  records

  linked back via partOf.

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/schema.yaml
- type: object
  properties:
    properties:
      type: object
      properties:
        siteType:
          type: string
          format: uri
          description: 'Heritage designation type as a controlled vocabulary URI (typically
            Getty AAT). Examples: palace complex, garden, World Heritage Site, national
            monument. Maps to crm:P2_has_type.'
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
          x-jsonld-type: '@id'
        designation:
          type: string
          description: Formal heritage designation code or label, e.g. a UNESCO World
            Heritage List reference number or national inventory entry. Maps to crm:P1_is_identified_by
            as a second identifier alongside the local inventory identifier.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
        responsibleOrganisation:
          type: string
          description: Name or URI of the body responsible for managing or custodying
            this site (crm:P50_has_current_keeper).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P50_has_current_keeper
        rights:
          type: string
          description: Access and use rights statement, preferably a Creative Commons
            or rights statement URI (dct:rights).
          x-jsonld-id: http://purl.org/dc/terms/rights
      required:
      - siteType
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-site/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-site/schema.yaml)


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
    "siteType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "designation": "crm:P1_is_identified_by",
    "responsibleOrganisation": "crm:P50_has_current_keeper",
    "rights": "dct:rights",
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
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-site/context.jsonld)

## Sources

* [CIDOC-CRM E27 Site](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E27)
* [HERITALISE D8.2 REQ-001](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/heritage-site`

