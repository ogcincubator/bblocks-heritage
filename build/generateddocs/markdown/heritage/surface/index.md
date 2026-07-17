
# Surface (Schema)

`ogc.heritage.surface` *v0.1*

A material finish layer or physical surface feature recognised on an architectural component or heritage object (CIDOC-CRM E25 Man-Made Feature), documenting material, application technique, historical phase and spatial footprint (CRRS-005).

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Surface

A **Surface** records a material finish layer or physical surface feature recognised on an
architectural component or heritage object — plaster, stucco, fresco, stone, gilding or any
other applied or inherent surface finish. The block documents the host object, material and
technique using Getty AAT controlled terms, the historical phase of application and a mandatory
polygon footprint delineating the spatial extent of the surface.

## Semantic anchor

The block is modelled as **CIDOC-CRM E25 Man-Made Feature** (`crm:E25_Man-Made_Feature`), a
subclass of E26 Physical Feature, representing a surface as a feature *recognised on* a physical
object rather than an independently manufactured entity. The mandatory link `crm:P56i_is_found_on`
connects the surface to its host heritage object or space.

## Property table

| Property | Required | CRM mapping | Description |
|---|---|---|---|
| `id` | no | `@id` | Persistent URI for this surface record |
| `type` | yes | `@type` → `crm:E25_Man-Made_Feature` | Fixed token `Surface` |
| `hostObject` | yes | `crm:P56i_is_found_on` (@id) | URI of the host heritage-object, architectural-space or building |
| `material` | yes | `crm:P45_consists_of` (@id) | Getty AAT URI for the primary surface material |
| `technique` | no | `crm:P33_used_specific_technique` (@id) | Getty AAT URI for the application technique |
| `historicalPhase` | no | `crm:P4_has_time-span` | Date or period label of application/last significant alteration |
| `exposure` | no | `crm:P3_has_note` | Positional context: "vault intrados", "north wall", "floor", etc. |
| `conditionSummary` | no | `crm:P44_has_condition` | Brief conservation condition description |
| `conditionAssessment` | no | *(unmapped — JSON payload)* | URI of a linked condition-assessment record |
| `sourceEvidence` | no | `crm:P70i_is_documented_in` (@id) | Source carrier or surrogate URI documenting the surface |
| `reviewStatus` | no | *(unmapped — JSON payload)* | Workflow status: "draft", "reviewed", "approved" |
| `footprint` | yes | `geojson:geometry` (`@type: @json`) | GeoJSON Polygon or MultiPolygon of the surface extent |

## Design notes

- `footprint` follows the embedded-geometry pattern used by `condition-assessment`: a plain
  GeoJSON Geometry sub-object mapped as `@type: "@json"` to prevent coordinate arrays being
  interpreted as RDF lists. The schema restricts geometry type to Polygon/MultiPolygon, since
  a surface always has area extent (not a point or line).
- `conditionAssessment` and `reviewStatus` are left unmapped in context — they carry workflow
  or cross-reference payload with no suitable CRM predicate and are not validated by SHACL.
- `sh:targetClass crm:E25_Man-Made_Feature` is safe: no other block in the register instantiates
  that class.
- `crm:P56i_is_found_on` is the inverse of `P56_bears_feature`; it records that this surface
  feature is found on (borne by) the host object.

## Standard alignments

- **CIDOC-CRM** (● mandatory): E25 Man-Made Feature; P56i found on (host linkage); P45 consists
  of (material); P33 technique; P4 time-span (phase); P3 note (exposure); P44 condition.
- **Getty AAT** (● mandatory): material and technique URIs.
- **GeoJSON / JSON-FG** (● mandatory): footprint geometry via `geojson:geometry`.
- **JSON-LD** (● mandatory): full context mapping.
- **CRMsci E14 Condition Assessment** (○ optional): linked via `conditionAssessment` URI.

## Use cases

- Reggia di Venaria (CRRS-005): Baroque fresco plaster on the vault of the Galleria Grande,
  with a polygon footprint covering an individual bay and links to photogrammetric survey
  products and condition-assessment records.
