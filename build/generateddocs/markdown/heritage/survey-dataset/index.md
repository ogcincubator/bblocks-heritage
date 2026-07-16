
# Survey Dataset (Schema)

`ogc.heritage.survey-dataset` *v0.1*

A geometric survey dataset (point cloud, photogrammetric model or similar) acquired from a heritage asset, modelled as a CRMdig D1 Digital Object with a CRMdig D7 acquisition event and a spatial coverage geometry. Profiles ogc.heritage.digital-representation, adding survey acquisition paradata (method, equipment, CRS, density, accuracy) and a mandatory coverage polygon.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Survey Dataset

A geometric survey dataset — point cloud, photogrammetric model, structured-light scan or similar
product — acquired from a heritage asset. Models the dataset as a **CRMdig D1 Digital Object**
(itself a subclass of CIDOC-CRM E73 Information Object) and captures the acquisition paradata as a
**CRMdig D7 Digital Machine Event** sub-object.

Profiles [`ogc.heritage.digital-representation`](../digital-representation/) and inherits its PROV-O
provenance fields (`wasAttributedTo`, `wasGeneratedBy`, `wasDerivedFrom`) and its `isAbout` link to
the surveyed heritage asset.

## CRM anchor

| Class | Namespace | Role |
|-------|-----------|------|
| `crmdig:D1_Digital_Object` | `http://www.ics.forth.gr/isl/CRMdig/` | The survey dataset itself |
| `crmdig:D7_Digital_Machine_Event` | `http://www.ics.forth.gr/isl/CRMdig/` | The acquisition event (embedded as `acquisitionEvent`) |

The JSON `type: "SurveyDataset"` token maps to `crmdig:D1_Digital_Object` via the JSON-LD context.
The `acquisitionEvent` sub-object maps to `crmdig:L11i_was_output_of` (D1 was output of D7), treated
as an opaque JSON value to avoid deep blank-node expansion.

## Properties

| Property | JSON key | Mapping | Required |
|----------|----------|---------|----------|
| Type token | `type` | `@type` → `crmdig:D1_Digital_Object` | yes |
| Dataset title | `title` | `dct:title` | yes |
| Surveyed asset | `isAbout` | `crm:P129_is_about` (inherited) | yes |
| Acquisition event | `acquisitionEvent` | `crmdig:L11i_was_output_of` | yes |
| — method | `acquisitionEvent.acquisitionMethod` | (opaque JSON) | yes |
| — date | `acquisitionEvent.acquisitionDate` | (opaque JSON) | yes |
| — operator | `acquisitionEvent.operator` | (opaque JSON) | no |
| — equipment | `acquisitionEvent.equipment` | (opaque JSON) | no |
| — CRS | `acquisitionEvent.crs` | (opaque JSON) | no |
| — density | `acquisitionEvent.density` | (opaque JSON) | no |
| — accuracy | `acquisitionEvent.accuracy` | (opaque JSON) | no |
| — processing | `acquisitionEvent.processingHistory` | (opaque JSON) | no |
| Coverage geometry | `coverage` | `geojson:geometry` | yes |
| Media type | `mediaType` | `dct:format` (inherited) | no |
| File location | `url` | `dcat:accessURL` | no |
| Limitations | `limitations` | `dct:description` | no |
| Workflow status | `reviewStatus` | (unmapped) | no |
| Persistent ID | `persistentIdentifier` | `crm:P1_is_identified_by` (inherited) | no |
| PROV attribution | `wasAttributedTo` | `prov:wasAttributedTo` (inherited) | no |
| PROV derivation | `wasDerivedFrom` | `prov:wasDerivedFrom` (inherited) | no |

## Design notes

- **`acquisitionEvent` as opaque JSON** — the D7 sub-object is mapped with `@type: "@json"` to
  prevent coordinate arrays and string fields from being misinterpreted as RDF lists. The
  acquisition paradata is thus preserved in the JSON payload without deep uplift.
- **`isAbout` is required** (inherited from `digital-representation`). It should reference the URI
  of the heritage-site, building, architectural-space or heritage-object that was surveyed.
