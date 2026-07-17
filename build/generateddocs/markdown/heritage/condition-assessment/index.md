
# Condition Assessment (Schema)

`ogc.heritage.condition-assessment` *v0.1*

A qualitative conservation inspection event (CRMsci E14 Condition Assessment) recording the condition state of a heritage object or place at a point in time, with assessor, method, severity and optional spatial localisation.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Condition Assessment

A `ConditionAssessment` records a qualitative conservation inspection of a heritage resource
at a specific point in time. It is anchored to **CRMsci E14 Condition Assessment**, which
specialises CRM E13 Attribute Assignment for the specific task of evaluating the physical
state of a heritage object or place.

This block is distinct from [`ogc.heritage.observation`](../observation), which covers
continuous sensor-based measurements (CRMsci E16 Measurement / SOSA Observation). A condition
assessment is a human-led inspection event that produces a qualitative finding (E3 Condition
State), not a numeric sensor reading.

Encoded as a GeoJSON/JSON-FG Feature: CIDOC-CRM/CRMsci attributes nest under `properties`, and
`properties.choType` carries the CRMsci class as a second alias to `@type`, alongside the fixed
`type: "Feature"`. Localisation is optional — `geometry` may be `null` (no spatial
localisation), an embedded GeoJSON geometry, or replaced by a `topology` reference
(`ogc.geo.topo.features.topo-feature`) into geometry already recorded on the affected object.

### Properties (all nested under `properties`)

| Property | CRM / PROV mapping | Required | Notes |
|---|---|---|---|
| `choType` | `sci:E14_Condition_Assessment` (via second `@type` alias) | yes | Fixed const; enables `sh:targetClass` |
| `affectedObject` | `crm:P140_assigned_attribute_to` (@id) | yes | URI of the `heritage-object`, `architectural-space` or `place` being assessed |
| `conditionState` | `crm:P35_has_identified` (@json) | yes | E3 Condition State sub-object: `conditionType` (Getty AAT URI), `severity` (minor/moderate/severe/critical), `extent` (text) |
| `assessmentDate` | `crm:P4_has_time-span` (xsd:date) | yes | ISO 8601 date |
| `assessor` | `prov:wasAttributedTo` (@id) | no | URI of agent (person or organisation) |
| `method` | `crm:P33_used_specific_technique` | no | Inspection technique label or URI |
| `confidence` | *(unmapped)* | no | low / medium / high |
| `evidence` | `crm:P16_used_specific_object` (@id, set) | no | URIs of supporting `digital-representation` or `archival-document` records |
| `recommendation` | `crm:P3_has_note` | no | Free-text conservation recommendation |
| `reviewStatus` | *(unmapped)* | no | Workflow status string |

`confidence` and `reviewStatus` are workflow/operational fields with no natural CRM predicate;
they are retained in JSON but not mapped to RDF.

`conditionState` is treated as an opaque JSON value (`@type: "@json"`) rather than a blank
node, keeping the graph simple while preserving the structured payload for JSON consumers.

### Use cases

- **REQ-006** — Localised deterioration, damage, moisture or cracking recorded by Venaria
  conservators (pathology-area layer of the MHS)
- Any pilot partner performing periodic visual or instrument-assisted inspections of heritage
  objects, surfaces or spaces

## Examples

