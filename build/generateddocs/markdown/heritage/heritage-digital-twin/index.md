
# Heritage Digital Twin (Schema)

`ogc.heritage.heritage-digital-twin` *v0.1*

The ECCCH Heritage Digital Twin Ontology's HC2 Heritage Digital Twin: the aggregate of formal proposition sets documenting one heritage entity, identified by its propositional content and creating actor rather than by instance identity.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Heritage Digital Twin

`HeritageDigitalTwin` is HDTO's **HC2 Heritage Digital Twin** (ECCCH's Heritage Digital Twin
Ontology, D7.1 v1.0 §4.1) — the part of HDTO with no prior analogue anywhere in
`bblocks-heritage`. Per D7.1's own worked example (the Deryneia icon, §3), an HC2 node is
lightweight: identified by its propositional content and creating actor, not by a rich internal
structure of its own. Updating a digital twin doesn't erase its previous state — see
`containedPropositionSet` below.

The actual content lives in one or more [`heritage-proposition-set`](../heritage-proposition-set)
(HC16) instances this block points to via `containsPropositionSet`/`containedPropositionSet`.

### Not `aggregation`

This block is deliberately **not** a profile of, or a replacement for, `aggregation` (EDM/ORE
Aggregation): D7.1 gives no crosswalk between EDM/ORE and any HC class anywhere in the document
(checked explicitly). `aggregation` remains the separate Europeana/ECCCH-portal display-record
construct it already was; `heritage-digital-twin` is the KB-facing HDTO construct. If a
relationship between the two turns out to be useful later, that's an additive property to add
once both exist, not a prerequisite.

### How it's built (HC11/HC13)

An HC2 instance is composed/maintained by an **HC11 Digital Twin Maintenance** activity
(`composedBy`, HP19 has composed, inverse direction), typically running under an **HC13 Project**.
Rather than two more new blocks, HC11 and HC13 are modelled by co-typing the existing
[`event`](../event) block — see its own "HDTO alignment" section for how.

### Property reference

| Property | HDTO property | Notes |
|---|---|---|
| `ofHeritageEntity` | HP1i is digital twin of (inverse of HP1 has digital twin) | Required. Points back to the HC1/HC3 entity — in practice, any HC3-co-typed `heritage-object`/`heritage-site`/`building`/`heritage-object-feature` instance in this register. |
| `label` | — | Human-readable label, per D7.1's own worked-example convention (rdfs:label). |
| `creator` | — | The actor who created/maintains this digital twin (prov:wasAttributedTo). |
| `containsPropositionSet` | HP33 contains | Required, ≥1. Currently valid content. |
| `containedPropositionSet` | HP34 contained | Superseded content — the versioning mechanism: an update moves a `heritage-proposition-set` URI here instead of deleting it. |
| `composedBy` | HP19i was composed by (inverse of HP19 has composed) | The `event` (co-typed HC11) that produced/last updated this twin. |

### `crmpem:` namespace — provisional

