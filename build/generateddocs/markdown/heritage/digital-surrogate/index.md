
# Digital Surrogate (Schema)

`ogc.heritage.digital-surrogate` *v0.1*

A digital file produced by digitising a physical source carrier (e.g. a photograph, drawing or bound volume), modelled as a CRMdig D1 Digital Object with a mandatory PROV derivation chain linking to the source-carrier and a D7 digitisation event sub-object. Profiles ogc.heritage.digital-representation.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Digital Surrogate

A digital file produced by digitising a physical source carrier — a photograph, architectural
drawing, negative, manuscript or bound volume — and made available for research, preservation or
publication. Modelled as a **CRMdig D1 Digital Object** (subtype of CIDOC-CRM E73 Information
Object) with a mandatory PROV derivation link to the originating `source-carrier` record and a
structured **CRMdig D7 Digital Machine Event** sub-object recording the digitisation process.

This block profiles `ogc.heritage.digital-representation` and inherits its `isAbout` link (the
heritage asset the surrogate documents), `mediaType` (IANA format), PROV-O provenance properties,
and persistent identifier field.

## CRM anchor

| JSON property | CRM / vocab predicate | Notes |
|---|---|---|
| `type` | `rdf:type crmdig:D1_Digital_Object` | Fixed const `"DigitalSurrogate"` |
| `wasDerivedFrom` | `prov:wasDerivedFrom` | URI → source-carrier; mandatory |
| `digitisationEvent` | `crmdig:L11i_was_output_of` | D7 event; `@type: @json` opaque object |
| `mediaType` | `dct:format` | Inherited from parent |
| `rights` | `dct:rights` | Licence or access label; mandatory |
| `isAbout` | `crm:P129_is_about` | Inherited; URI of the documented heritage asset |
| `iiifManifest` | `crm:P70i_is_documented_in` | Optional IIIF manifest URI |
| `completeness` | `crm:P3_has_note` | Optional free-text coverage statement |

## Digitisation event sub-object

The `digitisationEvent` object records the CRMdig D7 Digital Machine Event. It is stored as an
opaque JSON blob (`@type: "@json"`) linked via `crmdig:L11i_was_output_of` (the inverse of
`L11 had output`, meaning "this D1 object was the output of this D7 event"). At minimum, `method`
and `date` are required.

| Field | Description |
|---|---|
| `method` (required) | Technique: "flatbed scan", "photogrammetry", "RTI", "digital photography" |
| `date` (required) | ISO 8601 date of the digitisation event |
| `equipment` | Scanner / camera make and model |
| `software` | Capture or processing software |
| `operator` | Person or institution responsible |
| `resolution` | Pixel density or scan resolution |
| `colourMode` | "RGB", "grayscale", "bitonal" |
| `quality` | Derivative tier: "master", "access copy", "thumbnail" |
| `processingHistory` | Free-text log of post-processing steps |

## PROV chain

The full provenance chain for a digitised heritage resource is:

```
source-carrier (E31 Document)
  ← prov:wasDerivedFrom —
    digital-surrogate (D1 Digital Object)
```

For derived products (an OCR transcript, a vector tracing, a colour-corrected reprint), the
chain extends further using additional `prov:wasDerivedFrom` links within those records.

## SHACL targeting

SHACL shapes target nodes via `sh:targetSubjectsOf prov:wasDerivedFrom` rather than
`sh:targetClass crmdig:D1_Digital_Object`. This avoids false positives against
`survey-dataset` instances, which share the same RDF class but lack the derivation link.

## Pilot use cases

| Pilot | Requirement | Typical use |
|---|---|---|
| Reggia di Venaria (CRRS) | CRRS-013 | Digitised archival plans, engravings, photographs from ASTo and Archivio Fotografico |
| Villa Portelli, Malta (HM) | HM-05 | Digitised historical photographs from Heritage Malta collections |

## Examples

