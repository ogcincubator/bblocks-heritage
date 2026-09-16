
# Digital Representation (Schema)

`ogc.heritage.digital-representation` *v0.3*

A digital asset (image, 3D model, document...) representing or documenting a heritage object, modelled as a CIDOC-CRM E73 Information Object and profiling the PROV-O Entity.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Digital Representation

A `DigitalRepresentation` is a digital asset — an image, a 3D model, a scanned document — that
represents or documents a heritage object, modelled as a CIDOC-CRM `E73_Information_Object`.

Like `actor` and `event`, this block profiles a PROV-O class: a `DigitalRepresentation` *is* a
PROV-O `Entity`, so its provenance chain (`wasAttributedTo`, `wasGeneratedBy`, `wasDerivedFrom`)
comes for free from `ogc.ogc-utils.prov-entity`. This block adds `isAbout` (a required link back
to the [`heritage-object`](../heritage-object) it represents), `mediaType` and an optional
`persistentIdentifier`.

`DigitalRepresentation` is deliberately generic — it covers any digital asset type. The three
format-specific profiles constrain it for a particular kind of asset:

- [`three-d-model`](../three-d-model) — glTF / 3D Tiles / OBJ 3D models (profile B)
- [`archival-document`](../archival-document) — PDF/A and LIDO/XML documents with a mandatory
  persistent identifier (profile C)
- [`fabrication-output`](../fabrication-output) — STL/3MF outputs with a mandatory PROV
  derivation chain back to their source 3D model (profile F)

## HDTO alignment

Every instance requires two co-types, both mapping via `context.jsonld` directly to `rdf:type` —
asserted on a plain JSON-LD parse, no post-processing step needed:

