
# Historical Place (Schema)

`ogc.heritage.historical-place` *v0.1*

A named historical spatial feature — room, area, route or landscape designation — that has since been renamed, restructured or disappeared, mapped to its current spatial equivalent with a confidence rating (CIDOC-CRM E53 Place + P89, CRRS-015).

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Historical Place

A **Historical Place** records a named spatial feature — room designation, area code, route name
or landscape element — that existed at some point in the past and has since been renamed,
restructured or disappeared. The record bridges archival nomenclature to the contemporary
spatial inventory by linking each historical designation to its best current equivalent and
recording the confidence of that crosswalk.

## Semantic anchor

The block profiles **`ogc.heritage.place`**, inheriting JSON-FG Feature geometry and core
CIDOC-CRM E53 Place attributes (`identifier`, `name`, `placeType`, `partOf`, `sameAs`).
Added properties express the temporal scope of the historical designation, its archival
source, its crosswalk to a current space or place, and the confidence of that mapping.

## Property table

| Property | Required | CRM mapping | Description |
|---|---|---|---|
| *(inherited)* `identifier` | yes | `crm:P1_is_identified_by` | Inventory or gazetteer identifier |
| *(inherited)* `name` | yes | `crm:P87_is_identified_by` | Historical place name or room code |
| *(inherited)* `placeType` | no | `crm:P2_has_type` (@id) | Getty AAT URI for the type of feature |
| *(inherited)* `partOf` | no | `crm:P89_falls_within` (@id) | Broader spatial container (spatial hierarchy) |
| *(inherited)* `sameAs` | no | `owl:sameAs` (@id) | Authority record URI (Getty TGN, Wikidata) |
| `datePeriod` | no* | `crm:P4_has_time-span` | Period or date range when this name was in use |
| `historicalSource` | no* | `crm:P70i_is_documented_in` (@id) | Source carrier or surrogate URI |
| `currentEquivalent` | no | `crm:P89_falls_within` (@id) | Contemporary space or place URI |
| `mappingConfidence` | yes | `crm:P3_has_note` | `confirmed`, `probable`, `approximate`, `uncertain` |

\* `datePeriod` or `historicalSource` (or both) should be provided to support archival crosswalk traceability.

## Design notes

- `currentEquivalent` maps to `crm:P89_falls_within`, the same predicate as the inherited
  `partOf`. Use `currentEquivalent` for historical crosswalk relationships; use `partOf` for
  contemporary spatial containment. Both produce the same RDF predicate — the distinction is
  in the authoring intent.
- `mappingConfidence` maps to `crm:P3_has_note` with a literal value. Enum values follow a
  scale from archivally confirmed identity to speculative association.
- SHACL targets `sh:targetSubjectsOf crm:P4_has_time-span` to avoid a class-based target
  (JSON-FG Feature produces `geojson:Feature`, not `crm:E53_Place`, after uplift). The shape
  only checks IRI validity on optional links; required fields are validated by JSON Schema.

## Standard alignments

- **CIDOC-CRM** (● mandatory): E53 Place (via `place` profile); P89 `falls_within` for both
  spatial containment and historical-to-current mapping; P4 time-span; P70i documented in.
- **JSON-FG** (● mandatory): inherited geometry encoding as a JSON-FG Feature.
- **PROV-O** (● mandatory): inherited from `place` via `digital-representation` lineage where
  a source carrier is cited.
- **JSON-LD** (● mandatory): context maps all properties to CRM URIs.

## Use cases

- Reggia di Venaria (CRRS-015): historical room names from 18th-century inventories mapped
  to current gallery numbers, with source references to archival plans held in the Archivio
  di Stato di Torino.
- Villa Portelli Malta: historical garden features (pergola, terrace names) from pre-war
  survey plans crosswalked to current heritage-site zone records.

## Examples

