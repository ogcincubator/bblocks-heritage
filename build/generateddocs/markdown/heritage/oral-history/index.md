
# Oral History (Schema)

`ogc.heritage.oral-history` *v0.1*

An audio or video oral history recording or community narrative, modelled as a CIDOC-CRM E73 Information Object with a mandatory IIIF Presentation API manifest, interviewee reference, language, and consent status. Profiles ogc.heritage.digital-representation and adds schema.org AudioObject/VideoObject alignment for web visibility.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Oral History

An audio or video oral history recording, community narrative, or witness testimony associated
with a heritage site. Modelled as a **CIDOC-CRM E73 Information Object** (the recording as
an information-bearing object) and profiling `ogc.heritage.digital-representation` for its
PROV-O provenance chain, `isAbout` link, and media type field.

This block adds mandatory fields for the **interviewee**, **language**, **consent status**
(required for legal compliance), and an **IIIF Presentation API manifest** enabling playback
and timed annotation in IIIF viewers. It aligns with **schema.org AudioObject/VideoObject**
for web-visible metadata (`schema:interviewee`, `schema:duration`).

## CRM / vocab anchor

| JSON property | Predicate | Notes |
|---|---|---|
| `type` | `rdf:type crm:E73_Information_Object` | Fixed const `"OralHistory"` |
| `title` | `dct:title` | Human-readable recording title |
| `interviewee` | `schema:interviewee` | URI → actor; mandatory |
| `language` | `crm:P72_has_language` | BCP 47 code; mandatory |
| `consentStatus` | `dct:accessRights` | Consent/access label; mandatory |
| `iiifManifest` | `crm:P70i_is_documented_in` | IIIF manifest URI; mandatory |
| `mediaType` | `dct:format` | Inherited; must be audio/* or video/* |
| `rights` | `dct:rights` | Licence; inherited and required here |
| `isAbout` | `crm:P129_is_about` | Inherited; URI of heritage entity; required |
| `transcriptUri` | `crm:P70i_is_documented_in` | Optional transcript URI (same predicate as iiifManifest) |
| `duration` | `schema:duration` | ISO 8601 duration, e.g. "PT1H23M" |
| `contributor` | `dct:contributor` | Interviewer, technician, etc. |
| `theme` | `dct:subject` | Thematic keywords or controlled terms |
| `historicalPeriod` | `crm:P4_has_time-span` | Period addressed in the narrative |

## IIIF integration

The `iiifManifest` URI should point to a IIIF Presentation API 3 manifest. Timed annotations
(interview segments, thematic tags) can be added as IIIF Annotation Pages on the manifest's
Canvas, enabling rich viewer integration without modifying this block's schema.

## Consent handling

`consentStatus` is a required free-text or controlled-vocabulary field. Recommended values:

| Value | Meaning |
|---|---|
| `"consented — public access"` | Interviewee consented to unrestricted publication |
| `"consented — restricted"` | Consented but limited to specific audiences (e.g. researchers) |
| `"anonymised"` | Personal identifiers removed; public access permitted |
| `"pending consent review"` | Under review; restrict access until resolved |

`rights` (inherited from parent) carries the formal licence or copyright statement.

## SHACL targeting

`sh:targetClass crm:E73_Information_Object` is unambiguous: no other block in this register
uses E73 as a type const (the parent `digital-representation` has it in its bblock.json
metadata but no `type: const` in schema, so instances do not emit that RDF triple).

## Pilot use case

**HM-07** (Villa Portelli, Malta): audio and video testimonies from local residents and
historians recording lived memories of the villa, its garden, and surrounding community.

## Examples

### Oral history testimony about Villa Portelli, Malta (HM-07)
A video interview with a local resident recounting wartime memories of Villa Portelli and its grounds, recorded by Heritage Malta in 2023. Includes a transcript URI, IIIF manifest for playback, and consent metadata.
#### json
```json
{
  "id": "https://data.heritagemalta.org/oral-history/villap-oh-2023-004",
  "type": "OralHistory",
  "title": "Wartime memories of Villa Portelli — Interview with Maria Camilleri",
  "isAbout": "https://data.heritagemalta.org/place/villa-portelli",
  "interviewee": "https://data.heritagemalta.org/actor/maria-camilleri",
  "language": "mt",
  "consentStatus": "consented — public access",
  "iiifManifest": "https://data.heritagemalta.org/iiif/3/villap-oh-2023-004/manifest",
  "mediaType": "video/mp4",
  "rights": "https://creativecommons.org/licenses/by-nc/4.0/",
  "transcriptUri": "https://data.heritagemalta.org/oral-history/villap-oh-2023-004/transcript.pdf",
  "duration": "PT1H12M33S",
  "contributor": [
    "https://data.heritagemalta.org/actor/heritage-malta-oral-history-unit",
    "https://data.heritagemalta.org/actor/john-farrugia"
  ],
  "theme": ["wartime occupation", "domestic life", "World War II", "Villa Portelli gardens"],
  "historicalPeriod": "1940–1945",
  "persistentIdentifier": "https://hdl.handle.net/21.T11998/hm-oh-2023-004"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/oral-history/context.jsonld",
  "id": "https://data.heritagemalta.org/oral-history/villap-oh-2023-004",
  "type": "OralHistory",
  "title": "Wartime memories of Villa Portelli \u2014 Interview with Maria Camilleri",
  "isAbout": "https://data.heritagemalta.org/place/villa-portelli",
  "interviewee": "https://data.heritagemalta.org/actor/maria-camilleri",
  "language": "mt",
  "consentStatus": "consented \u2014 public access",
  "iiifManifest": "https://data.heritagemalta.org/iiif/3/villap-oh-2023-004/manifest",
  "mediaType": "video/mp4",
  "rights": "https://creativecommons.org/licenses/by-nc/4.0/",
  "transcriptUri": "https://data.heritagemalta.org/oral-history/villap-oh-2023-004/transcript.pdf",
  "duration": "PT1H12M33S",
  "contributor": [
    "https://data.heritagemalta.org/actor/heritage-malta-oral-history-unit",
    "https://data.heritagemalta.org/actor/john-farrugia"
  ],
  "theme": [
    "wartime occupation",
    "domestic life",
    "World War II",
    "Villa Portelli gardens"
  ],
  "historicalPeriod": "1940\u20131945",
  "persistentIdentifier": "https://hdl.handle.net/21.T11998/hm-oh-2023-004"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix schema: <https://schema.org/> .

<https://data.heritagemalta.org/oral-history/villap-oh-2023-004> a crm:E73_Information_Object ;
    dct:accessRights "consented — public access" ;
    dct:contributor <https://data.heritagemalta.org/actor/heritage-malta-oral-history-unit>,
        <https://data.heritagemalta.org/actor/john-farrugia> ;
    dct:format "video/mp4" ;
    dct:rights "https://creativecommons.org/licenses/by-nc/4.0/" ;
    dct:subject "Villa Portelli gardens",
        "World War II",
        "domestic life",
        "wartime occupation" ;
    dct:title "Wartime memories of Villa Portelli — Interview with Maria Camilleri" ;
    crm:P129_is_about <https://data.heritagemalta.org/place/villa-portelli> ;
    crm:P1_is_identified_by "https://hdl.handle.net/21.T11998/hm-oh-2023-004" ;
    crm:P4_has_time-span "1940–1945" ;
    crm:P70i_is_documented_in <https://data.heritagemalta.org/iiif/3/villap-oh-2023-004/manifest>,
        <https://data.heritagemalta.org/oral-history/villap-oh-2023-004/transcript.pdf> ;
    crm:P72_has_language "mt" ;
    schema:duration "PT1H12M33S" ;
    schema:interviewee <https://data.heritagemalta.org/actor/maria-camilleri> .


```


### Community narrative about garden traditions at Villa Portelli (HM-07)
A shorter audio recording of a garden keeper's account of traditional horticultural practices at Villa Portelli, with restricted consent status pending full review. Minimal example without transcript or contributor metadata.
#### json
```json
{
  "id": "https://data.heritagemalta.org/oral-history/villap-oh-2023-007",
  "type": "OralHistory",
  "title": "Traditional horticultural practices at Villa Portelli — Garden keeper account",
  "isAbout": "https://data.heritagemalta.org/heritage-site/villa-portelli-garden",
  "interviewee": "https://data.heritagemalta.org/actor/joseph-debono",
  "language": "mt",
  "consentStatus": "pending consent review",
  "iiifManifest": "https://data.heritagemalta.org/iiif/3/villap-oh-2023-007/manifest",
  "mediaType": "audio/mp4",
  "rights": "All rights reserved — Heritage Malta",
  "duration": "PT24M10S",
  "theme": ["garden history", "horticulture", "Villa Portelli"]
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/oral-history/context.jsonld",
  "id": "https://data.heritagemalta.org/oral-history/villap-oh-2023-007",
  "type": "OralHistory",
  "title": "Traditional horticultural practices at Villa Portelli \u2014 Garden keeper account",
  "isAbout": "https://data.heritagemalta.org/heritage-site/villa-portelli-garden",
  "interviewee": "https://data.heritagemalta.org/actor/joseph-debono",
  "language": "mt",
  "consentStatus": "pending consent review",
  "iiifManifest": "https://data.heritagemalta.org/iiif/3/villap-oh-2023-007/manifest",
  "mediaType": "audio/mp4",
  "rights": "All rights reserved \u2014 Heritage Malta",
  "duration": "PT24M10S",
  "theme": [
    "garden history",
    "horticulture",
    "Villa Portelli"
  ]
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix schema: <https://schema.org/> .

<https://data.heritagemalta.org/oral-history/villap-oh-2023-007> a crm:E73_Information_Object ;
    dct:accessRights "pending consent review" ;
    dct:format "audio/mp4" ;
    dct:rights "All rights reserved — Heritage Malta" ;
    dct:subject "Villa Portelli",
        "garden history",
        "horticulture" ;
    dct:title "Traditional horticultural practices at Villa Portelli — Garden keeper account" ;
    crm:P129_is_about <https://data.heritagemalta.org/heritage-site/villa-portelli-garden> ;
    crm:P70i_is_documented_in <https://data.heritagemalta.org/iiif/3/villap-oh-2023-007/manifest> ;
    crm:P72_has_language "mt" ;
    schema:duration "PT24M10S" ;
    schema:interviewee <https://data.heritagemalta.org/actor/joseph-debono> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Oral History
description: An audio or video oral history recording or community narrative. Profiles
  digital-representation (CIDOC-CRM E73 Information Object) and adds mandatory fields
  for interviewee, language, consent status and IIIF manifest, with optional schema.org
  AudioObject/VideoObject alignment for web-visible metadata.
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml
- type: object
  required:
  - type
  - interviewee
  - language
  - consentStatus
  - iiifManifest
  - mediaType
  - rights
  properties:
    type:
      const: OralHistory
      description: Fixed type token (maps to crm:E73_Information_Object).
      x-jsonld-id: '@type'
    title:
      type: string
      description: Human-readable title of the recording (dct:title).
      x-jsonld-id: http://purl.org/dc/terms/title
    interviewee:
      type: string
      format: uri
      description: URI of the person interviewed or narrating (schema:interviewee).
        Must reference an ogc.heritage.actor record. Required.
      x-jsonld-id: https://schema.org/interviewee
      x-jsonld-type: '@id'
    language:
      type: string
      description: BCP 47 language code of the recording, e.g. "mt", "en", "it" (crm:P72_has_language).
        Required.
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P72_has_language
    consentStatus:
      type: string
      description: "Consent and access status of the recording, e.g. \"consented \u2014
        public access\", \"consented \u2014 restricted to researchers\", \"anonymised\",
        \"pending consent review\" (dct:accessRights). Required for legal compliance."
      x-jsonld-id: http://purl.org/dc/terms/accessRights
    iiifManifest:
      type: string
      format: uri
      description: URI of an IIIF Presentation API 3 manifest for this recording,
        enabling playback and timed annotation in IIIF viewers (crm:P70i_is_documented_in).
        Required.
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70i_is_documented_in
      x-jsonld-type: '@id'
    mediaType:
      type: string
      description: IANA media type of the recording file (dct:format), e.g. "audio/mp4",
        "audio/mpeg", "video/mp4". Required; must be an audio/* or video/* type.
      pattern: ^(audio|video)/
      x-jsonld-id: http://purl.org/dc/terms/format
    rights:
      type: string
      description: "Licence or copyright statement (dct:rights), e.g. a Creative Commons
        URI or \"All rights reserved \u2014 Heritage Malta\". Required."
      x-jsonld-id: http://purl.org/dc/terms/rights
    transcriptUri:
      type: string
      format: uri
      description: URI of a text transcript of the recording (crm:P70i_is_documented_in).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70i_is_documented_in
      x-jsonld-type: '@id'
    duration:
      type: string
      description: Duration of the recording in ISO 8601 duration format (schema:duration),
        e.g. "PT1H23M45S".
      x-jsonld-id: https://schema.org/duration
    contributor:
      oneOf:
      - type: string
        format: uri
      - type: array
        items:
          type: string
          format: uri
      description: URI(s) of other persons or organisations who contributed to the
        recording (dct:contributor), e.g. interviewer, sound technician, transcriber.
      x-jsonld-id: http://purl.org/dc/terms/contributor
      x-jsonld-type: '@id'
    theme:
      oneOf:
      - type: string
      - type: array
        items:
          type: string
      description: Thematic keywords or controlled-vocabulary terms describing the
        recording's subject matter (dct:subject), e.g. "domestic life", "World War
        II occupation", "garden traditions".
      x-jsonld-id: http://purl.org/dc/terms/subject
    historicalPeriod:
      type: string
      description: "The historical period or date range addressed in the narrative
        (crm:P4_has_time-span), e.g. \"1940\u20131945\", \"interwar period\"."
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
x-jsonld-extra-terms:
  OralHistory: http://www.cidoc-crm.org/cidoc-crm/E73_Information_Object
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/
  schema: https://schema.org/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/oral-history/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/oral-history/schema.yaml)


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
    "type": "@type",
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
    "title": "dct:title",
    "interviewee": {
      "@id": "schema:interviewee",
      "@type": "@id"
    },
    "language": "crm:P72_has_language",
    "consentStatus": "dct:accessRights",
    "iiifManifest": {
      "@id": "crm:P70i_is_documented_in",
      "@type": "@id"
    },
    "rights": "dct:rights",
    "transcriptUri": {
      "@id": "crm:P70i_is_documented_in",
      "@type": "@id"
    },
    "duration": "schema:duration",
    "contributor": {
      "@id": "dct:contributor",
      "@type": "@id"
    },
    "theme": "dct:subject",
    "historicalPeriod": "crm:P4_has_time-span",
    "OralHistory": "crm:E73_Information_Object",
    "prov": "http://www.w3.org/ns/prov#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "dct": "http://purl.org/dc/terms/",
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "oa": "http://www.w3.org/ns/oa#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "schema": "https://schema.org/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/oral-history/context.jsonld)

## Sources

* [CIDOC-CRM E73 Information Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E73)
* [IIIF Presentation API 3.0](https://iiif.io/api/presentation/3.0/)
* [schema.org AudioObject / VideoObject](https://schema.org/AudioObject)
* [HERITALISE D8.2 HM-07 (Audiovisual Media / Oral Histories)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/oral-history`