- Villa Portelli Malta: Polychrome stone floor surface in the Grand Salon, with material
  (limestone) and technique (in-situ cast mosaic) documented with Getty AAT terms.

## Examples

### Baroque fresco vault surface, Galleria Grande Bay 7 — Venaria (CRRS-005)
The painted fresco plaster of vault bay 7 in the Galleria Grande at Reggia di Venaria. Material (fresco, Getty AAT 300178433) and technique (fresco secco, Getty AAT 300053343) are expressed via controlled terms. A polygon footprint delineates the bay extent. Includes links to a photogrammetric survey product (sourceEvidence) and a condition assessment record. Covers CRRS-005 (surface / finish layer).
#### json
```json
{
  "id": "https://heritalise.eu/venaria/surface/GG-vault-bay7-fresco",
  "type": "Surface",
  "hostObject": "https://heritalise.eu/venaria/space/galleria-grande-bay7",
  "material": "http://vocab.getty.edu/aat/300178433",
  "technique": "http://vocab.getty.edu/aat/300053343",
  "historicalPhase": "1699–1707",
  "exposure": "vault intrados",
  "conditionSummary": "localised cracking on north edge; stable elsewhere",
  "conditionAssessment": "https://heritalise.eu/venaria/condition/CA-GG-bay7-2024-06",
  "sourceEvidence": "https://heritalise.eu/venaria/survey/photogrammetry-GG-2023",
  "reviewStatus": "reviewed",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.6046, 45.1286],
        [7.6049, 45.1286],
        [7.6049, 45.1288],
        [7.6046, 45.1288],
        [7.6046, 45.1286]
      ]
    ]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/context.jsonld",
  "id": "https://heritalise.eu/venaria/surface/GG-vault-bay7-fresco",
  "type": "Surface",
  "hostObject": "https://heritalise.eu/venaria/space/galleria-grande-bay7",
  "material": "http://vocab.getty.edu/aat/300178433",
  "technique": "http://vocab.getty.edu/aat/300053343",
  "historicalPhase": "1699\u20131707",
  "exposure": "vault intrados",
  "conditionSummary": "localised cracking on north edge; stable elsewhere",
  "conditionAssessment": "https://heritalise.eu/venaria/condition/CA-GG-bay7-2024-06",
  "sourceEvidence": "https://heritalise.eu/venaria/survey/photogrammetry-GG-2023",
  "reviewStatus": "reviewed",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.6046,
          45.1286
        ],
        [
          7.6049,
          45.1286
        ],
        [
          7.6049,
          45.1288
        ],
        [
          7.6046,
          45.1288
        ],
        [
          7.6046,
          45.1286
        ]
      ]
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise.eu/venaria/surface/GG-vault-bay7-fresco> a crm:E25_Man-Made_Feature ;
    crm:P33_used_specific_technique <http://vocab.getty.edu/aat/300053343> ;
    crm:P3_has_note "vault intrados" ;
    crm:P44_has_condition "localised cracking on north edge; stable elsewhere" ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300178433> ;
    crm:P4_has_time-span "1699–1707" ;
    crm:P56i_is_found_on <https://heritalise.eu/venaria/space/galleria-grande-bay7> ;
    crm:P70i_is_documented_in <https://heritalise.eu/venaria/survey/photogrammetry-GG-2023> ;
    geojson:geometry "{\"coordinates\":[[[7.6046,45.1286],[7.6049,45.1286],[7.6049,45.1288],[7.6046,45.1288],[7.6046,45.1286]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```


