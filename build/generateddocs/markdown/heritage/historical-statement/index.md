
# Historical Statement (Schema)

`ogc.heritage.historical-statement` *v0.1*

A piece of historical knowledge extracted from a document or legacy record — an attribution, identification, date assignment or annotation — modelled as a CIDOC-CRM E13 Attribute Assignment with an inner E33 Linguistic Object and a W3C Web Annotation evidence locator pointing to the source passage.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Historical Statement

A piece of historical knowledge extracted from a document or legacy record — an authorship
attribution, date assignment, iconographic identification, material note, or condition
observation — modelled as a **CIDOC-CRM E13 Attribute Assignment**. The act of extraction
and interpretation is the E13 event; the extracted text itself is an inner **E33 Linguistic
Object** carried in the `statement` sub-object.

A W3C Web Annotation **evidence locator** (`evidenceLocator`) pinpoints the precise passage
within the source record — page number, image region (IIIF fragment), or XML row — using the
OA `oa:hasBody` / `oa:SpecificResource` pattern.

## CRM anchor

| JSON property | CRM / vocab predicate | Notes |
|---|---|---|
| `type` | `rdf:type crm:E13_Attribute_Assignment` | Fixed const `"HistoricalStatement"` |
| `statementType` | `crm:P2_has_type` | Category label or Getty AAT URI |
| `statement` | `crm:P141_assigned` | Inner E33; `@type: @json` opaque object |
| `describedObject` | `crm:P140_assigned_attribute_to` | URI of the heritage entity described |
| `sourceRecord` | `crm:P16_used_specific_object` | URI → source-carrier or digital-surrogate |
| `evidenceLocator` | `oa:hasBody` | W3C OA SpecificResource; `@type: @json` |
| `reviewer` | `prov:wasAttributedTo` | URI of the person/org who extracted the statement |
| `confidence` | `crm:P3_has_note` | Qualitative confidence label |
| `datePeriod` | `crm:P4_has_time-span` | Period the statement pertains to |

## Inner `statement` sub-object (E33 Linguistic Object)

Stored as an opaque JSON value (`@type: "@json"`) linked via `crm:P141_assigned`. Contains:

| Field | Description |
|---|---|
| `text` (required) | The verbatim or summarised statement text |
| `language` | BCP 47 language code, e.g. `"it"`, `"en"`, `"mt"` |
| `datePeriod` | The date or period the text addresses |
| `keywords` | Controlled-vocabulary or free keywords |

## Evidence locator (W3C OA)

The `evidenceLocator` object follows the W3C Web Annotation SpecificResource pattern. The
`source` URI points to the document or digital surrogate containing the evidence; the
`selector` narrows down to the fragment:

```json
{
  "source": "https://…/digital-surrogate/ASTo-VR-lett-1699-dig",
  "selector": {
    "type": "FragmentSelector",
    "conformsTo": "http://www.w3.org/TR/media-frags/",
    "value": "page=3"
  }
}
```

For IIIF-hosted images, use `"type": "FragmentSelector"` with an `xywh=` value conforming to
the IIIF Image API 3 region syntax.

## Relationship to other blocks

- `sourceRecord` must reference a `source-carrier` or `digital-surrogate` URI.
- `describedObject` should reference a `heritage-object`, `place`, `architectural-space`,
  or similar block URI.
- Multiple `historical-statement` records can describe the same heritage entity from different
  sources — they are linked via `describedObject`, not embedded.

## Pilot use case

Primarily addresses **CRRS-014** (Reggia di Venaria): art-historical attributions, dated
inscriptions and material identifications extracted from 17th–18th century archival sources
(letters, inventories, engravings) held in ASTo and the palace archive.

## Examples

