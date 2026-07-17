
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
  "id": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "type": "HeritageObject",
  "identifier": "RV-GG-014",
  "title": "Allegorical ceiling painting, Great Gallery",
  "objectType": "http://vocab.getty.edu/aat/300033618",
  "material": [
    "http://vocab.getty.edu/aat/300014078"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/place/great-gallery"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "type": "HeritageObject",
  "identifier": "RV-GG-014",
  "title": "Allegorical ceiling painting, Great Gallery",
  "objectType": "http://vocab.getty.edu/aat/300033618",
  "material": [
    "http://vocab.getty.edu/aat/300014078"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/place/great-gallery"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Allegorical ceiling painting, Great Gallery" ;
    crm:P1_is_identified_by "RV-GG-014" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300033618> ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300014078> ;
    crm:P53_has_former_or_current_location <https://heritalise-eccch.eu/resource/place/great-gallery> .


```


### A minimal record with only the required properties
Only `identifier` and `title` are required — everything else can be added incrementally as it becomes available, which matches the legacy-archive migration scenario in UC-V-3.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/fishing-creel-whm-1923-45",
  "type": "HeritageObject",
  "identifier": "WHM-1923.45",
  "title": "Fishing creel"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/fishing-creel-whm-1923-45",
  "type": "HeritageObject",
  "identifier": "WHM-1923.45",
  "title": "Fishing creel"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://heritalise-eccch.eu/resource/object/fishing-creel-whm-1923-45> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Fishing creel" ;
    crm:P1_is_identified_by "WHM-1923.45" .


```


### Architectural component with parent space and materials (CRRS-004)
A load-bearing vault section within the Reggia di Venaria north gallery, typed as an architectural element (Getty AAT), with masonry and fresco materials and linked to its containing interior space via `parentSpace` (crm:P46i_forms_part_of). Covers CRRS-004 (architectural components of the built fabric).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3",
  "type": "HeritageObject",
  "identifier": "RV-ARCH-NV-003",
  "title": "North gallery vault, bay 3",
  "objectType": "http://vocab.getty.edu/aat/300002862",
  "material": [
    "http://vocab.getty.edu/aat/300010439",
    "http://vocab.getty.edu/aat/300014130"
  ],
  "parentSpace": "https://heritalise-eccch.eu/resource/space/north-gallery",
  "description": "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown."
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3",
  "type": "HeritageObject",
  "identifier": "RV-ARCH-NV-003",
  "title": "North gallery vault, bay 3",
  "objectType": "http://vocab.getty.edu/aat/300002862",
  "material": [
    "http://vocab.getty.edu/aat/300010439",
    "http://vocab.getty.edu/aat/300014130"
  ],
  "parentSpace": "https://heritalise-eccch.eu/resource/space/north-gallery",
  "description": "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown."
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://heritalise-eccch.eu/resource/object/north-vault-bay-3> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "North gallery vault, bay 3" ;
    crm:P1_is_identified_by "RV-ARCH-NV-003" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300002862> ;
    crm:P3_has_note "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown." ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300010439>,
        <http://vocab.getty.edu/aat/300014130> ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/space/north-gallery> .


```


### Movable heritage object with movement history and threshold link (CRRS-016, HM-04)
A historical tapestry from the Reggia di Venaria — a movable item catalogued with object type (Getty AAT), fibre materials, its current storage location, a sequence of PROV-O movement events, and a link to the applicable climate threshold record. Covers CRRS-016 (movable furniture / decorative objects) and HM-04 (Malta movable heritage items with environmental thresholds).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/venaria-tapestry-hunt-04",
  "type": "HeritageObject",
  "identifier": "RV-MOV-TAP-004",
  "title": "Royal Hunt tapestry no. 4",
  "objectType": "http://vocab.getty.edu/aat/300205002",
  "material": [
    "http://vocab.getty.edu/aat/300014224",
    "http://vocab.getty.edu/aat/300011727"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/space/storage-wing-b",
  "movementHistory": [
    "https://heritalise-eccch.eu/resource/event/tapestry-relocation-2021",
    "https://heritalise-eccch.eu/resource/event/tapestry-treatment-2019"
  ],
  "conservationThresholdLink": "https://heritalise-eccch.eu/resource/threshold/textile-climate-rh"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/venaria-tapestry-hunt-04",
  "type": "HeritageObject",
  "identifier": "RV-MOV-TAP-004",
  "title": "Royal Hunt tapestry no. 4",
  "objectType": "http://vocab.getty.edu/aat/300205002",
  "material": [
    "http://vocab.getty.edu/aat/300014224",
    "http://vocab.getty.edu/aat/300011727"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/space/storage-wing-b",
  "movementHistory": [
    "https://heritalise-eccch.eu/resource/event/tapestry-relocation-2021",
    "https://heritalise-eccch.eu/resource/event/tapestry-treatment-2019"
  ],
  "conservationThresholdLink": "https://heritalise-eccch.eu/resource/threshold/textile-climate-rh"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix prov: <http://www.w3.org/ns/prov#> .

<https://heritalise-eccch.eu/resource/object/venaria-tapestry-hunt-04> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Royal Hunt tapestry no. 4" ;
    crm:P1_is_identified_by "RV-MOV-TAP-004" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300205002> ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300011727>,
        <http://vocab.getty.edu/aat/300014224> ;
    crm:P53_has_former_or_current_location <https://heritalise-eccch.eu/resource/space/storage-wing-b> ;
    prov:wasUsedBy <https://heritalise-eccch.eu/resource/event/tapestry-relocation-2021>,
        <https://heritalise-eccch.eu/resource/event/tapestry-treatment-2019> .


```


### Garden sculpture / fountain with Getty AAT type (CRRS-021)
The Fountain of Diana in the Reggia di Venaria gardens, typed as a fountain (Getty AAT), constructed in stone, linked to the garden heritage site, and minted with a persistent W3ID. Covers CRRS-021 (garden sculptures and landscape objects).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/diana-fountain",
  "type": "HeritageObject",
  "identifier": "RV-GARD-FNT-001",
  "title": "Fountain of Diana, central garden axis",
  "objectType": "http://vocab.getty.edu/aat/300006858",
  "material": [
    "http://vocab.getty.edu/aat/300011443"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "persistentIdentifier": "https://w3id.org/heritalise/obj/RV-GARD-FNT-001"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/diana-fountain",
  "type": "HeritageObject",
  "identifier": "RV-GARD-FNT-001",
  "title": "Fountain of Diana, central garden axis",
  "objectType": "http://vocab.getty.edu/aat/300006858",
  "material": [
    "http://vocab.getty.edu/aat/300011443"
  ],
  "currentLocation": "https://heritalise-eccch.eu/resource/site/reggia-di-venaria",
  "persistentIdentifier": "https://w3id.org/heritalise/obj/RV-GARD-FNT-001"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://heritalise-eccch.eu/resource/object/diana-fountain> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Fountain of Diana, central garden axis" ;
    crm:P1_is_identified_by <https://w3id.org/heritalise/obj/RV-GARD-FNT-001>,
        "RV-GARD-FNT-001" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300006858> ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300011443> ;
    crm:P53_has_former_or_current_location <https://heritalise-eccch.eu/resource/site/reggia-di-venaria> .


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
- id
- identifier
- title
x-jsonld-extra-terms:
  HeritageObject: http://www.cidoc-crm.org/cidoc-crm/E22_Man-Made_Object
  parentSpace:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P46i_forms_part_of
    x-jsonld-type: '@id'
  movementHistory:
    x-jsonld-id: http://www.w3.org/ns/prov#wasUsedBy
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritageObject": "crm:E22_Man-Made_Object",
    "parentSpace": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "movementHistory": {
      "@id": "prov:wasUsedBy",
      "@type": "@id",
      "@container": "@set"
    },
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
    "prov": "http://www.w3.org/ns/prov#",
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

