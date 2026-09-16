
# Heritage Proposition Set (Schema)

`ogc.heritage.heritage-proposition-set` *v0.1*

The ECCCH Heritage Digital Twin Ontology's HC16 Heritage Proposition Set: a named-graph content unit — what an HC2 Heritage Digital Twin actually contains, referencing the rest of this register's HDTO-typed content.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Heritage Proposition Set

`HeritagePropositionSet` is HDTO's **HC16 Heritage Proposition Set** (⊑ `crminf:I4 Proposition
Set` + `HC15 Persistent Digital Object`) — the named-graph content unit a
[`heritage-digital-twin`](../heritage-digital-twin) (HC2) actually *contains*, via HP33 (current
content) / HP34 (superseded content, for versioning).

### Deliberately flat: what's modelled here vs. D7.1's full worked example

D7.1's own worked example (the Deryneia icon, §3 Figures 16-17) shows a real HC16 instance is
**not a flat metadata bag** — it decomposes into a provenance-linked process chain per
digitization/study method: a `D2 Digitization Process` → `D14 Software`/`D6 Digital Device` →
`D9 Data Object` (or `HC7 Digital Audiovisual Object`) output, sometimes continuing into an
`HC17 Observation with Inference` node for interpretive conclusions.

Modelling that full chain is **out of scope for this pass** — it would need its own set of
CRMdig-anchored properties (L1/L23/L12/L20/L10 and others, several of which are still flagged
unconfirmed in `eccch-integration/hdto/09-worked-example-deryneia.md` §7) and is exactly the kind
of speculative authoring this register's own conventions avoid without a concrete pilot
requirement behind it (the same reasoning already applied to deferring HC9/HC10/HC12/HC17-HC20).
This schema instead gives HC16 a flat `content` property (`crm:P148_has_component`) referencing
the register entities the set's assertions are about or derived from — enough to make HC2/HC16
usable end-to-end now. **Extension point:** when a concrete pilot requirement needs the deeper
process chain, model it as a `$ref`/`allOf` composition adding a `process` property (or similar)
alongside `content`, rather than redesigning this block — `heritage-proposition-set`'s own worked
example in `09-worked-example-deryneia.md` §5 is the reference to build that against.

### Property reference

| Property | HDTO property | Notes |
|---|---|---|
| `content` | — (modelled as crm:P148_has_component, a flat simplification — see above) | Required, ≥1. What this proposition set's assertions are about/derived from. |
| `composedBy` | HP30i content was added by (inverse of HP30 added content) | Required. The `event` (co-typed HC11) that produced this set. |
| `replaces` | HP32 replaced | The prior `heritage-proposition-set` this one supersedes, within the same digital twin's version chain. |

### `crmpem:` lineage (inherited from HC15, not asserted directly here)

HC16's own parent, HC15 Persistent Digital Object, is declared by D7.1 as ≡ `crmpem:PE19`. This
block doesn't assert that equivalence directly (HC16 itself has no `crmpem:` equivalence in
D7.1 — only HC15/HC14 do), but see
[`heritage-digital-twin`](../heritage-digital-twin)'s own `ontology.ttl` and description for the
`crmpem:` namespace's provisional status and
[ogcincubator/bblocks-heritage#1](https://github.com/ogcincubator/bblocks-heritage/issues/1).

## Examples

### Photographic and 3D documentation batch (Venaria)
A proposition set aggregating a photograph and a 3D model of the Great Gallery ceiling painting, composed by the same HC11-co-typed `event` referenced from that entity's `heritage-digital-twin` example.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1",
  "type": "HeritagePropositionSet",
  "label": "Photographic and 3D documentation batch, Great Gallery ceiling painting, 2026",
  "content": [
    "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
    "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-model-01"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-proposition-set/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1",
  "type": "HeritagePropositionSet",
  "label": "Photographic and 3D documentation batch, Great Gallery ceiling painting, 2026",
  "content": [
    "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
    "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-model-01"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

<https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026> hdto:HP30_added_content <https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1> .

<https://heritalise-eccch.eu/resource/hps/great-gallery-painting-014-1> a hdto:HC16_Heritage_Proposition_Set ;
    rdfs:label "Photographic and 3D documentation batch, Great Gallery ceiling painting, 2026" ;
    crm:P148_has_component <https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-model-01>,
        <https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01> .


```