### Vault crack, Galleria Grande Bay 7 — Venaria (REQ-006)
A condition assessment of a moderate crack in the fresco plaster of bay 7 of the Galleria Grande, recorded by a Venaria conservator. Includes a polygon footprint localising the crack, photographic evidence references and a conservation recommendation. Covers REQ-006 (condition/pathology area).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/condition/RV-CA-GG-2026-003",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.62725, 45.13442],
        [7.62731, 45.13442],
        [7.62731, 45.13438],
        [7.62725, 45.13438],
        [7.62725, 45.13442]
      ]
    ]
  },
  "properties": {
    "choType": "ConditionAssessment",
    "affectedObject": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
    "conditionState": {
      "conditionType": "http://vocab.getty.edu/aat/300379825",
      "severity": "moderate",
      "extent": "Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7."
    },
    "assessmentDate": "2026-03-15",
    "assessor": "https://heritalise-eccch.eu/resource/actor/sofia-bianchi",
    "method": "Visual inspection with raking-light photography",
    "confidence": "high",
    "evidence": [
      "https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-01",
      "https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-02"
    ],
    "recommendation": "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation.",
    "reviewStatus": "reviewed"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/condition/RV-CA-GG-2026-003",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.62725,
          45.13442
        ],
        [
          7.62731,
          45.13442
        ],
        [
          7.62731,
          45.13438
        ],
        [
          7.62725,
          45.13438
        ],
        [
          7.62725,
          45.13442
        ]
      ]
    ]
  },
  "properties": {
    "choType": "ConditionAssessment",
    "affectedObject": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
    "conditionState": {
      "conditionType": "http://vocab.getty.edu/aat/300379825",
      "severity": "moderate",
      "extent": "Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7."
    },
    "assessmentDate": "2026-03-15",
    "assessor": "https://heritalise-eccch.eu/resource/actor/sofia-bianchi",
    "method": "Visual inspection with raking-light photography",
    "confidence": "high",
    "evidence": [
      "https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-01",
      "https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-02"
    ],
    "recommendation": "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation.",
    "reviewStatus": "reviewed"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix sci: <http://www.ics.forth.gr/isl/CRMsci/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/condition/RV-CA-GG-2026-003> a sci:E14_Condition_Assessment,
        geojson:Feature ;
    crm:P140_assigned_attribute_to <https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> ;
    crm:P16_used_specific_object <https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-01>,
        <https://heritalise-eccch.eu/resource/digital/RV-CA-GG-2026-003-photo-02> ;
    crm:P33_used_specific_technique "Visual inspection with raking-light photography" ;
    crm:P35_has_identified "{\"conditionType\":\"http://vocab.getty.edu/aat/300379825\",\"extent\":\"Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7.\",\"severity\":\"moderate\"}"^^rdf:JSON ;
    crm:P3_has_note "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation." ;
    crm:P4_has_time-span "2026-03-15"^^xsd:date ;
    prov:wasAttributedTo <https://heritalise-eccch.eu/resource/actor/sofia-bianchi> ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 7.62725e+00 4.513442e+01 ) ( 7.62731e+00 4.513442e+01 ) ( 7.62731e+00 4.513438e+01 ) ( 7.62725e+00 4.513438e+01 ) ( 7.62725e+00 4.513442e+01 ) ) ) ] .


```


### Paint loss, Villa Portelli Grand Salon — Malta
A condition assessment of minor paint flaking on the west wall of the Grand Salon at Villa Portelli. A minimal example — `geometry: null` (no spatial localisation) and no evidence URIs — demonstrating that only choType, affectedObject, conditionState and assessmentDate are required.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/condition/MT-CA-VP-2026-011",
  "type": "Feature",
  "geometry": null,
  "properties": {
    "choType": "ConditionAssessment",
    "affectedObject": "https://heritalise-eccch.eu/resource/space/villa-portelli-salon",
    "conditionState": {
      "conditionType": "http://vocab.getty.edu/aat/300379770",
      "severity": "minor",
      "extent": "Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m²."
    },
    "assessmentDate": "2026-04-20",
    "assessor": "https://heritalise-eccch.eu/resource/actor/heritage-malta-conservation",
    "method": "Visual inspection",
    "confidence": "high",
    "recommendation": "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point.",
    "reviewStatus": "draft"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/condition/MT-CA-VP-2026-011",
  "type": "Feature",
  "geometry": null,
  "properties": {
    "choType": "ConditionAssessment",
    "affectedObject": "https://heritalise-eccch.eu/resource/space/villa-portelli-salon",
    "conditionState": {
      "conditionType": "http://vocab.getty.edu/aat/300379770",
      "severity": "minor",
      "extent": "Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m\u00b2."
    },
    "assessmentDate": "2026-04-20",
    "assessor": "https://heritalise-eccch.eu/resource/actor/heritage-malta-conservation",
    "method": "Visual inspection",
    "confidence": "high",
    "recommendation": "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point.",
    "reviewStatus": "draft"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix sci: <http://www.ics.forth.gr/isl/CRMsci/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/condition/MT-CA-VP-2026-011> a sci:E14_Condition_Assessment,
        geojson:Feature ;
    crm:P140_assigned_attribute_to <https://heritalise-eccch.eu/resource/space/villa-portelli-salon> ;
    crm:P33_used_specific_technique "Visual inspection" ;
    crm:P35_has_identified "{\"conditionType\":\"http://vocab.getty.edu/aat/300379770\",\"extent\":\"Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m².\",\"severity\":\"minor\"}"^^rdf:JSON ;
    crm:P3_has_note "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point." ;
    crm:P4_has_time-span "2026-04-20"^^xsd:date ;
    prov:wasAttributedTo <https://heritalise-eccch.eu/resource/actor/heritage-malta-conservation> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Condition Assessment
description: 'A qualitative conservation inspection (CRMsci E14 Condition Assessment)
  recording the condition state of a heritage object or place, encoded as a GeoJSON/JSON-FG
  Feature: CIDOC-CRM/CRMsci attributes nest under `properties` (GeoJSON convention),
  and a `choType` property carries the CRMsci class as a second alias to `@type`,
  alongside the fixed `type: "Feature"`. Localisation is optional: `geometry` may
  be `null` (no spatial localisation), an embedded GeoJSON geometry, or given by reference
  via topology (`topology`, see ogc.geo.topo.features.topo-feature) to avoid duplicating
  coordinates already recorded on the affected object.'
$defs:
  properties:
    type: object
    required:
    - choType
    - affectedObject
    - conditionState
    - assessmentDate
    properties:
      choType:
        const: ConditionAssessment
        description: 'CRMsci class discriminator for this record (maps to sci:E14_Condition_Assessment
          via a second alias to @type, alongside the fixed GeoJSON `type: "Feature"`).'
        x-jsonld-id: '@type'
      affectedObject:
        type: string
        format: uri
        description: URI of the heritage-object, architectural-space or place being
          assessed.
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P140_assigned_attribute_to
        x-jsonld-type: '@id'
      conditionState:
        type: object
        description: 'The E3 Condition State found by this assessment: the type of
          condition, its severity and a textual description of its spatial extent.'
        properties:
          conditionType:
            type: string
            format: uri
            description: Getty AAT URI for the type of condition (e.g. crack, moisture,
              corrosion).
          severity:
            type: string
            enum:
            - minor
            - moderate
            - severe
            - critical
            description: Qualitative severity level.
          extent:
            type: string
            description: Textual description of the area, length or volume affected.
        required:
        - conditionType
        - severity
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P35_has_identified
        x-jsonld-type: '@json'
      assessmentDate:
        type: string
        format: date
        description: Date on which the assessment was carried out (ISO 8601).
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
        x-jsonld-type: http://www.w3.org/2001/XMLSchema#date
      assessor:
        type: string
        format: uri
        description: URI of the agent (person or organisation) who carried out the
          assessment.
        x-jsonld-id: http://www.w3.org/ns/prov#wasAttributedTo
        x-jsonld-type: '@id'
      method:
        type: string
        description: Label or URI of the inspection technique used (e.g. "visual inspection",
          "thermographic survey", "photogrammetric analysis").
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P33_used_specific_technique
      confidence:
        type: string
        enum:
        - low
        - medium
        - high
        description: Confidence level in the assessment result.
      evidence:
        type: array
        description: URIs of digital representations or archival documents used as
          evidence.
        items:
          type: string
          format: uri
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P16_used_specific_object
        x-jsonld-type: '@id'
        x-jsonld-container: '@set'
      recommendation:
        type: string
        description: Free-text conservation recommendation arising from this assessment.
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
      reviewStatus:
        type: string
        description: Workflow status of this record (e.g. draft, reviewed, approved).
allOf:
- oneOf:
  - allOf:
    - $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/geo/json-fg/feature-lenient/schema.yaml
    - type: object
      description: 'Condition assessment localised by its own embedded geometry, or
        not spatially localised at all (`geometry: null`).'
      not:
        required:
        - topology
      properties:
        geometry:
          oneOf:
          - type: 'null'
          - type: object
            required:
            - type
            - coordinates
            properties:
              type:
                type: string
                enum:
                - Point
                - LineString
                - Polygon
                - MultiPoint
                - MultiLineString
                - MultiPolygon
  - $ref: https://ogcincubator.github.io/topo-feature/build/annotated/geo/topo/features/topo-feature/schema.yaml
- type: object
  required:
  - id
  properties:
    id:
      type: string
      format: uri
      description: Persistent URI identifying this condition assessment record (overrides
        the inherited `id`, which also allows a bare number).
    properties:
      $ref: '#/$defs/properties'
x-jsonld-extra-terms:
  ConditionAssessment: http://www.ics.forth.gr/isl/CRMsci/E14_Condition_Assessment
x-jsonld-prefixes:
  sci: http://www.ics.forth.gr/isl/CRMsci/
  crm: http://www.cidoc-crm.org/cidoc-crm/
  xsd: http://www.w3.org/2001/XMLSchema#
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/schema.yaml)


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
    "choType": "@type",
    "affectedObject": {
      "@id": "crm:P140_assigned_attribute_to",
      "@type": "@id"
    },
    "conditionState": {
      "@id": "crm:P35_has_identified",
      "@type": "@json"
    },
    "assessmentDate": {
      "@id": "crm:P4_has_time-span",
      "@type": "xsd:date"
    },
    "assessor": {
      "@id": "prov:wasAttributedTo",
      "@type": "@id"
    },
    "method": "crm:P33_used_specific_technique",
    "evidence": {
      "@id": "crm:P16_used_specific_object",
      "@type": "@id",
      "@container": "@set"
    },
    "recommendation": "crm:P3_has_note",
    "ConditionAssessment": "sci:E14_Condition_Assessment",
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
    "geojson": "https://purl.org/geojson/vocab#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "oa": "http://www.w3.org/ns/oa#",
    "dct": "http://purl.org/dc/terms/",
    "owlTime": "http://www.w3.org/2006/time#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "sci": "http://www.ics.forth.gr/isl/CRMsci/",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "prov": "http://www.w3.org/ns/prov#",
    "topo": "https://purl.org/geojson/topo#",
    "prof": "http://www.w3.org/ns/dx/prof/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/context.jsonld)

## Sources

* [CRMsci E14 Condition Assessment](https://www.ics.forth.gr/isl/CRMsci/CRMsci_v2.0.pdf)
* [CIDOC-CRM E3 Condition State](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E3)
* [HERITALISE D8.2 REQ-006](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/condition-assessment`

