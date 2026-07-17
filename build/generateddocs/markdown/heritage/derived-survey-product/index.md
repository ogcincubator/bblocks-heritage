
# Derived Survey Product (Schema)

`ogc.heritage.derived-survey-product` *v0.1*

A processed output derived from a geometric survey dataset — section drawing, triangulated mesh, deviation map or registration report — modelled as a CRMdig D9 Data Object. Profiles ogc.heritage.digital-representation, adding a mandatory PROV derivation link to the parent survey dataset and a structured CRMdig D10 Software Execution sub-object recording the processing parameters.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Derived Survey Product

A processed output produced by applying computational methods to a raw geometric survey dataset
(point cloud, photogrammetric model, etc.). Typical products include triangulated meshes, section
drawings, deviation maps, orthophotos and registration reports.

Modelled as a **CRMdig D9 Data Object** (a derived digital object). Profiles
`ogc.heritage.digital-representation`, adding:

- a mandatory **PROV derivation link** (`prov:wasDerivedFrom`) to the source
  `ogc.heritage.survey-dataset` record
- a structured **CRMdig D10 Software Execution** sub-object (`processingEvent`) capturing the
  processing method, software, operator, parameters and quality result

The spatial extent of the derived product is inherited from the parent survey dataset; no
separate coverage geometry is required.

## CRM anchor

| Term | URI |
|------|-----|
| D9 Data Object | `crmdig:D9_Data_Object` |
| D10 Software Execution | `crmdig:D10_Software_Execution` |
| L11i was_output_of | `crmdig:L11i_was_output_of` |

## Properties

| Property | Mapping | Required | Notes |
|----------|---------|----------|-------|
| `type` | `@type` → `crmdig:D9_Data_Object` | ✔ | Fixed const `"DerivedSurveyProduct"` |
| `title` | `dct:title` | ✔ | Human-readable label for the product |
| `isAbout` | `crm:P129_is_about` | ✔ | URI of the heritage asset (inherited from `digital-representation`) |
| `parentDataset` | `prov:wasDerivedFrom` | ✔ | URI of the source `survey-dataset` record |
| `processingEvent` | `crmdig:L11i_was_output_of` (@json) | ✔ | D10 Software Execution sub-object (see below) |
| `url` | `dcat:accessURL` | — | Download URL for the derived file (inherited) |
| `mediaType` | `dct:format` | — | IANA media type of the output file (inherited) |
| `limitations` | `dct:description` | — | Known artefacts or coverage caveats |
| `reviewStatus` | — | — | Workflow status (draft / reviewed / approved) |

### `processingEvent` sub-object (D10 Software Execution)

Treated as an opaque JSON value at the RDF level (`@type: "@json"`). Fields within the sub-object
are not individually mapped to RDF predicates — they are captured as processing paradata.

| Field | Required | Notes |
|-------|----------|-------|
| `processingMethod` | ✔ | e.g. `"mesh generation"`, `"section extraction"`, `"deviation mapping"` |
| `processingDate` | ✔ | ISO 8601 date |
| `software` | — | Name of the processing software |
| `softwareVersion` | — | Version string |
| `operator` | — | URI of the responsible person/organisation |
| `parameters` | — | Key settings used (free-text) |
| `accuracy` | — | Quality metric, e.g. `"mean deviation ±2 mm"` |
| `qaResult` | — | QA outcome label |

## Design notes

- **Profile choice:** `digital-representation` (not `survey-dataset`) is the correct JSON Schema
  parent because `survey-dataset` requires `acquisitionEvent` and `coverage`, which derived
  products do not have. The semantic relationship to the source dataset is captured via
  `prov:wasDerivedFrom`. CRMdig D9 IS-A D1 Digital Object, consistent with profiling the
  D1-anchored `digital-representation` block.
- **SHACL targeting:** `sh:targetClass crmdig:D9_Data_Object` is safe because no other block in
  this register maps any type token to that class.
- **processingEvent as @json:** Consistent with the `acquisitionEvent` pattern in `survey-dataset`.
  Inner fields are not SHACL-validated; the shape only checks that the sub-object is present.

## Use cases