### A re-documentation batch superseding an earlier one (Malta)
The current proposition set for Villa Portelli's main building, referencing a UAV survey dataset and declaring `replaces` against the proposition set it supersedes — demonstrates HP32 alongside `heritage-digital-twin`'s own HP33/HP34 versioning example.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2",
  "type": "HeritagePropositionSet",
  "label": "UAV survey and deviation-map re-documentation batch, Villa Portelli main building, 2026",
  "content": [
    "https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-villa-portelli-main-2026",
  "replaces": "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-proposition-set/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2",
  "type": "HeritagePropositionSet",
  "label": "UAV survey and deviation-map re-documentation batch, Villa Portelli main building, 2026",
  "content": [
    "https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025"
  ],
  "composedBy": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-villa-portelli-main-2026",
  "replaces": "https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

<https://heritalise-eccch.eu/resource/event/hdt-maintenance-villa-portelli-main-2026> hdto:HP30_added_content <https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2> .

<https://heritalise-eccch.eu/resource/hps/villa-portelli-main-2> a hdto:HC16_Heritage_Proposition_Set ;
    rdfs:label "UAV survey and deviation-map re-documentation batch, Villa Portelli main building, 2026" ;
    hdto:HP32_replaced <https://heritalise-eccch.eu/resource/hps/villa-portelli-main-1> ;
    crm:P148_has_component <https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Heritage Proposition Set
description: "HDTO's HC16 Heritage Proposition Set (\u2291 crminf:I4 Proposition Set
  + HC15 Persistent Digital\nObject): the named-graph content unit a `heritage-digital-twin`
  (HC2) actually \"contains\" via\nHP33/HP34. Per D7.1's own worked example (Figures
  16-17), a real HC16 instance is a\nprovenance-linked graph (digitization/study process
  \u2192 software/device \u2192 data output), not a\nflat metadata bag \u2014 modelling
  that full CRMdig process chain is deliberately deferred (see\n`description.md`);
  this schema covers the flat \"what does this set assert, and about what\" shape\nneeded
  to make HC2 usable now, with `content` as the extension point for deeper chains
  later.\n"
type: object
required:
- id
- type
- content
- composedBy
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this proposition set (named graph).
    x-jsonld-id: '@id'
  type:
    const: HeritagePropositionSet
    description: Fixed type token (maps to hdto:HC16_Heritage_Proposition_Set).
    x-jsonld-id: '@type'
  label:
    type: string
    description: Short human-readable label for this proposition set, e.g. naming
      the digitization/study batch it captures.
    x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
  content:
    type: array
    minItems: 1
    items:
      type: string
      format: uri
    description: "URIs of the register entities this proposition set's assertions
      are about or derived from (e.g. `digital-representation`/`three-d-model`/`survey-dataset`/`observation`
      instances it aggregates as evidence for the documented `heritage-object`). Modelled
      as a flat reference list (crm:P148_has_component) \u2014 the deeper process/software/device
      provenance chain D7.1's worked example shows (D2 Digitization Process, D14 Software,
      D6 Digital Device, D9 Data Object) is not modelled here; see `description.md`."
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P148_has_component
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  composedBy:
    type: string
    format: uri
    description: "URI of the `event` instance (co-typed hdto:HC11_Digital_Twin_Maintenance)
      that added this proposition set (HP30 added content, inverse direction \u2014
      HP30i). Required: every proposition set exists because some maintenance activity
      produced it."
    x-jsonld-reverse: hdto:HP30_added_content
    x-jsonld-type: '@id'
  replaces:
    type: string
    format: uri
    description: URI of the `heritage-proposition-set` this one supersedes within
      the same digital twin's version chain (HP32 replaced).
    x-jsonld-id: http://isl.ics.forth.gr/ontology/echoes/HP32_replaced
    x-jsonld-type: '@id'
x-jsonld-extra-terms:
  HeritagePropositionSet: http://isl.ics.forth.gr/ontology/echoes/HC16_Heritage_Proposition_Set
x-jsonld-prefixes:
  hdto: http://isl.ics.forth.gr/ontology/echoes/
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-proposition-set/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-proposition-set/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritagePropositionSet": "hdto:HC16_Heritage_Proposition_Set",
    "id": "@id",
    "type": "@type",
    "label": "rdfs:label",
    "content": {
      "@id": "crm:P148_has_component",
      "@type": "@id",
      "@container": "@set"
    },
    "composedBy": {
      "@reverse": "hdto:HP30_added_content",
      "@type": "@id"
    },
    "replaces": {
      "@id": "hdto:HP32_replaced",
      "@type": "@id"
    },
    "hdto": "http://isl.ics.forth.gr/ontology/echoes/",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-proposition-set/context.jsonld)

## Sources

* [ECHOES D7.1 "The Digital Commons" v1.0 (HDTO)](https://zenodo.org/records/20445938)
* [HDTO live ontology page](https://isl.ics.forth.gr/ontology/echoes/html/HDT_v1.1.html)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/heritage-proposition-set`