### Digitised architectural plan from Venaria Reale (CRRS-013)
A high-resolution scan of a 17th-century architectural plan from the State Archive of Turin, recording the Galleria Grande wing of the Reggia di Venaria. The surrogate is derived from a paper drawing held in the archive (source-carrier), with full digitisation paradata and an IIIF manifest for viewer integration.
#### json
```json
{
  "id": "https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-mazzo12-n3-scan-2024",
  "type": "DigitalSurrogate",
  "isAbout": "https://data.regiadivenaria.it/heritage-object/galleria-grande",
  "wasDerivedFrom": "https://data.regiadivenaria.it/source-carrier/ASTo-VR-mazzo12-n3",
  "digitisationEvent": {
    "method": "flatbed scan",
    "date": "2024-03-15",
    "equipment": "Zeutschel OS 14000 A1 overhead scanner",
    "software": "Zeutschel OmniScan 12",
    "operator": "Archivio di Stato di Torino, Laboratorio Digitalizzazione",
    "resolution": "400dpi",
    "colourMode": "RGB",
    "quality": "master",
    "processingHistory": "Flatbed scan of rolled plan; ICC colour calibration; converted from TIFF master to JPEG access copy."
  },
  "mediaType": "image/tiff",
  "rights": "https://creativecommons.org/licenses/by/4.0/",
  "persistentIdentifier": "https://hdl.handle.net/21.T11998/rv-plan-2024-0042",
  "iiifManifest": "https://data.regiadivenaria.it/iiif/3/ASTo-VR-mazzo12-n3-scan-2024/manifest",
  "completeness": "complete — full sheet digitised; minor foxing on lower-right corner noted in processing history"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-surrogate/context.jsonld",
  "id": "https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-mazzo12-n3-scan-2024",
  "type": "DigitalSurrogate",
  "isAbout": "https://data.regiadivenaria.it/heritage-object/galleria-grande",
  "wasDerivedFrom": "https://data.regiadivenaria.it/source-carrier/ASTo-VR-mazzo12-n3",
  "digitisationEvent": {
    "method": "flatbed scan",
    "date": "2024-03-15",
    "equipment": "Zeutschel OS 14000 A1 overhead scanner",
    "software": "Zeutschel OmniScan 12",
    "operator": "Archivio di Stato di Torino, Laboratorio Digitalizzazione",
    "resolution": "400dpi",
    "colourMode": "RGB",
    "quality": "master",
    "processingHistory": "Flatbed scan of rolled plan; ICC colour calibration; converted from TIFF master to JPEG access copy."
  },
  "mediaType": "image/tiff",
  "rights": "https://creativecommons.org/licenses/by/4.0/",
  "persistentIdentifier": "https://hdl.handle.net/21.T11998/rv-plan-2024-0042",
  "iiifManifest": "https://data.regiadivenaria.it/iiif/3/ASTo-VR-mazzo12-n3-scan-2024/manifest",
  "completeness": "complete \u2014 full sheet digitised; minor foxing on lower-right corner noted in processing history"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.ics.forth.gr/isl/CRMdig/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://data.regiadivenaria.it/digital-surrogate/ASTo-VR-mazzo12-n3-scan-2024> a crmdig:D1_Digital_Object ;
    dct:format "image/tiff" ;
    dct:rights "https://creativecommons.org/licenses/by/4.0/" ;
    crm:P129_is_about <https://data.regiadivenaria.it/heritage-object/galleria-grande> ;
    crm:P1_is_identified_by "https://hdl.handle.net/21.T11998/rv-plan-2024-0042" ;
    crm:P3_has_note "complete — full sheet digitised; minor foxing on lower-right corner noted in processing history" ;
    crm:P70i_is_documented_in <https://data.regiadivenaria.it/iiif/3/ASTo-VR-mazzo12-n3-scan-2024/manifest> ;
    crmdig:L11i_was_output_of "{\"colourMode\":\"RGB\",\"date\":\"2024-03-15\",\"equipment\":\"Zeutschel OS 14000 A1 overhead scanner\",\"method\":\"flatbed scan\",\"operator\":\"Archivio di Stato di Torino, Laboratorio Digitalizzazione\",\"processingHistory\":\"Flatbed scan of rolled plan; ICC colour calibration; converted from TIFF master to JPEG access copy.\",\"quality\":\"master\",\"resolution\":\"400dpi\",\"software\":\"Zeutschel OmniScan 12\"}"^^rdf:JSON ;
    prov:wasDerivedFrom <https://data.regiadivenaria.it/source-carrier/ASTo-VR-mazzo12-n3> .


```