D7.1 declares HC2 ⊑ HC14 Volatile Digital Object ≡ `crmpem:PE20`, but never states a resolvable
namespace URI for `crmpem:` — not in D7.1 itself, not on the live HDTO ontology page, and not in
the underlying PARTHENOS Entities v3.1 model-description document (the source D7.1 cites).
`ontology.ttl` asserts `hdto:HC2_Heritage_Digital_Twin owl:equivalentClass crmpem:PE20` using
`http://www.ics.forth.gr/isl/CRMext/CRMpe.rdfs/` as a **provisional** namespace URI (the
FORTH-ISL "CRMpe" extension, following the same URI convention this register already uses for
`crmdig:`). This is flagged, not resolved — see
[ogcincubator/bblocks-heritage#1](https://github.com/ogcincubator/bblocks-heritage/issues/1) for
the open question. Do not treat this URI as confirmed.

### Scope note

This block does not attempt to model HC14/HC15 (Volatile/Persistent Digital Object) as their own
schema — per `eccch-integration/hdto/03-digital-twin-infrastructure.md`, these are infrastructure
classes, not content-bearing blocks in their own right. HP28 (has snapshot)/HP29 (has digital
object part), which relate HC14/HC15/crmdig:D1, are likewise out of scope here — revisit only if
a concrete pilot requirement needs versioned digital-object snapshotting modelled explicitly.

## Examples

### A Venaria painting's digital twin, freshly composed
The HDT of the Great Gallery ceiling painting (`heritage-object` HC3 instance), with a single current proposition set and the HC11-co-typed `event` that composed it.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/hdt/great-gallery-painting-014",
  "type": "HeritageDigitalTwin",
  "ofHeritageEntity": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "label": "The HDT of the Great Gallery ceiling painting, Reggia di Venaria Reale",
  "creator": "https://heritalise-eccch.eu/resource/actor/giulia-bianchi",
  "containsPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-digital-twin/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/hdt/great-gallery-painting-014",
  "type": "HeritageDigitalTwin",
  "ofHeritageEntity": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "label": "The HDT of the Great Gallery ceiling painting, Reggia di Venaria Reale",
  "creator": "https://heritalise-eccch.eu/resource/actor/giulia-bianchi",
  "containsPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026"
}
```

#### ttl
```ttl
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

<https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026> hdto:HP19_has_composed <https://heritalise-eccch.eu/resource/hdt/great-gallery-painting-014> .

<https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> hdto:HP1_has_digital_twin <https://heritalise-eccch.eu/resource/hdt/great-gallery-painting-014> .

<https://heritalise-eccch.eu/resource/hdt/great-gallery-painting-014> a hdto:HC2_Heritage_Digital_Twin ;
    rdfs:label "The HDT of the Great Gallery ceiling painting, Reggia di Venaria Reale" ;
    hdto:HP33_contains <https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1> ;
    prov:wasAttributedTo <https://heritalise-eccch.eu/resource/actor/giulia-bianchi> .


```


### A Malta building's digital twin with version history
The HDT of Villa Portelli's main building, showing HC2 versioning: the current content (`containsPropositionSet`) plus one superseded proposition set preserved in `containedPropositionSet` for referential integrity, per HP33/HP34.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/hdt/villa-portelli-main",
  "type": "HeritageDigitalTwin",
  "ofHeritageEntity": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "label": "The HDT of Villa Portelli's main building",
  "containsPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2"
  ],
  "containedPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1"
  ]
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-digital-twin/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/hdt/villa-portelli-main",
  "type": "HeritageDigitalTwin",
  "ofHeritageEntity": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "label": "The HDT of Villa Portelli's main building",
  "containsPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2"
  ],
  "containedPropositionSet": [
    "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1"
  ]
}
```

