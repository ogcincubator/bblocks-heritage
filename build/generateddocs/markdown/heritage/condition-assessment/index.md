
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

### Properties

| Property | CRM / PROV mapping | Required | Notes |
|---|---|---|---|
| `type` | `sci:E14_Condition_Assessment` (via `@type`) | yes | Fixed const; enables `sh:targetClass` |
| `affectedObject` | `crm:P140_assigned_attribute_to` (@id) | yes | URI of the `heritage-object`, `architectural-space` or `place` being assessed |
| `conditionState` | `crm:P35_has_identified` (@json) | yes | E3 Condition State sub-object: `conditionType` (Getty AAT URI), `severity` (minor/moderate/severe/critical), `extent` (text) |
| `assessmentDate` | `crm:P4_has_time-span` (xsd:date) | yes | ISO 8601 date |
| `assessor` | `prov:wasAttributedTo` (@id) | no | URI of agent (person or organisation) |
| `method` | `crm:P33_used_specific_technique` | no | Inspection technique label or URI |
| `confidence` | *(unmapped)* | no | low / medium / high |
| `evidence` | `crm:P16_used_specific_object` (@id, set) | no | URIs of supporting `digital-representation` or `archival-document` records |
| `recommendation` | `crm:P3_has_note` | no | Free-text conservation recommendation |
| `reviewStatus` | *(unmapped)* | no | Workflow status string |
| `footprint` | `geojson:geometry` (@json) | no | GeoJSON geometry localising the condition on the object or surface |

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
  "id": "https://example.org/heritalise/condition/RV-CA-GG-2026-003",
  "type": "ConditionAssessment",
  "affectedObject": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "conditionState": {
    "conditionType": "http://vocab.getty.edu/aat/300379825",
    "severity": "moderate",
    "extent": "Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7."
  },
  "assessmentDate": "2026-03-15",
  "assessor": "https://example.org/heritalise/actor/sofia-bianchi",
  "method": "Visual inspection with raking-light photography",
  "confidence": "high",
  "evidence": [
    "https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-01",
    "https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-02"
  ],
  "recommendation": "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation.",
  "reviewStatus": "reviewed",
  "footprint": {
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
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/context.jsonld",
  "id": "https://example.org/heritalise/condition/RV-CA-GG-2026-003",
  "type": "ConditionAssessment",
  "affectedObject": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "conditionState": {
    "conditionType": "http://vocab.getty.edu/aat/300379825",
    "severity": "moderate",
    "extent": "Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7."
  },
  "assessmentDate": "2026-03-15",
  "assessor": "https://example.org/heritalise/actor/sofia-bianchi",
  "method": "Visual inspection with raking-light photography",
  "confidence": "high",
  "evidence": [
    "https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-01",
    "https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-02"
  ],
  "recommendation": "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation.",
  "reviewStatus": "reviewed",
  "footprint": {
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

<https://example.org/heritalise/condition/RV-CA-GG-2026-003> a sci:E14_Condition_Assessment ;
    crm:P140_assigned_attribute_to <https://example.org/heritalise/space/galleria-grande-bay-7> ;
    crm:P16_used_specific_object <https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-01>,
        <https://example.org/heritalise/digital/RV-CA-GG-2026-003-photo-02> ;
    crm:P33_used_specific_technique "Visual inspection with raking-light photography" ;
    crm:P35_has_identified "{\"conditionType\":\"http://vocab.getty.edu/aat/300379825\",\"extent\":\"Linear crack approximately 80 cm long along the plaster joint between the vault fresco and the cornice moulding, north face of bay 7.\",\"severity\":\"moderate\"}"^^rdf:JSON ;
    crm:P3_has_note "Apply consolidant injection to crack edges within 6 months; re-assess after stabilisation." ;
    crm:P4_has_time-span "2026-03-15"^^xsd:date ;
    prov:wasAttributedTo <https://example.org/heritalise/actor/sofia-bianchi> ;
    geojson:geometry "{\"coordinates\":[[[7.62725,45.13442],[7.62731,45.13442],[7.62731,45.13438],[7.62725,45.13438],[7.62725,45.13442]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```


### Paint loss, Villa Portelli Grand Salon — Malta
A condition assessment of minor paint flaking on the west wall of the Grand Salon at Villa Portelli. A minimal example — no footprint or evidence URIs — demonstrating that only type, affectedObject, conditionState and assessmentDate are required.
#### json
```json
{
  "id": "https://example.org/heritalise/condition/MT-CA-VP-2026-011",
  "type": "ConditionAssessment",
  "affectedObject": "https://example.org/heritalise/space/villa-portelli-salon",
  "conditionState": {
    "conditionType": "http://vocab.getty.edu/aat/300379770",
    "severity": "minor",
    "extent": "Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m²."
  },
  "assessmentDate": "2026-04-20",
  "assessor": "https://example.org/heritalise/actor/heritage-malta-conservation",
  "method": "Visual inspection",
  "confidence": "high",
  "recommendation": "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point.",
  "reviewStatus": "draft"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/context.jsonld",
  "id": "https://example.org/heritalise/condition/MT-CA-VP-2026-011",
  "type": "ConditionAssessment",
  "affectedObject": "https://example.org/heritalise/space/villa-portelli-salon",
  "conditionState": {
    "conditionType": "http://vocab.getty.edu/aat/300379770",
    "severity": "minor",
    "extent": "Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m\u00b2."
  },
  "assessmentDate": "2026-04-20",
  "assessor": "https://example.org/heritalise/actor/heritage-malta-conservation",
  "method": "Visual inspection",
  "confidence": "high",
  "recommendation": "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point.",
  "reviewStatus": "draft"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix sci: <http://www.ics.forth.gr/isl/CRMsci/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://example.org/heritalise/condition/MT-CA-VP-2026-011> a sci:E14_Condition_Assessment ;
    crm:P140_assigned_attribute_to <https://example.org/heritalise/space/villa-portelli-salon> ;
    crm:P33_used_specific_technique "Visual inspection" ;
    crm:P35_has_identified "{\"conditionType\":\"http://vocab.getty.edu/aat/300379770\",\"extent\":\"Scattered flaking paint on lower 40 cm of the west wall, covering an area of approximately 0.3 m².\",\"severity\":\"minor\"}"^^rdf:JSON ;
    crm:P3_has_note "Consolidate flaking paint layers; monitor humidity levels via adjacent monitoring-point." ;
    crm:P4_has_time-span "2026-04-20"^^xsd:date ;
    prov:wasAttributedTo <https://example.org/heritalise/actor/heritage-malta-conservation> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Condition Assessment
description: A qualitative conservation inspection (CRMsci E14 Condition Assessment)
  recording the condition state of a heritage object or place.
type: object
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this condition assessment record.
    x-jsonld-id: '@id'
  type:
    const: ConditionAssessment
    x-jsonld-id: '@type'
  affectedObject:
    type: string
    format: uri
    description: URI of the heritage-object, architectural-space or place being assessed.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P140_assigned_attribute_to
    x-jsonld-type: '@id'
  conditionState:
    type: object
    description: 'The E3 Condition State found by this assessment: the type of condition,
      its severity and a textual description of its spatial extent.'
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
    description: URI of the agent (person or organisation) who carried out the assessment.
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
    description: URIs of digital representations or archival documents used as evidence.
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
  footprint:
    type: object
    description: GeoJSON geometry localising the condition on the affected object
      or surface (point, polyline or polygon patch).
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
        x-jsonld-id: '@type'
      coordinates:
        type: array
    required:
    - type
    - coordinates
    x-jsonld-id: https://purl.org/geojson/vocab#geometry
    x-jsonld-type: '@json'
required:
- type
- affectedObject
- conditionState
- assessmentDate
x-jsonld-extra-terms:
  ConditionAssessment: http://www.ics.forth.gr/isl/CRMsci/E14_Condition_Assessment
x-jsonld-prefixes:
  sci: http://www.ics.forth.gr/isl/CRMsci/
  crm: http://www.cidoc-crm.org/cidoc-crm/
  xsd: http://www.w3.org/2001/XMLSchema#
  prov: http://www.w3.org/ns/prov#
  geojson: https://purl.org/geojson/vocab#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/condition-assessment/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "ConditionAssessment": "sci:E14_Condition_Assessment",
    "id": "@id",
    "type": "@type",
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
    "footprint": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "sci": "http://www.ics.forth.gr/isl/CRMsci/",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "prov": "http://www.w3.org/ns/prov#",
    "geojson": "https://purl.org/geojson/vocab#",
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