### Sala degli Scudieri — historical room name, Reggia di Venaria (CRRS-015)
An 18th-century room designation ("Sala degli Scudieri") from the 1720 Venaria inventory, mapped to its current survey equivalent (Sala 10, Eastern Gallery) with "probable" confidence based on floor-plan comparison. The historicalSource URI references the archival inventory record held in the Archivio di Stato di Torino. Covers CRRS-015 (historical place / legacy spatial feature).
#### json
```json
{
  "type": "Feature",
  "id": "https://heritalise.eu/venaria/historical-place/sala-scudieri-1720",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.6045, 45.1285],
        [7.6050, 45.1285],
        [7.6050, 45.1289],
        [7.6045, 45.1289],
        [7.6045, 45.1285]
      ]
    ]
  },
  "properties": {
    "identifier": "ASTo-VR-inv1720-sala-scudieri",
    "name": "Sala degli Scudieri",
    "placeType": "http://vocab.getty.edu/aat/300004704",
    "datePeriod": "1720–1798",
    "historicalSource": "https://heritalise.eu/venaria/source-carrier/ASTo-VR-inv1720",
    "currentEquivalent": "https://heritalise.eu/venaria/space/sala-10-galleria-est",
    "mappingConfidence": "probable",
    "sameAs": "https://www.wikidata.org/entity/Q999999"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-place/context.jsonld",
  "type": "Feature",
  "id": "https://heritalise.eu/venaria/historical-place/sala-scudieri-1720",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.6045,
          45.1285
        ],
        [
          7.605,
          45.1285
        ],
        [
          7.605,
          45.1289
        ],
        [
          7.6045,
          45.1289
        ],
        [
          7.6045,
          45.1285
        ]
      ]
    ]
  },
  "properties": {
    "identifier": "ASTo-VR-inv1720-sala-scudieri",
    "name": "Sala degli Scudieri",
    "placeType": "http://vocab.getty.edu/aat/300004704",
    "datePeriod": "1720\u20131798",
    "historicalSource": "https://heritalise.eu/venaria/source-carrier/ASTo-VR-inv1720",
    "currentEquivalent": "https://heritalise.eu/venaria/space/sala-10-galleria-est",
    "mappingConfidence": "probable",
    "sameAs": "https://www.wikidata.org/entity/Q999999"
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

<https://heritalise.eu/venaria/historical-place/sala-scudieri-1720> a geojson:Feature ;
    crm:P1_is_identified_by "ASTo-VR-inv1720-sala-scudieri" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004704> ;
    crm:P3_has_note "probable" ;
    crm:P4_has_time-span "1720–1798" ;
    crm:P70i_is_documented_in <https://heritalise.eu/venaria/source-carrier/ASTo-VR-inv1720> ;
    crm:P87_is_identified_by "Sala degli Scudieri" ;
    crm:P89_falls_within <https://heritalise.eu/venaria/space/sala-10-galleria-est> ;
    owl:sameAs <https://www.wikidata.org/entity/Q999999> ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 7.6045e+00 4.51285e+01 ) ( 7.605e+00 4.51285e+01 ) ( 7.605e+00 4.51289e+01 ) ( 7.6045e+00 4.51289e+01 ) ( 7.6045e+00 4.51285e+01 ) ) ) ] .


```