- **Coverage geometry** uses the `geojson:geometry` predicate (WGS 84 coordinates by default). For
  interior surveys where a 2D WGS 84 polygon is not meaningful, a bounding rectangle or a GeoJSON
  Point at the centroid of the asset is acceptable.
- **Derived products** (processed meshes, deviation maps, registration reports) are modelled as
  `ogc.heritage.derived-survey-product`, which profiles this block and adds a mandatory
  `prov:wasDerivedFrom` link back to the parent survey dataset.

## Use cases

- **Venaria REQ-007** — Point-cloud and photogrammetric survey datasets of buildings and
  decorative systems at Reggia di Venaria Reale.
- **Malta MT-01 / MT-03** — UAV photogrammetric models and 3D scans of Villa Portelli and its
  gardens.

## Examples

### Venaria — TLS point cloud of Galleria Grande ceiling
{
  "type": "SurveyDataset",
  "id": "https://heritage.venaria.it/datasets/gc-ceiling-tls-2024",
  "title": "TLS Point Cloud — Galleria Grande Ceiling (2024)",
  "isAbout": "https://heritage.venaria.it/buildings/galleria-grande",
  "mediaType": "application/vnd.las",
  "acquisitionEvent": {
    "acquisitionMethod": "TLS",
    "acquisitionDate": "2024-03-15",
    "operator": "https://orcid.org/0000-0000-0000-0001",
    "equipment": "Leica RTC360",
    "crs": "https://www.opengis.net/def/crs/EPSG/0/32632",
    "density": "~3500 pts/m²",
    "accuracy": "±3 mm",
    "processingHistory": "Registration: Leica Cyclone REGISTER 360 v1.8; automatic noise filter applied; colourised from concurrent RGB imagery."
  },
  "coverage": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.6025, 45.1290],
        [7.6035, 45.1290],
        [7.6035, 45.1295],
        [7.6025, 45.1295],
        [7.6025, 45.1290]
      ]
    ]
  },
  "url": "https://heritage.venaria.it/storage/gc-ceiling-tls-2024.laz",
  "limitations": "Interior scan only; exterior facade not included. NW corner partially occluded by scaffolding during acquisition.",
  "reviewStatus": "reviewed"
}


