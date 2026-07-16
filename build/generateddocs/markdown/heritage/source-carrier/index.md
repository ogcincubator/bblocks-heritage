
# Source Carrier (Schema)

`ogc.heritage.source-carrier` *v0.1*

A physical original document, photograph, drawing, negative or bound volume that carries heritage information, modelled as CIDOC-CRM E31 Document. Records the physical carrier from which digital surrogates are derived, with repository, archival code, and rights metadata.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Source Carrier

A physical original document, photograph, drawing, negative or bound volume that carries heritage
information, modelled as **CIDOC-CRM E31 Document**. This block records the *physical carrier* —
the original artefact held in an archive, library or collection — as distinct from any digital
copy derived from it.

The intended workflow is:
1. A `source-carrier` record describes the physical original.
2. An [`ogc.heritage.digital-surrogate`](../digital-surrogate/) record describes the digital copy
   and includes a mandatory `prov:wasDerivedFrom` link back to this source carrier.
3. An [`ogc.heritage.historical-statement`](../historical-statement/) can reference this carrier
   as its `sourceRecord` when extracting historical knowledge.

## CRM anchor

| Class | Role |
|-------|------|
| `crm:E31_Document` | The physical carrier as both a physical object and an information-bearing entity |

The JSON `type: "SourceCarrier"` token maps to `crm:E31_Document` via the JSON-LD context.

## Properties

| Property | JSON key | CRM / DCT mapping | Required |
|----------|----------|--------------------|----------|
| Type token | `type` | `@type` → `crm:E31_Document` | yes |
| Carrier type | `carrierType` | `crm:P2_has_type` (Getty AAT URI) | yes |
| Title | `title` | `crm:P102_has_title` | yes |
| Creator(s) | `creator` | `dct:creator` | no |
| Date / period | `datePeriod` | `crm:P4_has_time-span` | no |
| Language | `language` | `crm:P72_has_language` (BCP 47) | no |
| Repository | `repository` | `crm:P50_has_current_keeper` | no |
| Archival code | `archivalCode` | `crm:P1_is_identified_by` | no |
| Collection / series | `collection` | `dct:isPartOf` | no |
| Physical format | `physicalFormat` | `crm:P3_has_note` | no |
| Condition | `condition` | `crm:P44_has_condition` | no |
| Rights | `rights` | `dct:rights` | no |
| Documents object | `documentsObject` | `crm:P70_documents` | no |

## Design notes

- **E31 Document as dual nature** — In CIDOC-CRM, E31 Document is a subclass of both E22 Man-Made
  Object (it has physical existence) and E73 Information Object (it carries propositional content).
  This block exploits both: the physical attributes (format, condition, repository) and the
  documentary function (`P70_documents` the heritage subject).
- **`carrierType` as Getty AAT URI** — Use Getty AAT to distinguish carrier subtypes rather than a
  free text `subtype` field. Example URIs: manuscript (300265483), photograph (300046300),
  architectural drawing (300034787), photographic negative (300127173), bound volume (300028051).
- **`repository` as label or URI** — When the holding institution has a stable URI (ISNI, VIAF,
  Wikidata), prefer that. Literals are accepted for institutions without one.
- **`documentsObject` is optional** — The link from carrier to its heritage subject is provided
  where known, but archives often lack this attribution in their finding aids. The reverse direction
  (heritage object documented by this carrier) can also be expressed via the heritage object's own
  `prov:wasInfluencedBy` or through the `historical-statement` block.
- **Digital surrogates link back** — The forward link from source carrier to digital surrogate is
  not modelled here (to avoid circular dependency in the authoring order). The `digital-surrogate`
  block carries a mandatory `prov:wasDerivedFrom` pointing to the source carrier URI.

## Use cases

- **Venaria REQ-012** — Physical originals (drawings, plans, manuscripts) held in the Archivio di
  Stato di Torino and other repositories, from which digital surrogates were produced.
- **Malta MT-05** — Historical photographs of Villa Portelli (glass plates, prints) held in the
  Malta National Archives or private collections.
- **Malta MT-06** — Books, ledgers and archival documents from the villa's estate records.

## Examples

### Venaria — 18th-century architectural plan from the State Archive of Turin
{
  "type": "SourceCarrier",
  "id": "https://heritage.venaria.it/carriers/asto-venaria-mazzo12-n3",
  "carrierType": "http://vocab.getty.edu/aat/300034787",
  "title": "Pianta del piano nobile della Reggia di Venaria — Juvarra, c. 1716",
  "creator": ["https://vocab.getty.edu/ulan/500115568"],
  "datePeriod": "c. 1716",
  "language": "it",
  "repository": "Archivio di Stato di Torino",
  "archivalCode": "ASTo, Corte, Venaria Reale, mazzo 12, n. 3",
  "collection": "Corte — Venaria Reale",
  "physicalFormat": "58 × 82 cm, ink and wash on paper, rolled",
  "condition": "good — minor foxing on margins",
  "rights": "http://rightsstatements.org/vocab/InC-EDU/1.0/",
  "documentsObject": "https://heritage.venaria.it/buildings/galleria-grande"
}