- `crmdigType`, a fixed `const` of `crmdig:D9_Data_Object`. HDTO's own OGC SensorThings crosswalk
  table (D7.1 Table 1, p.28) names `crmdig:D9_Data_Object`, not `crm:E73_Information_Object` (this
  block's `bblock.json` `rdfType` metadata), as HC5's actual CRM anchor — this block's instances
  carried no CIDOC-CRM class triple at all before this (the real uplifted type was only
  `prov:Entity`, inherited from the PROV-O Entity profile), so D9 is now asserted directly rather
  than left as documentation-only metadata.
- `hdtoType`, required with an `enum` of `hdto:HC5_Digital_Representation` /
  `hdto:HC7_Digital_Audiovisual_Object` / `hdto:HC8_3D_Model` (HC7/HC8 ⊑ HC5 per D7.1).
  `oral-history` and `three-d-model` narrow this to their own specific HC7/HC8 `const` via their
  own schema; a plain `digital-representation` or `archival-document` instance defaults to HC5.

**Cross-block D9 collision, resolved:** `derived-survey-product` also profiles this block and
previously targeted `crmdig:D9_Data_Object` exclusively via `sh:targetClass` in its own
`shapes.shacl`, on the assumption that no sibling block asserted D9. Now that every
digital-representation-family instance does, `derived-survey-product`'s shape was switched to
`sh:targetSubjectsOf crmdig:L11i_was_output_of` (a predicate unique to that block) so its
processing-specific constraints don't false-positive on a plain photo or 3D model that also
happens to carry the shared D9 co-type — see its own `shapes.shacl` and the register's established
convention for this exact situation (`feedback_bblocks_authoring` memory).

`archival-document` is co-typed HC5 too (see its own description); `fabrication-output` is
deliberately left unassigned — it produces a *physical* 3D-printed artifact from digital input, so
it may belong in the HC3 tangible family instead of HC5, and that's a decision for whoever owns
that pilot requirement, not OGC alone.

## Examples

### A documentation photograph with provenance and a persistent identifier
A photograph of the Great Gallery ceiling painting, attributed to the photographer and minted with a DOI.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
  "identifier": "RV-GG-014-PHOTO-01",
  "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "mediaType": "image/jpeg",
  "wasAttributedTo": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "persistentIdentifier": "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01",
  "crmdigType": "crmdig:D9_Data_Object",
  "hdtoType": "hdto:HC5_Digital_Representation"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
  "identifier": "RV-GG-014-PHOTO-01",
  "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
  "mediaType": "image/jpeg",
  "wasAttributedTo": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "persistentIdentifier": "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01",
  "crmdigType": "crmdig:D9_Data_Object",
  "hdtoType": "hdto:HC5_Digital_Representation"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.cidoc-crm.org/extensions/crmdig/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix prov: <http://www.w3.org/ns/prov#> .

<https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01> a hdto:HC5_Digital_Representation,
        crmdig:D9_Data_Object ;
    dct:format "image/jpeg" ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> ;
    crm:P1_is_identified_by "RV-GG-014-PHOTO-01",
        "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01" ;
    prov:wasAttributedTo <https://heritalise-eccch.eu/resource/actor/giulia-bianchi> .


```


### A minimal record with only the required `isAbout` link
Only `id` and `isAbout` are required — media type, attribution and persistent identifiers can be added incrementally as they become available.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/digital/fishing-creel-photo-01",
  "isAbout": "https://heritalise-eccch.eu/resource/object/fishing-creel",
  "crmdigType": "crmdig:D9_Data_Object",
  "hdtoType": "hdto:HC5_Digital_Representation"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/digital/fishing-creel-photo-01",
  "isAbout": "https://heritalise-eccch.eu/resource/object/fishing-creel",
  "crmdigType": "crmdig:D9_Data_Object",
  "hdtoType": "hdto:HC5_Digital_Representation"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.cidoc-crm.org/extensions/crmdig/> .
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .

<https://heritalise-eccch.eu/resource/digital/fishing-creel-photo-01> a hdto:HC5_Digital_Representation,
        crmdig:D9_Data_Object ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/object/fishing-creel> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Digital Representation
description: 'A digital asset (image, 3D model, document...) representing or documenting
  a heritage object,

  modelled as a CIDOC-CRM E73 Information Object. Profiles PROV-O''s Entity: `wasAttributedTo`,

  `wasGeneratedBy` and `wasDerivedFrom` carry its provenance chain. This block adds
  `isAbout`

  (the heritage object it represents), a media type and an optional persistent identifier.

  '
$defs:
  properties:
    type: object
    properties:
      identifier:
        type: string
        description: A local identifier for this digital asset (CIDOC-CRM P1_is_identified_by).
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
      isAbout:
        type: string
        format: uri
        description: URI of the heritage object (or other CRM entity) this digital
          asset represents or documents (CIDOC-CRM P129_is_about).
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P129_is_about
        x-jsonld-type: '@id'
      mediaType:
        type: string
        description: The IANA media type of the asset, e.g. `image/jpeg` or `model/gltf-binary`
          (dct:format).
        x-jsonld-id: http://purl.org/dc/terms/format
      persistentIdentifier:
        type: string
        format: uri
        description: A persistent identifier (e.g. ARK, DOI, Handle) minted for this
          asset.
        x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
      url:
        type: string
        format: uri
        description: URL where this digital asset can be accessed or downloaded (dcat:accessURL).
        x-jsonld-id: http://www.w3.org/ns/dcat#accessURL
        x-jsonld-type: '@id'
      crmdigType:
        type: string
        const: crmdig:D9_Data_Object
        description: "Fixed co-type, shared by the whole digital-representation family.
          Maps via context directly to rdf:type. D7.1's own OGC SensorThings crosswalk
          table (Table 1, p.28) names crmdig:D9_Data_Object, not crm:E73_Information_Object,
          as HDTO's actual HC5 anchor \u2014 asserted here alongside (not instead
          of) whatever CRM/PROV-O typing this instance already carries. Required on
          every instance of this block and its profiles."
        x-jsonld-id: http://www.w3.org/1999/02/22-rdf-syntax-ns#type
        x-jsonld-type: '@id'
      hdtoType:
        type: string
        format: uri
        enum:
        - hdto:HC5_Digital_Representation
        - hdto:HC7_Digital_Audiovisual_Object
        - hdto:HC8_3D_Model
        default: hdto:HC5_Digital_Representation
        description: "HDTO co-type (D7.1). Maps via context directly to rdf:type.
          Required on every instance; value must be HC5 (the family default \u2014
          used when this block is not further profiled) or one of HC5's own HDTO subclasses
          HC7/HC8, which `oral-history`/`three-d-model` narrow to via their own fixed
          `const`."
        x-jsonld-id: http://www.w3.org/1999/02/22-rdf-syntax-ns#type
        x-jsonld-type: '@id'
    required:
    - isAbout
    - crmdigType
    - hdtoType
allOf:
- $ref: https://ogcincubator.github.io/bblock-prov-schema/build/annotated/ogc-utils/prov-entity/schema.yaml
- $ref: '#/$defs/properties'
x-jsonld-prefixes:
  rdf: http://www.w3.org/1999/02/22-rdf-syntax-ns#
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/
  dcat: http://www.w3.org/ns/dcat#
  crmdig: http://www.cidoc-crm.org/extensions/crmdig/
  hdto: http://isl.ics.forth.gr/ontology/echoes/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "wasInfluencedBy": {
      "@id": "prov:wasInfluencedBy",
      "@type": "@id"
    },
    "qualifiedInfluence": {
      "@id": "prov:qualifiedInfluence",
      "@type": "@id"
    },
    "hadMember": {
      "@id": "prov:hadMember",
      "@type": "@id"
    },
    "id": "@id",
    "provType": "@type",
    "featureType": "@type",
    "entityType": "@type",
    "has_provenance": {
      "@id": "dct:provenance",
      "@type": "@id"
    },
    "wasGeneratedBy": {
      "@id": "prov:wasGeneratedBy",
      "@type": "@id"
    },
    "wasAttributedTo": {
      "@id": "prov:wasAttributedTo",
      "@type": "@id"
    },
    "wasDerivedFrom": {
      "@id": "prov:wasDerivedFrom",
      "@type": "@id"
    },
    "alternateOf": {
      "@id": "prov:alternateOf",
      "@type": "@id"
    },
    "hadPrimarySource": {
      "@id": "prov:hadPrimarySource",
      "@type": "@id"
    },
    "specializationOf": {
      "@id": "prov:specializationOf",
      "@type": "@id"
    },
    "wasInvalidatedBy": {
      "@id": "prov:wasInvalidatedBy",
      "@type": "@id"
    },
    "wasQuotedFrom": {
      "@id": "prov:wasQuotedFrom",
      "@type": "@id"
    },
    "wasRevisionOf": {
      "@id": "prov:wasRevisionOf",
      "@type": "@id"
    },
    "generatedAtTime": {
      "@id": "prov:generatedAtTime",
      "@type": "xsd:dateTime"
    },
    "invalidatedAtTime": {
      "@id": "prov:invalidatedAtTime",
      "@type": "xsd:dateTime"
    },
    "value": "prov:value",
    "qualifiedPrimarySource": {
      "@id": "prov:qualifiedPrimarySource",
      "@type": "@id"
    },
    "qualifiedQuotation": {
      "@id": "prov:qualifiedQuotation",
      "@type": "@id"
    },
    "qualifiedRevision": {
      "@id": "prov:qualifiedRevision",
      "@type": "@id"
    },
    "atLocation": {
      "@id": "prov:atLocation",
      "@type": "@id"
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
    "qualifiedGeneration": {
      "@id": "prov:qualifiedGeneration",
      "@type": "@id"
    },
    "qualifiedInvalidation": {
      "@id": "prov:qualifiedInvalidation",
      "@type": "@id"
    },
    "qualifiedDerivation": {
      "@id": "prov:qualifiedDerivation",
      "@type": "@id"
    },
    "qualifiedAttribution": {
      "@id": "prov:qualifiedAttribution",
      "@type": "@id"
    },
    "activityType": "@type",
    "agentType": "@type",
    "Activity": "prov:Activity",
    "ActivityInfluence": "prov:ActivityInfluence",
    "Agent": "prov:Agent",
    "AgentInfluence": "prov:AgentInfluence",
    "Association": "prov:Association",
    "Attribution": "prov:Attribution",
    "Bundle": "prov:Bundle",
    "Collection": "prov:Collection",
    "Communication": "prov:Communication",
    "Delegation": "prov:Delegation",
    "Derivation": "prov:Derivation",
    "EmptyCollection": "prov:EmptyCollection",
    "End": "prov:End",
    "Entity": "prov:Entity",
    "EntityInfluence": "prov:EntityInfluence",
    "Generation": "prov:Generation",
    "Influence": "prov:Influence",
    "InstantaneousEvent": "prov:InstantaneousEvent",
    "Invalidation": "prov:Invalidation",
    "Location": "prov:Location",
    "Organization": "prov:Organization",
    "Person": "prov:Person",
    "Plan": "prov:Plan",
    "PrimarySource": "prov:PrimarySource",
    "Quotation": "prov:Quotation",
    "Revision": "prov:Revision",
    "Role": "prov:Role",
    "SoftwareAgent": "prov:SoftwareAgent",
    "Start": "prov:Start",
    "Usage": "prov:Usage",
    "ServiceDescription": "prov:ServiceDescription",
    "DirectQueryService": "prov:DirectQueryService",
    "Accept": "prov:Accept",
    "Contribute": "prov:Contribute",
    "Contributor": "prov:Contributor",
    "Copyright": "prov:Copyright",
    "Create": "prov:Create",
    "Creator": "prov:Creator",
    "Modify": "prov:Modify",
    "Publish": "prov:Publish",
    "Publisher": "prov:Publisher",
    "Replace": "prov:Replace",
    "RightsAssignment": "prov:RightsAssignment",
    "RightsHolder": "prov:RightsHolder",
    "Submit": "prov:Submit",
    "Dictionary": "prov:Dictionary",
    "EmptyDictionary": "prov:EmptyDictionary",
    "KeyEntityPair": "prov:KeyEntityPair",
    "Insertion": "prov:Insertion",
    "Removal": "prov:Removal",
    "atTime": {
      "@id": "prov:atTime",
      "@type": "xsd:dateTime"
    },
    "endedAtTime": {
      "@id": "prov:endedAtTime",
      "@type": "xsd:dateTime"
    },
    "startedAtTime": {
      "@id": "prov:startedAtTime",
      "@type": "xsd:dateTime"
    },
    "provenanceUriTemplate": "prov:provenanceUriTemplate",
    "pairKey": {
      "@id": "prov:pairKey",
      "@type": "rdfs:Literal"
    },
    "removedKey": {
      "@id": "prov:removedKey",
      "@type": "rdfs:Literal"
    },
    "actedOnBehalfOf": {
      "@id": "prov:actedOnBehalfOf",
      "@type": "@id"
    },
    "agent": {
      "@id": "prov:agent",
      "@type": "@id"
    },
    "entity": {
      "@id": "prov:entity",
      "@type": "@id"
    },
    "generated": {
      "@id": "prov:generated",
      "@type": "@id"
    },
    "hadActivity": {
      "@id": "prov:hadActivity",
      "@type": "@id"
    },
    "activity": {
      "@id": "prov:activity",
      "@type": "@id"
    },
    "hadGeneration": {
      "@id": "prov:hadGeneration",
      "@type": "@id"
    },
    "hadPlan": {
      "@id": "prov:hadPlan",
      "@type": "@id"
    },
    "hadRole": {
      "@id": "prov:hadRole",
      "@type": "@id"
    },
    "hadUsage": {
      "@id": "prov:hadUsage",
      "@type": "@id"
    },
    "influenced": {
      "@id": "prov:influenced",
      "@type": "@id"
    },
    "influencer": {
      "@id": "prov:influencer",
      "@type": "@id"
    },
    "invalidated": {
      "@id": "prov:invalidated",
      "@type": "@id"
    },
    "qualifiedAssociation": {
      "@id": "prov:qualifiedAssociation",
      "@type": "@id"
    },
    "qualifiedCommunication": {
      "@id": "prov:qualifiedCommunication",
      "@type": "@id"
    },
    "qualifiedDelegation": {
      "@id": "prov:qualifiedDelegation",
      "@type": "@id"
    },
    "qualifiedEnd": {
      "@id": "prov:qualifiedEnd",
      "@type": "@id"
    },
    "qualifiedStart": {
      "@id": "prov:qualifiedStart",
      "@type": "@id"
    },
    "qualifiedUsage": {
      "@id": "prov:qualifiedUsage",
      "@type": "@id"
    },
    "used": {
      "@id": "prov:used",
      "@type": "@id"
    },
    "wasAssociatedWith": {
      "@id": "prov:wasAssociatedWith",
      "@type": "@id"
    },
    "wasEndedBy": {
      "@id": "prov:wasEndedBy",
      "@type": "@id"
    },
    "wasInformedBy": {
      "@id": "prov:wasInformedBy",
      "@type": "@id"
    },
    "wasStartedBy": {
      "@id": "prov:wasStartedBy",
      "@type": "@id"
    },
    "has_anchor": {
      "@id": "prov:has_anchor",
      "@type": "@id"
    },
    "has_query_service": {
      "@id": "prov:has_query_service",
      "@type": "@id"
    },
    "describesService": {
      "@id": "prov:describesService",
      "@type": "@id"
    },
    "pingback": {
      "@id": "prov:pingback",
      "@type": "@id"
    },
    "dictionary": {
      "@id": "prov:dictionary",
      "@type": "@id"
    },
    "derivedByInsertionFrom": {
      "@id": "prov:derivedByInsertionFrom",
      "@type": "@id"
    },
    "derivedByRemovalFrom": {
      "@id": "prov:derivedByRemovalFrom",
      "@type": "@id"
    },
    "insertedKeyEntityPair": {
      "@id": "prov:insertedKeyEntityPair",
      "@type": "@id"
    },
    "hadDictionaryMember": {
      "@id": "prov:hadDictionaryMember",
      "@type": "@id"
    },
    "pairEntity": {
      "@id": "prov:pairEntity",
      "@type": "@id"
    },
    "qualifiedInsertion": {
      "@id": "prov:qualifiedInsertion",
      "@type": "@id"
    },
    "qualifiedRemoval": {
      "@id": "prov:qualifiedRemoval",
      "@type": "@id"
    },
    "asInBundle": {
      "@id": "prov:asInBundle",
      "@type": "@id"
    },
    "mentionOf": {
      "@id": "prov:mentionOf",
      "@type": "@id"
    },
    "name": "rdfs:label",
    "identifier": "crm:P1_is_identified_by",
    "isAbout": {
      "@id": "crm:P129_is_about",
      "@type": "@id"
    },
    "mediaType": "dct:format",
    "persistentIdentifier": "crm:P1_is_identified_by",
    "url": {
      "@id": "dcat:accessURL",
      "@type": "@id"
    },
    "crmdigType": {
      "@id": "rdf:type",
      "@type": "@id"
    },
    "hdtoType": {
      "@id": "rdf:type",
      "@type": "@id"
    },
    "prov": "http://www.w3.org/ns/prov#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "dct": "http://purl.org/dc/terms/",
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "oa": "http://www.w3.org/ns/oa#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "crmdig": "http://www.cidoc-crm.org/extensions/crmdig/",
    "hdto": "http://isl.ics.forth.gr/ontology/echoes/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/context.jsonld)

## Sources

* [CIDOC-CRM](https://www.cidoc-crm.org/)
* [PROV-O](https://www.w3.org/TR/prov-o/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/digital-representation`