#### ttl
```ttl
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

<https://heritalise-eccch.eu/resource/building/villa-portelli-main> hdto:HP1_has_digital_twin <https://heritalise-eccch.eu/resource/hdt/villa-portelli-main> .

<https://heritalise-eccch.eu/resource/hdt/villa-portelli-main> a hdto:HC2_Heritage_Digital_Twin ;
    rdfs:label "The HDT of Villa Portelli's main building" ;
    hdto:HP33_contains <https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2> ;
    hdto:HP34_contained <https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Heritage Digital Twin
description: "HDTO's HC2 Heritage Digital Twin: the aggregate of formal proposition
  sets documenting one\nheritage entity (an `hdto:HC1_Heritage_Entity` \u2014 in this
  register, any HC3-co-typed\n`heritage-object`/`heritage-site`/`building`/`heritage-object-feature`
  instance). D7.1's own\nworked example (the Deryneia icon) shows HC2 is a lightweight
  node identified by its\npropositional content and creating actor, not a rich record
  in its own right \u2014 the actual\ncontent lives in one or more `heritage-proposition-set`
  (HC16) instances it points to.\n\nThis is deliberately **not** modelled as a profile
  of `aggregation` (EDM/ORE Aggregation):\nD7.1 gives no crosswalk between EDM/ORE
  and any HC class \u2014 `aggregation` remains the separate\nEuropeana/ECCCH-portal
  display-record construct it already was.\n"
type: object
required:
- id
- type
- ofHeritageEntity
- containsPropositionSet
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this Heritage Digital Twin.
    x-jsonld-id: '@id'
  type:
    const: HeritageDigitalTwin
    description: Fixed type token (maps to hdto:HC2_Heritage_Digital_Twin).
    x-jsonld-id: '@type'
  ofHeritageEntity:
    type: string
    format: uri
    description: "URI of the heritage entity this is the digital twin of (HP1 has
      digital twin, referenced here in its inverse \"is digital twin of\" direction
      \u2014 HP1i)."
    x-jsonld-reverse: hdto:HP1_has_digital_twin
    x-jsonld-type: '@id'
  label:
    type: string
    description: Short human-readable label identifying this digital twin (e.g. "The
      HDT of the Great Gallery ceiling painting"), per D7.1's own worked example convention.
    x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
  creator:
    type: string
    format: uri
    description: URI of the actor (person or organisation) that created/maintains
      this digital twin.
    x-jsonld-id: http://www.w3.org/ns/prov#wasAttributedTo
    x-jsonld-type: '@id'
  containsPropositionSet:
    type: array
    minItems: 1
    items:
      type: string
      format: uri
    description: URIs of `heritage-proposition-set` instances currently valid as content
      of this digital twin (HP33 contains).
    x-jsonld-id: http://isl.ics.forth.gr/ontology/echoes/HP33_contains
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  containedPropositionSet:
    type: array
    items:
      type: string
      format: uri
    description: "URIs of `heritage-proposition-set` instances formerly, but no longer,
      valid content (HP34 contained) \u2014 preserves referential integrity for prior
      scholarly references. This is how HC2 versioning works: superseding content
      moves from `containsPropositionSet` to here rather than being deleted."
    x-jsonld-id: http://isl.ics.forth.gr/ontology/echoes/HP34_contained
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  composedBy:
    type: string
    format: uri
    description: "URI of the `event` instance (co-typed hdto:HC11_Digital_Twin_Maintenance
      \u2014 see ogc.heritage.event's HDTO alignment note) that composed/most recently
      updated this digital twin (HP19 has composed, inverse direction \u2014 HP19i)."
    x-jsonld-reverse: hdto:HP19_has_composed
    x-jsonld-type: '@id'
x-jsonld-extra-terms:
  HeritageDigitalTwin: http://isl.ics.forth.gr/ontology/echoes/HC2_Heritage_Digital_Twin
x-jsonld-prefixes:
  hdto: http://isl.ics.forth.gr/ontology/echoes/
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-digital-twin/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-digital-twin/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritageDigitalTwin": "hdto:HC2_Heritage_Digital_Twin",
    "id": "@id",
    "type": "@type",
    "ofHeritageEntity": {
      "@reverse": "hdto:HP1_has_digital_twin",
      "@type": "@id"
    },
    "label": "rdfs:label",
    "creator": {
      "@id": "prov:wasAttributedTo",
      "@type": "@id"
    },
    "containsPropositionSet": {
      "@id": "hdto:HP33_contains",
      "@type": "@id",
      "@container": "@set"
    },
    "containedPropositionSet": {
      "@id": "hdto:HP34_contained",
      "@type": "@id",
      "@container": "@set"
    },
    "composedBy": {
      "@reverse": "hdto:HP19_has_composed",
      "@type": "@id"
    },
    "hdto": "http://isl.ics.forth.gr/ontology/echoes/",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "prov": "http://www.w3.org/ns/prov#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-digital-twin/context.jsonld)

## Sources

* [ECHOES D7.1 "The Digital Commons" v1.0 (HDTO)](https://zenodo.org/records/20445938)
* [HDTO live ontology page](https://isl.ics.forth.gr/ontology/echoes/html/HDT_v1.1.html)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/heritage-digital-twin`