- CRRS-008 (Reggia di Venaria): mesh, section, deviation map and registration report derived from
  TLS or UAV photogrammetric surveys of palace interiors.
- HM-05 (Villa Portelli, Malta): processed photogrammetric outputs (dense point cloud mesh,
  orthophotos) submitted as deliverables from a recording campaign.

## Examples

### Venaria — triangulated mesh derived from TLS point cloud of Galleria Grande ceiling
A triangulated mesh generated from the Galleria Grande ceiling TLS point cloud, with the PROV derivation link back to the source survey dataset and a structured processing event recording the mesh-generation method, software and QA result.
#### json
```json
{
  "type": "DerivedSurveyProduct",
  "id": "https://heritalise-eccch.eu/resource/product/gc-ceiling-mesh-2024",
  "title": "Triangulated Mesh — Galleria Grande Ceiling (derived from TLS 2024)",
  "isAbout": "https://heritalise-eccch.eu/resource/building/galleria-grande",
  "parentDataset": "https://heritalise-eccch.eu/resource/survey/gc-ceiling-tls-2024",
  "mediaType": "model/obj",
  "url": "https://heritalise-eccch.eu/resource/files/gc-ceiling-mesh-2024.obj",
  "processingEvent": {
    "processingMethod": "mesh generation",
    "processingDate": "2024-04-03",
    "software": "Leica Cyclone 3DR",
    "softwareVersion": "2024.0.1",
    "operator": "https://orcid.org/0000-0000-0000-0001",
    "parameters": "Poisson reconstruction; octree depth 10; smoothing kernel radius 5 mm",
    "accuracy": "mean deviation from source cloud ±1.8 mm",
    "qaResult": "passed"
  },
  "limitations": "Fresco surface micro-detail below 2 mm not captured; scaffolding shadow in NW corner present in source cloud.",
  "reviewStatus": "reviewed"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/derived-survey-product/context.jsonld",
  "type": "DerivedSurveyProduct",
  "id": "https://heritalise-eccch.eu/resource/product/gc-ceiling-mesh-2024",
  "title": "Triangulated Mesh \u2014 Galleria Grande Ceiling (derived from TLS 2024)",
  "isAbout": "https://heritalise-eccch.eu/resource/building/galleria-grande",
  "parentDataset": "https://heritalise-eccch.eu/resource/survey/gc-ceiling-tls-2024",
  "mediaType": "model/obj",
  "url": "https://heritalise-eccch.eu/resource/files/gc-ceiling-mesh-2024.obj",
  "processingEvent": {
    "processingMethod": "mesh generation",
    "processingDate": "2024-04-03",
    "software": "Leica Cyclone 3DR",
    "softwareVersion": "2024.0.1",
    "operator": "https://orcid.org/0000-0000-0000-0001",
    "parameters": "Poisson reconstruction; octree depth 10; smoothing kernel radius 5 mm",
    "accuracy": "mean deviation from source cloud \u00b11.8 mm",
    "qaResult": "passed"
  },
  "limitations": "Fresco surface micro-detail below 2 mm not captured; scaffolding shadow in NW corner present in source cloud.",
  "reviewStatus": "reviewed"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.ics.forth.gr/isl/CRMdig/> .
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise-eccch.eu/resource/product/gc-ceiling-mesh-2024> a crmdig:D9_Data_Object ;
    dct:description "Fresco surface micro-detail below 2 mm not captured; scaffolding shadow in NW corner present in source cloud." ;
    dct:format "model/obj" ;
    dct:title "Triangulated Mesh — Galleria Grande Ceiling (derived from TLS 2024)" ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/building/galleria-grande> ;
    crmdig:L11i_was_output_of "{\"accuracy\":\"mean deviation from source cloud ±1.8 mm\",\"operator\":\"https://orcid.org/0000-0000-0000-0001\",\"parameters\":\"Poisson reconstruction; octree depth 10; smoothing kernel radius 5 mm\",\"processingDate\":\"2024-04-03\",\"processingMethod\":\"mesh generation\",\"qaResult\":\"passed\",\"software\":\"Leica Cyclone 3DR\",\"softwareVersion\":\"2024.0.1\"}"^^rdf:JSON ;
    dcat:accessURL <https://heritalise-eccch.eu/resource/files/gc-ceiling-mesh-2024.obj> ;
    prov:wasDerivedFrom <https://heritalise-eccch.eu/resource/survey/gc-ceiling-tls-2024> .


```