### Malta — UAV photogrammetric model of Villa Portelli exterior
{
  "type": "SurveyDataset",
  "id": "https://heritage.gov.mt/datasets/villa-portelli-uav-2025",
  "title": "UAV Photogrammetric Model — Villa Portelli Exterior (2025)",
  "isAbout": "https://heritage.gov.mt/buildings/villa-portelli",
  "mediaType": "model/obj",
  "acquisitionEvent": {
    "acquisitionMethod": "UAV-photogrammetry",
    "acquisitionDate": "2025-06-10",
    "equipment": "DJI Matrice 300 RTK with Zenmuse P1",
    "crs": "https://www.opengis.net/def/crs/EPSG/0/32633",
    "density": "3 cm/pixel ground sampling distance",
    "accuracy": "±5 cm",
    "processingHistory": "Processed in Agisoft Metashape 2.1 at medium quality: dense point cloud → mesh → texture. 42 GCPs used."
  },
  "coverage": {
    "type": "Polygon",
    "coordinates": [
      [
        [14.4320, 35.9020],
        [14.4335, 35.9020],
        [14.4335, 35.9030],
        [14.4320, 35.9030],
        [14.4320, 35.9020]
      ]
    ]
  },
  "url": "https://heritage.gov.mt/storage/villa-portelli-uav-2025.zip",
  "persistentIdentifier": "https://doi.org/10.00000/villa-portelli-survey-2025",
  "reviewStatus": "approved"
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Survey Dataset
description: A geometric survey dataset (point cloud, photogrammetric model, etc.)
  acquired from a heritage asset. Profiles digital-representation (CRMdig D1 / E73
  Information Object) and adds a CRMdig D7 acquisition event sub-object and a mandatory
  spatial coverage geometry.
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml
- type: object
  required:
  - type
  - title
  - acquisitionEvent
  - coverage
  properties:
    type:
      const: SurveyDataset
      description: Fixed type token identifying this record as a survey dataset (maps
        to crmdig:D1_Digital_Object).
      x-jsonld-id: '@type'
    title:
      type: string
      description: Human-readable title of the dataset (dct:title).
      x-jsonld-id: http://purl.org/dc/terms/title
    acquisitionEvent:
      type: object
      description: 'CRMdig D7 Digital Machine Event recording the survey acquisition:
        the method, equipment, operator, date and spatial/quality parameters.'
      required:
      - acquisitionMethod
      - acquisitionDate
      properties:
        acquisitionMethod:
          type: string
          description: Acquisition technique label or controlled-vocabulary URI (e.g.
            "TLS", "UAV-photogrammetry", "SLAM", "structured-light", "terrestrial-photogrammetry").
        acquisitionDate:
          type: string
          format: date
          description: Date (ISO 8601) on which the survey data was collected.
        operator:
          type: string
          format: uri
          description: URI of the person or organisation who carried out the survey.
        equipment:
          type: string
          description: Make and model of the primary acquisition instrument.
        crs:
          type: string
          format: uri
          description: EPSG URI for the coordinate reference system of the output
            data (e.g. https://www.opengis.net/def/crs/EPSG/0/32632).
        density:
          type: string
          description: "Point density or ground sampling distance (e.g. \"~3500 pts/m\xB2\",
            \"3 cm/pixel GSD\")."
        accuracy:
          type: string
          description: "Stated absolute or relative accuracy of the dataset (e.g.
            \"\xB13 mm\")."
        processingHistory:
          type: string
          description: Software and processing steps applied before delivery (registration,
            noise filtering, etc.).
      x-jsonld-id: http://www.ics.forth.gr/isl/CRMdig/L11i_was_output_of
      x-jsonld-type: '@json'
    coverage:
      type: object
      description: GeoJSON geometry delineating the spatial extent of the surveyed
        area (polygon or bounding box in WGS 84 unless stated otherwise in acquisitionEvent.crs).
      required:
      - type
      - coordinates
      properties:
        type:
          type: string
          enum:
          - Point
          - LineString
          - Polygon
          - MultiPoint
          - MultiLineString
          - MultiPolygon
          - GeometryCollection
          x-jsonld-id: '@type'
        coordinates:
          type: array
      x-jsonld-id: https://purl.org/geojson/vocab#geometry
      x-jsonld-type: '@json'
    limitations:
      type: string
      description: Known limitations, coverage gaps or quality caveats of the dataset.
      x-jsonld-id: http://purl.org/dc/terms/description
    reviewStatus:
      type: string
      description: Workflow status of this record (e.g. draft, reviewed, approved).
x-jsonld-extra-terms:
  SurveyDataset: http://www.ics.forth.gr/isl/CRMdig/D1_Digital_Object
x-jsonld-prefixes:
  crmdig: http://www.ics.forth.gr/isl/CRMdig/
  dct: http://purl.org/dc/terms/
  geojson: https://purl.org/geojson/vocab#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/survey-dataset/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/survey-dataset/schema.yaml)


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
    "acquisitionEvent": {
      "@id": "crmdig:L11i_was_output_of",
      "@type": "@json"
    },
    "coverage": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "limitations": "dct:description",
    "SurveyDataset": "crmdig:D1_Digital_Object",
    "prov": "http://www.w3.org/ns/prov#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "dct": "http://purl.org/dc/terms/",
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "oa": "http://www.w3.org/ns/oa#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "crmdig": "http://www.ics.forth.gr/isl/CRMdig/",
    "geojson": "https://purl.org/geojson/vocab#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/survey-dataset/context.jsonld)

## Sources

* [CRMdig D1 Digital Object / D7 Digital Machine Event](https://www.ics.forth.gr/isl/CRMdig/)
* [DCAT Data Catalog Vocabulary v3](https://www.w3.org/TR/vocab-dcat-3/)
* [HERITALISE D8.2 REQ-007 (Survey Dataset)](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/survey-dataset`