### Stone floor surface, Grand Salon — Villa Portelli, Malta
The limestone floor surface of the Grand Salon at Villa Portelli. A minimal example — only type, hostObject, material and footprint are provided — demonstrating the required fields. Material is mapped to the Getty AAT stone concept (300011176).
#### json
```json
{
  "id": "https://heritalise.eu/malta/surface/VP-grand-salon-floor",
  "type": "Surface",
  "hostObject": "https://heritalise.eu/malta/space/grand-salon",
  "material": "http://vocab.getty.edu/aat/300011176",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [14.4519, 35.8896],
        [14.4523, 35.8896],
        [14.4523, 35.8899],
        [14.4519, 35.8899],
        [14.4519, 35.8896]
      ]
    ]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/context.jsonld",
  "id": "https://heritalise.eu/malta/surface/VP-grand-salon-floor",
  "type": "Surface",
  "hostObject": "https://heritalise.eu/malta/space/grand-salon",
  "material": "http://vocab.getty.edu/aat/300011176",
  "footprint": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          14.4519,
          35.8896
        ],
        [
          14.4523,
          35.8896
        ],
        [
          14.4523,
          35.8899
        ],
        [
          14.4519,
          35.8899
        ],
        [
          14.4519,
          35.8896
        ]
      ]
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise.eu/malta/surface/VP-grand-salon-floor> a crm:E25_Man-Made_Feature ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300011176> ;
    crm:P56i_is_found_on <https://heritalise.eu/malta/space/grand-salon> ;
    geojson:geometry "{\"coordinates\":[[[14.4519,35.8896],[14.4523,35.8896],[14.4523,35.8899],[14.4519,35.8899],[14.4519,35.8896]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```


### Vault fresco with extent by reference (geometry-by-reference / topology)
The fresco covering the vault of Galleria Grande bay 7 extends across the entire bay, so its spatial extent coincides exactly with the bay's own footprint (galleria-grande-bay.json) rather than a separately-drawn polygon. No `footprint` is given here — instead `references` points at the hostObject's own record, using the geometry-by-reference pattern from ogc.ogc-utils.topology to avoid duplicating coordinates.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/surface/galleria-grande-bay-7-vault-fresco",
  "type": "Surface",
  "hostObject": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
  "material": "http://vocab.getty.edu/aat/300178433",
  "technique": "http://vocab.getty.edu/aat/300053343",
  "historicalPhase": "1700–1710",
  "exposure": "vault intrados",
  "conditionSummary": "stable",
  "references": [
    "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
  ]
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/surface/galleria-grande-bay-7-vault-fresco",
  "type": "Surface",
  "hostObject": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
  "material": "http://vocab.getty.edu/aat/300178433",
  "technique": "http://vocab.getty.edu/aat/300053343",
  "historicalPhase": "1700\u20131710",
  "exposure": "vault intrados",
  "conditionSummary": "stable",
  "references": [
    "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
  ]
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise-eccch.eu/resource/surface/galleria-grande-bay-7-vault-fresco> a crm:E25_Man-Made_Feature ;
    crm:P33_used_specific_technique <http://vocab.getty.edu/aat/300053343> ;
    crm:P3_has_note "vault intrados" ;
    crm:P44_has_condition "stable" ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300178433> ;
    crm:P4_has_time-span "1700–1710" ;
    crm:P56i_is_found_on <https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> ;
    geojson:relatedFeatures ( <https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> ) .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Surface
description: "A material finish layer or physical surface feature recognised on an
  architectural component or heritage object (CIDOC-CRM E25 Man-Made Feature). Records
  the host object, material and technique (Getty AAT), historical phase, exposure
  context and a mandatory polygon or surface-patch geometry \u2014 embedded, or given
  by reference to shared/topological geometry (see ogc.ogc-utils.topology). May be
  linked to a condition-assessment record for pathology documentation."
type: object
required:
- id
- type
- hostObject
- material
oneOf:
- type: object
  description: Surface with its own embedded footprint geometry.
  properties:
    footprint:
      type: object
      description: GeoJSON Geometry delineating the spatial extent of this surface
        as a polygon or surface-patch. Coordinates should be in the reference system
        of the host structure's survey (local or EPSG:4326). Maps to geojson:geometry.
      properties:
        type:
          type: string
          enum:
          - Polygon
          - MultiPolygon
          x-jsonld-id: '@type'
        coordinates:
          type: array
      required:
      - type
      - coordinates
      x-jsonld-id: https://purl.org/geojson/vocab#geometry
      x-jsonld-type: '@json'
  required:
  - footprint
  not:
    required:
    - references