### Malta — deviation map derived from UAV photogrammetric survey of Villa Portelli exterior
A facade deviation map derived from the Villa Portelli UAV photogrammetric survey, comparing the surveyed geometry against a design BIM reference mesh, with a persistent DOI alongside the record's own URI.
#### json
```json
{
  "type": "DerivedSurveyProduct",
  "id": "https://heritalise-eccch.eu/resource/product/villa-portelli-deviation-map-2025",
  "title": "Facade Deviation Map — Villa Portelli exterior (derived from UAV survey 2025)",
  "isAbout": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "parentDataset": "https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025",
  "mediaType": "image/tiff",
  "url": "https://heritalise-eccch.eu/resource/files/villa-portelli-deviation-map-2025.tif",
  "persistentIdentifier": "https://doi.org/10.00000/villa-portelli-deviationmap-2025",
  "processingEvent": {
    "processingMethod": "deviation mapping",
    "processingDate": "2025-07-02",
    "software": "CloudCompare",
    "softwareVersion": "2.13.1",
    "operator": "https://orcid.org/0000-0000-0000-0002",
    "parameters": "Reference surface: design BIM mesh (IFC); max search radius 0.5 m; colour ramp ±100 mm",
    "accuracy": "RMS deviation 38 mm; 92 % of facade within ±50 mm of design intent",
    "qaResult": "passed with remarks — two pilasters exceed ±80 mm threshold"
  },
  "limitations": "Roof surface excluded (access restrictions). Balcony undersides have partial occlusion.",
  "reviewStatus": "approved"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/derived-survey-product/context.jsonld",
  "type": "DerivedSurveyProduct",
  "id": "https://heritalise-eccch.eu/resource/product/villa-portelli-deviation-map-2025",
  "title": "Facade Deviation Map \u2014 Villa Portelli exterior (derived from UAV survey 2025)",
  "isAbout": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
  "parentDataset": "https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025",
  "mediaType": "image/tiff",
  "url": "https://heritalise-eccch.eu/resource/files/villa-portelli-deviation-map-2025.tif",
  "persistentIdentifier": "https://doi.org/10.00000/villa-portelli-deviationmap-2025",
  "processingEvent": {
    "processingMethod": "deviation mapping",
    "processingDate": "2025-07-02",
    "software": "CloudCompare",
    "softwareVersion": "2.13.1",
    "operator": "https://orcid.org/0000-0000-0000-0002",
    "parameters": "Reference surface: design BIM mesh (IFC); max search radius 0.5 m; colour ramp \u00b1100 mm",
    "accuracy": "RMS deviation 38 mm; 92 % of facade within \u00b150 mm of design intent",
    "qaResult": "passed with remarks \u2014 two pilasters exceed \u00b180 mm threshold"
  },
  "limitations": "Roof surface excluded (access restrictions). Balcony undersides have partial occlusion.",
  "reviewStatus": "approved"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix crmdig: <http://www.ics.forth.gr/isl/CRMdig/> .
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://heritalise-eccch.eu/resource/product/villa-portelli-deviation-map-2025> a crmdig:D9_Data_Object ;
    dct:description "Roof surface excluded (access restrictions). Balcony undersides have partial occlusion." ;
    dct:format "image/tiff" ;
    dct:title "Facade Deviation Map — Villa Portelli exterior (derived from UAV survey 2025)" ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/building/villa-portelli-main> ;
    crm:P1_is_identified_by "https://doi.org/10.00000/villa-portelli-deviationmap-2025" ;
    crmdig:L11i_was_output_of "{\"accuracy\":\"RMS deviation 38 mm; 92 % of facade within ±50 mm of design intent\",\"operator\":\"https://orcid.org/0000-0000-0000-0002\",\"parameters\":\"Reference surface: design BIM mesh (IFC); max search radius 0.5 m; colour ramp ±100 mm\",\"processingDate\":\"2025-07-02\",\"processingMethod\":\"deviation mapping\",\"qaResult\":\"passed with remarks — two pilasters exceed ±80 mm threshold\",\"software\":\"CloudCompare\",\"softwareVersion\":\"2.13.1\"}"^^rdf:JSON ;
    dcat:accessURL <https://heritalise-eccch.eu/resource/files/villa-portelli-deviation-map-2025.tif> ;
    prov:wasDerivedFrom <https://heritalise-eccch.eu/resource/survey/villa-portelli-uav-2025> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Derived Survey Product
description: A processed output derived from a geometric survey dataset (point cloud,
  photogrammetric model, etc.), modelled as a CRMdig D9 Data Object. Profiles digital-representation
  and adds a mandatory PROV derivation link to the parent survey dataset and a structured
  CRMdig D10 Software Execution sub-object recording the processing method, software
  and quality parameters.
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml
- type: object
  required:
  - type
  - title
  - parentDataset
  - processingEvent
  properties:
    type:
      const: DerivedSurveyProduct
      description: Fixed type token identifying this record as a derived survey product
        (maps to crmdig:D9_Data_Object).
      x-jsonld-id: '@type'
    title:
      type: string
      description: Human-readable title of the derived product (dct:title).
      x-jsonld-id: http://purl.org/dc/terms/title
    parentDataset:
      type: string
      format: uri
      description: Persistent URI of the source survey dataset from which this product
        was derived (prov:wasDerivedFrom). Must reference an ogc.heritage.survey-dataset
        record.
      x-jsonld-id: http://www.w3.org/ns/prov#wasDerivedFrom
      x-jsonld-type: '@id'
    processingEvent:
      type: object
      description: CRMdig D10 Software Execution recording the processing run that
        produced this derived product. Captures the method, software, operator, parameters
        and quality result.
      required:
      - processingMethod
      - processingDate
      properties:
        processingMethod:
          type: string
          description: Type of processing applied, e.g. "mesh generation", "section
            extraction", "deviation mapping", "registration report", "orthophoto generation".
        processingDate:
          type: string
          format: date
          description: Date on which the processing was performed (ISO 8601).
        software:
          type: string
          description: Name of the software used for processing.
        softwareVersion:
          type: string
          description: Version string of the processing software.
        operator:
          type: string
          format: uri
          description: URI of the person or organisation who ran the processing.
        parameters:
          type: string
          description: Key processing parameters or settings used (free-text or JSON-stringified).
        accuracy:
          type: string
          description: "Accuracy or quality metric of the derived product (e.g. \"mean
            deviation \xB12 mm\")."
        qaResult:
          type: string
          description: "Outcome of the quality assurance check, e.g. \"passed\", \"passed
            with remarks\", \"failed \u2014 reprocessing required\"."
      x-jsonld-id: http://www.ics.forth.gr/isl/CRMdig/L11i_was_output_of
      x-jsonld-type: '@json'
    limitations:
      type: string
      description: Known limitations, artefacts or coverage gaps of the derived product.
      x-jsonld-id: http://purl.org/dc/terms/description
    reviewStatus:
      type: string
      description: Workflow status of this record (e.g. draft, reviewed, approved).
x-jsonld-extra-terms:
  DerivedSurveyProduct: http://www.ics.forth.gr/isl/CRMdig/D9_Data_Object
x-jsonld-prefixes:
  crmdig: http://www.ics.forth.gr/isl/CRMdig/
  dct: http://purl.org/dc/terms/
  prov: http://www.w3.org/ns/prov#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/derived-survey-product/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/derived-survey-product/schema.yaml)


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
    "parentDataset": {
      "@id": "prov:wasDerivedFrom",
      "@type": "@id"
    },
    "processingEvent": {
      "@id": "crmdig:L11i_was_output_of",
      "@type": "@json"
    },
    "limitations": "dct:description",
    "DerivedSurveyProduct": "crmdig:D9_Data_Object",
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
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/derived-survey-product/context.jsonld)

## Sources

* [CRMdig D9 Data Object / D10 Software Execution](https://www.ics.forth.gr/isl/CRMdig/)
* [PROV-O: The PROV Ontology](https://www.w3.org/TR/prov-o/)
* [HERITALISE D8.2 CRRS-008 (Derived Survey / QA Product)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/derived-survey-product`