### Digitised historical photograph of Villa Portelli, Malta (HM-05)
A digital photograph produced from a 1953 photographic negative depicting Villa Portelli, held by Heritage Malta. Records the derivation chain from the physical negative (source-carrier), the digitisation event, and access rights. A minimal example without an IIIF manifest.
#### json
```json
{
  "id": "https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig",
  "type": "DigitalSurrogate",
  "isAbout": "https://data.heritagemalta.org/place/villa-portelli",
  "wasDerivedFrom": "https://data.heritagemalta.org/source-carrier/photo-neg-1953-001",
  "digitisationEvent": {
    "method": "digital photography from negative",
    "date": "2023-11-08",
    "equipment": "Phase One iXG 50MP with copy stand",
    "software": "Capture One 23",
    "operator": "Heritage Malta Conservation Department",
    "resolution": "50MP",
    "colourMode": "RGB",
    "quality": "access copy",
    "processingHistory": "35mm negative photographed on copy stand; colour correction and contrast adjustment applied."
  },
  "mediaType": "image/jpeg",
  "rights": "https://creativecommons.org/licenses/by-nc/4.0/"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-surrogate/context.jsonld",
  "id": "https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig",
  "type": "DigitalSurrogate",
  "isAbout": "https://data.heritagemalta.org/place/villa-portelli",
  "wasDerivedFrom": "https://data.heritagemalta.org/source-carrier/photo-neg-1953-001",
  "digitisationEvent": {
    "method": "digital photography from negative",
    "date": "2023-11-08",
    "equipment": "Phase One iXG 50MP with copy stand",
    "software": "Capture One 23",
    "operator": "Heritage Malta Conservation Department",
    "resolution": "50MP",
    "colourMode": "RGB",
    "quality": "access copy",
    "processingHistory": "35mm negative photographed on copy stand; colour correction and contrast adjustment applied."
  },
  "mediaType": "image/jpeg",
  "rights": "https://creativecommons.org/licenses/by-nc/4.0/"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.ics.forth.gr/isl/CRMdig/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://data.heritagemalta.org/digital-surrogate/villap-photo-neg-1953-001-dig> a crmdig:D1_Digital_Object ;
    dct:format "image/jpeg" ;
    dct:rights "https://creativecommons.org/licenses/by-nc/4.0/" ;
    crm:P129_is_about <https://data.heritagemalta.org/place/villa-portelli> ;
    crmdig:L11i_was_output_of "{\"colourMode\":\"RGB\",\"date\":\"2023-11-08\",\"equipment\":\"Phase One iXG 50MP with copy stand\",\"method\":\"digital photography from negative\",\"operator\":\"Heritage Malta Conservation Department\",\"processingHistory\":\"35mm negative photographed on copy stand; colour correction and contrast adjustment applied.\",\"quality\":\"access copy\",\"resolution\":\"50MP\",\"software\":\"Capture One 23\"}"^^rdf:JSON ;
    prov:wasDerivedFrom <https://data.heritagemalta.org/source-carrier/photo-neg-1953-001> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Digital Surrogate
description: A digital file produced by digitising a physical source carrier, modelled
  as a CRMdig D1 Digital Object. Adds a mandatory PROV derivation link to the source-carrier
  and a structured CRMdig D7 digitisation event sub-object.
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml
- type: object
  required:
  - type
  - wasDerivedFrom
  - digitisationEvent
  - mediaType
  - rights
  properties:
    type:
      const: DigitalSurrogate
      description: Fixed type token (maps to crmdig:D1_Digital_Object via JSON-LD
        context).
      x-jsonld-id: '@type'
    wasDerivedFrom:
      type: string
      format: uri
      description: Persistent URI of the source-carrier (physical original) from which
        this digital file was produced (prov:wasDerivedFrom). Must reference an ogc.heritage.source-carrier
        record.
      x-jsonld-id: http://www.w3.org/ns/prov#wasDerivedFrom
      x-jsonld-type: '@id'
    digitisationEvent:
      type: object
      description: Structured record of the CRMdig D7 Digital Machine Event that created
        this digital surrogate. The digitisation method and date are mandatory; all
        other fields are optional paradata.
      required:
      - method
      - date
      properties:
        method:
          type: string
          description: Digitisation technique used, e.g. "flatbed scan", "digital
            photography", "photogrammetry", "RTI", "multispectral imaging".
        date:
          type: string
          format: date
          description: Date on which the digitisation was performed (ISO 8601, e.g.
            "2024-03-15").
        equipment:
          type: string
          description: Make and model of the scanner, camera or acquisition instrument.
        software:
          type: string
          description: Software used for capture or initial format conversion.
        operator:
          type: string
          description: Person or organisation responsible for the digitisation.
        resolution:
          type: string
          description: Scan resolution or pixel density, e.g. "600dpi", "24MP", "3
            cm/pixel GSD".
        colourMode:
          type: string
          description: Colour mode of the output file, e.g. "RGB", "grayscale", "bitonal".
        quality:
          type: string
          description: Quality or tier label of this file within the derivative chain,
            e.g. "master", "access copy", "thumbnail".
        processingHistory:
          type: string
          description: Free-text description of post-processing steps applied after
            capture.
      x-jsonld-id: http://www.ics.forth.gr/isl/CRMdig/L11i_was_output_of
      x-jsonld-type: '@json'
    mediaType:
      type: string
      description: IANA media type of the digital surrogate file (dct:format), e.g.
        "image/tiff", "image/jpeg", "application/pdf". Required.
      x-jsonld-id: http://purl.org/dc/terms/format
    rights:
      type: string
      description: "Access rights or licence for this digital surrogate (dct:rights),
        e.g. a Creative Commons URI or a label such as \"Restricted \u2014 archive
        access only\". Required."
      x-jsonld-id: http://purl.org/dc/terms/rights
    iiifManifest:
      type: string
      format: uri
      description: URI of an IIIF Presentation API manifest for this digital surrogate,
        enabling deep-zoom or annotation in IIIF viewers (crm:P70i_is_documented_in).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P70i_is_documented_in
      x-jsonld-type: '@id'
    completeness:
      type: string
      description: "Free-text statement on coverage completeness, e.g. \"complete\",
        \"pages 3\u20135 missing due to damage\", \"recto only\"."
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
x-jsonld-extra-terms:
  DigitalSurrogate: http://www.ics.forth.gr/isl/CRMdig/D1_Digital_Object
x-jsonld-prefixes:
  crmdig: http://www.ics.forth.gr/isl/CRMdig/
  prov: http://www.w3.org/ns/prov#
  dct: http://purl.org/dc/terms/
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-surrogate/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-surrogate/schema.yaml)


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
    "digitisationEvent": {
      "@id": "crmdig:L11i_was_output_of",
      "@type": "@json"
    },
    "rights": "dct:rights",
    "iiifManifest": {
      "@id": "crm:P70i_is_documented_in",
      "@type": "@id"
    },
    "completeness": "crm:P3_has_note",
    "DigitalSurrogate": "crmdig:D1_Digital_Object",
    "prov": "http://www.w3.org/ns/prov#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "dct": "http://purl.org/dc/terms/",
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "oa": "http://www.w3.org/ns/oa#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "crmdig": "http://www.ics.forth.gr/isl/CRMdig/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-surrogate/context.jsonld)

## Sources

* [CRMdig D1 Digital Object / D7 Digital Machine Event](https://www.ics.forth.gr/isl/CRMdig/)
* [PROV-O: The PROV Ontology](https://www.w3.org/TR/prov-o/)
* [IIIF Presentation API 3.0](https://iiif.io/api/presentation/3.0/)
* [HERITALISE D8.2 CRRS-013 (Digital Surrogate / Migrated Digital Resource)](https://heritalise-eccch.eu/)
* [HERITALISE D8.2 HM-05 (Digital Photographs, Malta)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/digital-surrogate`

