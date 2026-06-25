
# Heritage Object (Schema)

`ogc.heritage.heritage-object` *v0.1*

A physical cultural heritage item — an artwork, building element, garden feature or museum object — modelled as a CIDOC-CRM E22 Man-Made Object.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Heritage Object

A `HeritageObject` is the physical cultural heritage item itself — a painting, a building
element, a garden feature, a museum artefact — modelled as a CIDOC-CRM `E22_Man-Made_Object`.

It is deliberately minimal: identity (`identifier`, `title`), typing and material via Getty AAT
URIs, current location, and an optional persistent identifier. Everything else attaches to it by
reference:

- digital assets (images, 3D models, documents) point back to it via `isAbout` in
  [`digital-representation`](../digital-representation)
- sensor observations point to it (or a `place` derived from it) as their feature of interest
- provenance, events (production, restoration) and actors are modelled as separate, linked
  entities rather than embedded here, so the same object can accumulate history over time without
  growing an unbounded record.

## Examples

### A catalogued painting with type, material and location
A ceiling painting from the Reggia di Venaria's Great Gallery, typed and materialised with Getty AAT concepts and linked to a place.
#### json
```json
{
  "id": "https://example.org/heritalise/object/great-gallery-painting-014",
  "type": "HeritageObject",
  "identifier": "RV-GG-014",
  "title": "Allegorical ceiling painting, Great Gallery",
  "objectType": "http://vocab.getty.edu/aat/300033618",
  "material": [
    "http://vocab.getty.edu/aat/300014078"
  ],
  "currentLocation": "https://example.org/heritalise/place/great-gallery"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://example.org/heritalise/object/great-gallery-painting-014",
  "type": "HeritageObject",
  "identifier": "RV-GG-014",
  "title": "Allegorical ceiling painting, Great Gallery",
  "objectType": "http://vocab.getty.edu/aat/300033618",
  "material": [
    "http://vocab.getty.edu/aat/300014078"
  ],
  "currentLocation": "https://example.org/heritalise/place/great-gallery"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://example.org/heritalise/object/great-gallery-painting-014> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Allegorical ceiling painting, Great Gallery" ;
    crm:P1_is_identified_by "RV-GG-014" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300033618> ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300014078> ;
    crm:P53_has_former_or_current_location <https://example.org/heritalise/place/great-gallery> .


```


### A minimal record with only the required properties
Only `identifier` and `title` are required — everything else can be added incrementally as it becomes available, which matches the legacy-archive migration scenario in UC-V-3.
#### json
```json
{
  "type": "HeritageObject",
  "identifier": "WHM-1923.45",
  "title": "Fishing creel"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "type": "HeritageObject",
  "identifier": "WHM-1923.45",
  "title": "Fishing creel"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

[] a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Fishing creel" ;
    crm:P1_is_identified_by "WHM-1923.45" .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Heritage Object
description: 'A physical cultural heritage item (artwork, building element, garden
  feature, museum object...),

  modelled as a CIDOC-CRM E22 Man-Made Object.

  '
type: object
properties:
  id:
    type: string
    format: uri
    description: The persistent or local URI identifying this object.
    x-jsonld-id: '@id'
  type:
    const: HeritageObject
    x-jsonld-id: '@type'
  identifier:
    type: string
    description: A local catalogue or inventory number (CIDOC-CRM P1_is_identified_by).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
  title:
    type: string
    description: A human-readable title or name for the object (CIDOC-CRM P102_has_title).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P102_has_title
  description:
    type: string
    description: Free-text description or curatorial note (CIDOC-CRM P3_has_note).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  objectType:
    type: string
    format: uri
    description: The type of object, typically a Getty AAT concept URI (CIDOC-CRM
      P2_has_type).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
    x-jsonld-type: '@id'
  material:
    type: array
    description: Materials the object consists of, typically Getty AAT concept URIs
      (CIDOC-CRM P45_consists_of).
    items:
      type: string
      format: uri
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P45_consists_of
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  currentLocation:
    type: string
    format: uri
    description: URI of the place where the object is currently or was last located
      (CIDOC-CRM P53_has_former_or_current_location).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P53_has_former_or_current_location
    x-jsonld-type: '@id'
  persistentIdentifier:
    type: string
    format: uri
    description: A persistent identifier (e.g. ARK, DOI, Handle) minted for this object.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    x-jsonld-type: '@id'
required:
- identifier
- title
x-jsonld-extra-terms:
  HeritageObject: http://www.cidoc-crm.org/cidoc-crm/E22_Man-Made_Object
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritageObject": "crm:E22_Man-Made_Object",
    "id": "@id",
    "type": "@type",
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
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld)

## Sources

* [CIDOC-CRM](https://www.cidoc-crm.org/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/heritage-object`