- $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/ogc-utils/topology/schema.yaml
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this surface record.
    x-jsonld-id: '@id'
  type:
    const: Surface
    description: Fixed type token identifying this record as a surface (maps to crm:E25_Man-Made_Feature).
    x-jsonld-id: '@type'
  hostObject:
    type: string
    format: uri
    description: URI of the heritage-object, architectural-space or building that
      this surface is recognised on (crm:P56i_is_found_on).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P56i_is_found_on
    x-jsonld-type: '@id'
  material:
    type: string
    format: uri
    description: 'Getty AAT URI for the primary surface material, e.g.: painted plaster
      (300014927), stucco (300014927), stone (300011176), fresco (300177433), gold
      leaf (300263998). Maps to crm:P45_consists_of.'
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P45_consists_of
    x-jsonld-type: '@id'
  technique:
    type: string
    format: uri
    description: 'Getty AAT URI for the application or fabrication technique, e.g.:
      fresco (300053343), oil painting (300178684), polishing (300053344). Maps to
      crm:P33_used_specific_technique.'
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P33_used_specific_technique
    x-jsonld-type: '@id'
  historicalPhase:
    type: string
    description: "Date or period label of the phase in which this surface was applied
      or last significantly altered, e.g. \"1690\u20131710\", \"post-1945 restoration\".
      Maps to crm:P4_has_time-span."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
  exposure:
    type: string
    description: Positional context of the surface within the architectural element,
      e.g. "vault intrados", "north wall", "floor", "exterior facade". Maps to crm:P3_has_note.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  conditionSummary:
    type: string
    description: Brief description of the current conservation condition of this surface,
      e.g. "stable", "localised cracking", "active moisture ingress". Maps to crm:P44_has_condition.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
  conditionAssessment:
    type: string
    format: uri
    description: URI of a condition-assessment record (ogc.heritage.condition-assessment)
      that provides detailed pathology documentation for this surface.
  sourceEvidence:
    type: string
    format: uri
    description: URI of the archival or survey source (ogc.heritage.source-carrier
      or ogc.heritage.digital-surrogate) that documents this surface's existence and
      attributes. Maps to crm:P70i_is_documented_in.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70i_is_documented_in
    x-jsonld-type: '@id'
  reviewStatus:
    type: string
    description: Workflow status of this record, e.g. "draft", "reviewed", "approved".
x-jsonld-extra-terms:
  Surface: http://www.cidoc-crm.org/cidoc-crm/E25_Man-Made_Feature
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  geojson: https://purl.org/geojson/vocab#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "footprint": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "LineString": "geojson:LineString",
    "type": "@type",
    "references": {
      "@id": "geojson:relatedFeatures",
      "@type": "@id",
      "@container": "@list"
    },
    "Surface": "crm:E25_Man-Made_Feature",
    "id": "@id",
    "hostObject": {
      "@id": "crm:P56i_is_found_on",
      "@type": "@id"
    },
    "material": {
      "@id": "crm:P45_consists_of",
      "@type": "@id"
    },
    "technique": {
      "@id": "crm:P33_used_specific_technique",
      "@type": "@id"
    },
    "historicalPhase": "crm:P4_has_time-span",
    "exposure": "crm:P3_has_note",
    "conditionSummary": "crm:P44_has_condition",
    "sourceEvidence": {
      "@id": "crm:P70i_is_documented_in",
      "@type": "@id"
    },
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "geojson": "https://purl.org/geojson/vocab#",
    "csdm": "https://linked.data.gov.au/def/csdm/",
    "dct": "http://purl.org/dc/terms/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/surface/context.jsonld)

## Sources

* [CIDOC-CRM E25 Man-Made Feature](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E25)
* [Getty Art & Architecture Thesaurus](https://www.getty.edu/research/tools/vocabularies/aat/)
* [HERITALISE D8.2 CRRS-005](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/surface`