### Authorship attribution for a ceiling fresco (CRRS-014, Venaria)
An art historian's attribution of the ceiling fresco in the Galleria Grande to the painter Michelangelo Morello, extracted from an 18th-century letter in the State Archive of Turin. Includes a W3C OA evidence locator pointing to page 3 of the digitised letter.
#### json
```json
{
  "id": "https://data.regiadivenaria.it/historical-statement/attr-morello-galleria-grande-2024-001",
  "type": "HistoricalStatement",
  "statementType": "authorship attribution",
  "statement": {
    "text": "La volta della Galleria Grande fu dipinta da Michelangelo Morello per commissione del Duca Vittorio Amedeo II nel 1699, come attestato dalla lettera del 14 marzo di quell'anno.",
    "language": "it",
    "datePeriod": "1699",
    "keywords": ["ceiling fresco", "attribution", "Morello", "Galleria Grande", "Savoy commission"]
  },
  "describedObject": "https://data.regiadivenaria.it/heritage-object/galleria-grande-ceiling-fresco",
  "sourceRecord": "https://data.regiadivenaria.it/source-carrier/ASTo-VR-lett-1699-mar14",
  "evidenceLocator": {
    "source": "https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-lett-1699-mar14-scan",
    "selector": {
      "type": "FragmentSelector",
      "conformsTo": "http://www.w3.org/TR/media-frags/",
      "value": "page=3"
    }
  },
  "reviewer": "https://orcid.org/0000-0000-0000-0001",
  "confidence": "probable",
  "datePeriod": "1699"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-statement/context.jsonld",
  "id": "https://data.regiadivenaria.it/historical-statement/attr-morello-galleria-grande-2024-001",
  "type": "HistoricalStatement",
  "statementType": "authorship attribution",
  "statement": {
    "text": "La volta della Galleria Grande fu dipinta da Michelangelo Morello per commissione del Duca Vittorio Amedeo II nel 1699, come attestato dalla lettera del 14 marzo di quell'anno.",
    "language": "it",
    "datePeriod": "1699",
    "keywords": [
      "ceiling fresco",
      "attribution",
      "Morello",
      "Galleria Grande",
      "Savoy commission"
    ]
  },
  "describedObject": "https://data.regiadivenaria.it/heritage-object/galleria-grande-ceiling-fresco",
  "sourceRecord": "https://data.regiadivenaria.it/source-carrier/ASTo-VR-lett-1699-mar14",
  "evidenceLocator": {
    "source": "https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-lett-1699-mar14-scan",
    "selector": {
      "type": "FragmentSelector",
      "conformsTo": "http://www.w3.org/TR/media-frags/",
      "value": "page=3"
    }
  },
  "reviewer": "https://orcid.org/0000-0000-0000-0001",
  "confidence": "probable",
  "datePeriod": "1699"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix oa: <http://www.w3.org/ns/oa#> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://data.regiadivenaria.it/historical-statement/attr-morello-galleria-grande-2024-001> a crm:E13_Attribute_Assignment ;
    crm:P140_assigned_attribute_to <https://data.regiadivenaria.it/heritage-object/galleria-grande-ceiling-fresco> ;
    crm:P141_assigned "{\"datePeriod\":\"1699\",\"keywords\":[\"ceiling fresco\",\"attribution\",\"Morello\",\"Galleria Grande\",\"Savoy commission\"],\"language\":\"it\",\"text\":\"La volta della Galleria Grande fu dipinta da Michelangelo Morello per commissione del Duca Vittorio Amedeo II nel 1699, come attestato dalla lettera del 14 marzo di quell'anno.\"}"^^rdf:JSON ;
    crm:P16_used_specific_object <https://data.regiadivenaria.it/source-carrier/ASTo-VR-lett-1699-mar14> ;
    crm:P2_has_type "authorship attribution" ;
    crm:P3_has_note "probable" ;
    crm:P4_has_time-span "1699" ;
    oa:hasBody "{\"selector\":{\"conformsTo\":\"http://www.w3.org/TR/media-frags/\",\"type\":\"FragmentSelector\",\"value\":\"page=3\"},\"source\":\"https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-lett-1699-mar14-scan\"}"^^rdf:JSON ;
    prov:wasAttributedTo <https://orcid.org/0000-0000-0000-0001> .


```