### South Pergola Terrace — historical garden feature, Villa Portelli, Malta
A historical garden terrace at Villa Portelli named in pre-1943 architectural plans, approximately corresponding to the current south garden zone. A minimal example — no historicalSource or sameAs — demonstrating that only identifier, name, geometry and mappingConfidence are required (beyond the inherited place fields).
#### json
```json
{
  "type": "Feature",
  "id": "https://heritalise.eu/malta/historical-place/south-pergola-1930",
  "geometry": {
    "type": "Point",
    "coordinates": [14.4521, 35.8897]
  },
  "properties": {
    "identifier": "VP-arch1930-south-pergola",
    "name": "South Pergola Terrace",
    "placeType": "http://vocab.getty.edu/aat/300006981",
    "datePeriod": "c. 1890–1943",
    "currentEquivalent": "https://heritalise.eu/malta/place/garden-zone-south",
    "mappingConfidence": "approximate"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-place/context.jsonld",
  "type": "Feature",
  "id": "https://heritalise.eu/malta/historical-place/south-pergola-1930",
  "geometry": {
    "type": "Point",
    "coordinates": [
      14.4521,
      35.8897
    ]
  },
  "properties": {
    "identifier": "VP-arch1930-south-pergola",
    "name": "South Pergola Terrace",
    "placeType": "http://vocab.getty.edu/aat/300006981",
    "datePeriod": "c. 1890\u20131943",
    "currentEquivalent": "https://heritalise.eu/malta/place/garden-zone-south",
    "mappingConfidence": "approximate"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise.eu/malta/historical-place/south-pergola-1930> a geojson:Feature ;
    crm:P1_is_identified_by "VP-arch1930-south-pergola" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300006981> ;
    crm:P3_has_note "approximate" ;
    crm:P4_has_time-span "c. 1890–1943" ;
    crm:P87_is_identified_by "South Pergola Terrace" ;
    crm:P89_falls_within <https://heritalise.eu/malta/place/garden-zone-south> ;
    geojson:geometry [ a geojson:Point ;
            geojson:coordinates ( 1.44521e+01 3.58897e+01 ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Historical Place
description: "A named historical spatial feature \u2014 room, area, route or landscape
  designation \u2014 that has\nsince been renamed, restructured or disappeared. Profiles
  ogc.heritage.place, inheriting\nJSON-FG geometry and core CRM place attributes,
  and adding historical period, source\nprovenance, a link to the current spatial
  equivalent, and a mapping-confidence rating.\n\nThe inherited `name` field carries
  the historical designation (e.g. \"Sala degli Scudieri\",\n\"Cortile dei Cani\").
  The inherited `partOf` field or the profile-specific `currentEquivalent`\nfield
  (both map to crm:P89_falls_within) link the historical feature to the contemporary\nspace
  or place record that best corresponds to it.\n"
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/place/schema.yaml
- type: object
  properties:
    properties:
      type: object
      properties:
        datePeriod:
          type: string
          description: "The historical period or date range during which this place
            name or spatial configuration was in use, expressed as a free-text string
            (e.g. \"1720\u20131798\", \"18th century\", \"pre-1943\"). Maps to crm:P4_has_time-span."
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
        historicalSource:
          type: string
          format: uri
          description: URI of the archival or documentary source (ogc.heritage.source-carrier
            or ogc.heritage.digital-surrogate) from which this historical designation
            is derived. Maps to crm:P70i_is_documented_in.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70i_is_documented_in
          x-jsonld-type: '@id'
        currentEquivalent:
          type: string
          format: uri
          description: URI of the contemporary space or place record that best corresponds
            to this historical feature (ogc.heritage.architectural-space, ogc.heritage.heritage-site,
            or ogc.heritage.place). Maps to crm:P89_falls_within, the same predicate
            as the inherited `partOf` field; use `currentEquivalent` when the semantic
            intent is historical crosswalk rather than spatial containment.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P89_falls_within
          x-jsonld-type: '@id'
        mappingConfidence:
          type: string
          enum:
          - confirmed
          - probable
          - approximate
          - uncertain
          description: 'Confidence level of the crosswalk between this historical
            place and its current equivalent: confirmed (identical boundary documented),
            probable (strong archival evidence), approximate (partial overlap), uncertain
            (speculative). Maps to crm:P3_has_note.'
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
      required:
      - mappingConfidence
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-place/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-place/schema.yaml)


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
    "datePeriod": "crm:P4_has_time-span",
    "historicalSource": {
      "@id": "crm:P70i_is_documented_in",
      "@type": "@id"
    },
    "currentEquivalent": {
      "@id": "crm:P89_falls_within",
      "@type": "@id"
    },
    "mappingConfidence": "crm:P3_has_note",
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
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-place/context.jsonld)

## Sources

* [CIDOC-CRM E53 Place](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E53)
* [CIDOC-CRM P89 falls within](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#P89)
* [HERITALISE D8.2 CRRS-015](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/historical-place`