### Malta — historic photograph of Villa Portelli main facade (c. 1920)
{
  "type": "SourceCarrier",
  "id": "https://heritage.gov.mt/carriers/mna-portelli-photo-1920-042",
  "carrierType": "http://vocab.getty.edu/aat/300046300",
  "title": "Villa Portelli main facade — exterior view, c. 1920",
  "datePeriod": "c. 1920",
  "repository": "Malta National Archives",
  "archivalCode": "MNA/PHO/1920/042",
  "collection": "Maltese Heritage Photography Collection",
  "physicalFormat": "13 × 18 cm silver gelatin print",
  "condition": "fair — surface scratches, slight yellowing",
  "rights": "http://rightsstatements.org/vocab/NoC-NC/1.0/",
  "documentsObject": "https://heritage.gov.mt/buildings/villa-portelli"
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Source Carrier
description: "A physical original document, photograph, drawing, negative or bound
  volume that carries heritage information (CIDOC-CRM E31 Document). Records the physical
  carrier \u2014 the entity from which digital surrogates (ogc.heritage.digital-surrogate)
  are derived \u2014 including repository location, archival reference, rights and
  physical condition."
type: object
required:
- type
- carrierType
- title
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this source carrier record.
  type:
    const: SourceCarrier
    description: Fixed type token identifying this record as a source carrier (maps
      to crm:E31_Document).
    x-jsonld-id: '@type'
  carrierType:
    type: string
    format: uri
    description: 'Getty AAT URI for the physical carrier type, e.g.: paper document
      (300264612), photograph (300046300), architectural drawing (300034787), photographic
      negative (300127173), bound volume (300028051).'
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
    x-jsonld-type: '@id'
  title:
    type: string
    description: Title or descriptive label of the physical carrier (crm:P102_has_title).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P102_has_title
  creator:
    oneOf:
    - type: string
      format: uri
    - type: array
      items:
        type: string
        format: uri
    description: URI(s) of the person(s) or organisation(s) who created the carrier
      (dct:creator).
    x-jsonld-id: http://purl.org/dc/terms/creator
    x-jsonld-type: '@id'
  datePeriod:
    type: string
    description: "Date or date range of creation (free text or ISO 8601, e.g. \"1742\",
      \"1890\u20131910\")."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
  language:
    type: string
    description: BCP 47 language code of the textual content, if applicable (e.g.
      "it", "en", "mt").
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P72_has_language
  repository:
    type: string
    description: Name or URI of the institution holding the physical carrier (crm:P50_has_current_keeper).
      Use a URI where available (e.g. ISNI, Wikidata).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P50_has_current_keeper
  archivalCode:
    type: string
    description: Archival reference code, call number or shelfmark assigned by the
      holding institution (crm:P1_is_identified_by), e.g. "ASTo, Corte, Venaria Reale,
      mazzo 12, n. 3".
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
  collection:
    type: string
    description: Collection or archival series to which this carrier belongs (dct:isPartOf).
    x-jsonld-id: http://purl.org/dc/terms/isPartOf
  physicalFormat:
    type: string
    description: "Physical dimensions, substrate and format description, e.g. \"44
      \xD7 62 cm, ink on paper, rolled\"."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  condition:
    type: string
    description: "Overall physical condition of the carrier, e.g. \"good\", \"fragile
      \u2014 foxing on margins\", \"damaged \u2014 lower third missing\" (crm:P44_has_condition)."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
  rights:
    type: string
    description: "Access rights or licence URI or label, e.g. a Creative Commons URI
      or \"Restricted \u2014 archive access only\" (dct:rights)."
    x-jsonld-id: http://purl.org/dc/terms/rights
  documentsObject:
    type: string
    format: uri
    description: "URI of the heritage-object, place or site that this carrier documents
      (crm:P70_documents). Optional \u2014 use when the primary subject is known."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70_documents
    x-jsonld-type: '@id'
x-jsonld-extra-terms:
  SourceCarrier: http://www.cidoc-crm.org/cidoc-crm/E31_Document
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/source-carrier/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/source-carrier/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "SourceCarrier": "crm:E31_Document",
    "type": "@type",
    "carrierType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "title": "crm:P102_has_title",
    "creator": {
      "@id": "dct:creator",
      "@type": "@id"
    },
    "datePeriod": "crm:P4_has_time-span",
    "language": "crm:P72_has_language",
    "repository": "crm:P50_has_current_keeper",
    "archivalCode": "crm:P1_is_identified_by",
    "collection": "dct:isPartOf",
    "physicalFormat": "crm:P3_has_note",
    "condition": "crm:P44_has_condition",
    "rights": "dct:rights",
    "documentsObject": {
      "@id": "crm:P70_documents",
      "@type": "@id"
    },
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dct": "http://purl.org/dc/terms/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/source-carrier/context.jsonld)

## Sources

* [CIDOC-CRM E31 Document](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E31)
* [ISAD(G) General International Standard Archival Description](https://www.ica.org/sites/default/files/CBPS_2000_Guidelines_ISAD%28G%29_Second-edition_EN.pdf)
* [HERITALISE D8.2 REQ-012 (Original Historical Source Carrier)](https://heritalise-eccch.eu/)
* [HERITALISE D8.2 MT-05 (Historic Photographs, Malta)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/source-carrier`