### Construction technique note extracted from a Malta photograph caption (HM)
A statement identifying rubble-stone masonry technique in the north wing of Villa Portelli, extracted from a 1953 photograph caption held by Heritage Malta. Minimal example without an evidence locator.
#### json
```json
{
  "id": "https://data.heritagemalta.org/historical-statement/hs-villap-north-wing-masonry-2023-001",
  "type": "HistoricalStatement",
  "statementType": "construction technique",
  "statement": {
    "text": "The north wing of Villa Portelli shows coursed rubble-stone masonry (ħaġar tal-franka) typical of mid-19th century Maltese vernacular construction.",
    "language": "en",
    "datePeriod": "mid-19th century",
    "keywords": ["rubble masonry", "franka limestone", "vernacular", "north wing"]
  },
  "describedObject": "https://data.heritagemalta.org/architectural-space/villa-portelli-north-wing",
  "sourceRecord": "https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig",
  "reviewer": "https://data.heritagemalta.org/actor/conservation-dept",
  "confidence": "certain"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-statement/context.jsonld",
  "id": "https://data.heritagemalta.org/historical-statement/hs-villap-north-wing-masonry-2023-001",
  "type": "HistoricalStatement",
  "statementType": "construction technique",
  "statement": {
    "text": "The north wing of Villa Portelli shows coursed rubble-stone masonry (\u0127a\u0121ar tal-franka) typical of mid-19th century Maltese vernacular construction.",
    "language": "en",
    "datePeriod": "mid-19th century",
    "keywords": [
      "rubble masonry",
      "franka limestone",
      "vernacular",
      "north wing"
    ]
  },
  "describedObject": "https://data.heritagemalta.org/architectural-space/villa-portelli-north-wing",
  "sourceRecord": "https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig",
  "reviewer": "https://data.heritagemalta.org/actor/conservation-dept",
  "confidence": "certain"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://data.heritagemalta.org/historical-statement/hs-villap-north-wing-masonry-2023-001> a crm:E13_Attribute_Assignment ;
    crm:P140_assigned_attribute_to <https://data.heritagemalta.org/architectural-space/villa-portelli-north-wing> ;
    crm:P141_assigned "{\"datePeriod\":\"mid-19th century\",\"keywords\":[\"rubble masonry\",\"franka limestone\",\"vernacular\",\"north wing\"],\"language\":\"en\",\"text\":\"The north wing of Villa Portelli shows coursed rubble-stone masonry (ħaġar tal-franka) typical of mid-19th century Maltese vernacular construction.\"}"^^rdf:JSON ;
    crm:P16_used_specific_object <https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig> ;
    crm:P2_has_type "construction technique" ;
    crm:P3_has_note "certain" ;
    prov:wasAttributedTo <https://data.heritagemalta.org/actor/conservation-dept> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Historical Statement
description: A piece of historical knowledge extracted from a document or legacy record,
  modelled as a CIDOC-CRM E13 Attribute Assignment. Captures the statement text (inner
  E33 Linguistic Object), the source record it was drawn from, an optional W3C Web
  Annotation evidence locator identifying the precise passage, and reviewer provenance.
type: object
required:
- id
- type
- statementType
- sourceRecord
- reviewer
- confidence
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this historical statement record.
    x-jsonld-id: '@id'
  type:
    const: HistoricalStatement
    description: Fixed type token (maps to crm:E13_Attribute_Assignment).
    x-jsonld-id: '@type'
  statementType:
    type: string
    description: Category of the statement, e.g. "material attribution", "authorship
      attribution", "date attribution", "iconographic identification", "condition
      note", "construction technique". Use a Getty AAT URI where available.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
  statement:
    type: object
    description: The inner CIDOC-CRM E33 Linguistic Object containing the extracted
      text and metadata. Stored as an opaque JSON sub-object (crm:P141_assigned).
    required:
    - text
    properties:
      text:
        type: string
        description: The verbatim or summarised statement text.
      language:
        type: string
        description: BCP 47 language code of the text, e.g. "it", "en", "mt".
      datePeriod:
        type: string
        description: "Date or period to which the statement pertains, e.g. \"1699\",
          \"early 18th century\", \"1720\u20131735\"."
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
      keywords:
        type: array
        items:
          type: string
        description: Optional controlled-vocabulary terms or free keywords summarising
          the statement.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P141_assigned
    x-jsonld-type: '@json'
  describedObject:
    type: string
    format: uri
    description: URI of the heritage entity (heritage-object, place, architectural-space,
      etc.) to which this statement assigns an attribute (crm:P140_assigned_attribute_to).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P140_assigned_attribute_to
    x-jsonld-type: '@id'
  sourceRecord:
    type: string
    format: uri
    description: URI of the source-carrier or digital-surrogate from which this statement
      was extracted (crm:P16_used_specific_object). Required.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P16_used_specific_object
    x-jsonld-type: '@id'
  evidenceLocator:
    type: object
    description: W3C Web Annotation SpecificResource identifying the precise passage
      (page, image region, row) within the source record. Stored as an opaque JSON
      sub-object (oa:hasBody).
    properties:
      source:
        type: string
        format: uri
        description: URI of the document or digital surrogate containing the evidence.
      selector:
        type: object
        description: 'W3C OA selector identifying the fragment: use FragmentSelector
          for page/time ranges, XPathSelector for XML documents, or a free-text description
          in the `value` field.'
        properties:
          type:
            type: string
            description: OA selector type, e.g. "FragmentSelector", "XPathSelector",
              "TextPositionSelector".
            x-jsonld-id: '@type'
          conformsTo:
            type: string
            format: uri
            description: URI of the fragment specification (e.g. http://www.w3.org/TR/media-frags/).
          value:
            type: string
            description: The selector value, e.g. "page=12", "xywh=160,120,320,240",
              "row=42".
    x-jsonld-id: http://www.w3.org/ns/oa#hasBody
    x-jsonld-type: '@json'
  reviewer:
    type: string
    format: uri
    description: URI of the person or organisation who extracted and reviewed this
      statement (prov:wasAttributedTo). Required.
    x-jsonld-id: http://www.w3.org/ns/prov#wasAttributedTo
    x-jsonld-type: '@id'
  confidence:
    type: string
    description: Qualitative confidence level assigned by the reviewer, e.g. "certain",
      "probable", "possible", "speculative". Required.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  datePeriod:
    type: string
    description: Date or period of the original event or condition described by the
      statement (crm:P4_has_time-span), e.g. "1699", "late 17th century".
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
x-jsonld-extra-terms:
  HistoricalStatement: http://www.cidoc-crm.org/cidoc-crm/E13_Attribute_Assignment
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  oa: http://www.w3.org/ns/oa#
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-statement/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-statement/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HistoricalStatement": "crm:E13_Attribute_Assignment",
    "id": "@id",
    "type": "@type",
    "statementType": "crm:P2_has_type",
    "statement": {
      "@id": "crm:P141_assigned",
      "@type": "@json"
    },
    "describedObject": {
      "@id": "crm:P140_assigned_attribute_to",
      "@type": "@id"
    },
    "sourceRecord": {
      "@id": "crm:P16_used_specific_object",
      "@type": "@id"
    },
    "evidenceLocator": {
      "@id": "oa:hasBody",
      "@type": "@json"
    },
    "reviewer": {
      "@id": "prov:wasAttributedTo",
      "@type": "@id"
    },
    "confidence": "crm:P3_has_note",
    "datePeriod": "crm:P4_has_time-span",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "oa": "http://www.w3.org/ns/oa#",
    "prov": "http://www.w3.org/ns/prov#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/historical-statement/context.jsonld)

## Sources

* [CIDOC-CRM E13 Attribute Assignment](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E13)
* [CIDOC-CRM E33 Linguistic Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E33)
* [W3C Web Annotation Data Model](https://www.w3.org/TR/annotation-model/)
* [PROV-O: The PROV Ontology](https://www.w3.org/TR/prov-o/)
* [HERITALISE D8.2 CRRS-014 (Extracted Historical Statement)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/historical-statement`

