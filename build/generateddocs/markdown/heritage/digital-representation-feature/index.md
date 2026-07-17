
# Digital Representation Feature (Schema)

`ogc.heritage.digital-representation-feature` *v0.1*

Feature-envelope wrapper for Digital Representation: a spatially located digital asset encoded as a GeoJSON/JSON-FG Feature, or as a topo-feature when geometry is defined by reference/topology.

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### Great Gallery painting photograph — located by embedded point geometry
A digital photograph with an embedded GeoJSON point marking where it was captured. PROV-O provenance (`wasAttributedTo`) stays flat at the top level, inherited from ogc.ogc-utils.prov-entity; `isAbout`/`mediaType`/etc. nest under `properties`. `properties.choType` is left open at this abstract level — concrete profiles (e.g. ogc.heritage.survey-dataset) pin it to their own class.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [7.6273, 45.1344]
  },
  "wasAttributedTo": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "properties": {
    "choType": "DigitalRepresentation",
    "identifier": "RV-GG-014-PHOTO-01",
    "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
    "mediaType": "image/jpeg",
    "persistentIdentifier": "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation-feature/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      7.6273,
      45.1344
    ]
  },
  "wasAttributedTo": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "properties": {
    "choType": "DigitalRepresentation",
    "identifier": "RV-GG-014-PHOTO-01",
    "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
    "mediaType": "image/jpeg",
    "persistentIdentifier": "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-01> a crm:E73_Information_Object,
        geojson:Feature ;
    dct:format "image/jpeg" ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> ;
    crm:P1_is_identified_by "RV-GG-014-PHOTO-01",
        "https://doi.org/10.1234/heritalise.rv-gg-014-photo-01" ;
    prov:wasAttributedTo <https://heritalise-eccch.eu/resource/actor/giulia-bianchi> ;
    geojson:geometry [ a geojson:Point ;
            geojson:coordinates ( 7.6273e+00 4.51344e+01 ) ] .


```


### Great Gallery painting photograph — location by topology reference
The same kind of photograph located by reference instead of embedded coordinates: `geometry` is `null` and `topology.references` points at the depicted bay's own record, using ogc.geo.topo.features.topo-feature to avoid duplicating coordinates.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-02",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
    ]
  },
  "properties": {
    "choType": "DigitalRepresentation",
    "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
    "mediaType": "image/jpeg"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation-feature/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-02",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
    ]
  },
  "properties": {
    "choType": "DigitalRepresentation",
    "isAbout": "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014",
    "mediaType": "image/jpeg"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix topo: <https://purl.org/geojson/topo#> .

<https://heritalise-eccch.eu/resource/digital/great-gallery-painting-014-photo-02> a crm:E73_Information_Object,
        geojson:Feature ;
    dct:format "image/jpeg" ;
    crm:P129_is_about <https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> ;
    geojson:topology [ a geojson:Polygon ;
            topo:relatedFeatures ( <https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Digital Representation Feature
description: "Feature-envelope wrapper for ogc.heritage.digital-representation: a
  spatially located digital asset encoded as a GeoJSON/JSON-FG Feature, or as an ogc.geo.topo.features.topo-feature
  when its geometry is defined by reference/topology rather than embedded coordinates.
  PROV-O's prov-entity (id, provType, wasAttributedTo, wasGeneratedBy, wasDerivedFrom,
  ...) is inherited flat at the top level, since prov-entity does not fix `type` to
  a specific class the way ogc.heritage.heritage-object does; digital-representation's
  own additions (isAbout, mediaType, persistentIdentifier, url) nest under `properties`,
  per GeoJSON convention. Like digital-representation itself, this block asserts no
  fixed CIDOC-CRM class of its own \u2014 `properties.choType` is required but left
  open (no `const`) for concrete profiles (e.g. ogc.heritage.survey-dataset) to pin
  to their own class, the same way digital-representation's other profiles (digital-surrogate,
  oral-history, derived-survey-product) each pin their own `type`."
allOf:
- $ref: https://ogcincubator.github.io/bblock-prov-schema/build/annotated/ogc-utils/prov-entity/schema.yaml
- oneOf:
  - allOf:
    - $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/geo/json-fg/feature-lenient/schema.yaml
    - type: object
      description: Digital representation with its own embedded footprint geometry
        (or none at all).
      not:
        required:
        - topology
  - $ref: https://ogcincubator.github.io/topo-feature/build/annotated/geo/topo/features/topo-feature/schema.yaml
- type: object
  required:
  - id
  properties:
    id:
      type: string
      format: uri
      description: Persistent URI identifying this record (overrides the inherited
        `id`, which also allows a bare number or non-URI string).
    properties:
      allOf:
      - $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation/schema.yaml#/$defs/properties
      - type: object
        required:
        - choType
        properties:
          choType:
            type: string
            description: 'CIDOC-CRM (or CRMdig) class discriminator for this record,
              a second alias to @type alongside the fixed GeoJSON `type: "Feature"`.
              Left open here; concrete profiles of this block pin it to their own
              class (e.g. `"SurveyDataset"` in ogc.heritage.survey-dataset).'
            x-jsonld-id: '@type'
x-jsonld-extra-terms:
  DigitalRepresentation: http://www.cidoc-crm.org/cidoc-crm/E73_Information_Object
  identifier: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
  isAbout:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P129_is_about
    x-jsonld-type: '@id'
  mediaType: http://purl.org/dc/terms/format
  persistentIdentifier: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
  url:
    x-jsonld-id: http://www.w3.org/ns/dcat#accessURL
    x-jsonld-type: '@id'
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  dct: http://purl.org/dc/terms/
  dcat: http://www.w3.org/ns/dcat#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation-feature/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation-feature/schema.yaml)


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
    "Feature": "geojson:Feature",
    "FeatureCollection": "geojson:FeatureCollection",
    "GeometryCollection": "geojson:GeometryCollection",
    "LineString": "geojson:LineString",
    "MultiLineString": "geojson:MultiLineString",
    "MultiPoint": "geojson:MultiPoint",
    "MultiPolygon": "geojson:MultiPolygon",
    "Point": "geojson:Point",
    "Polygon": "geojson:Polygon",
    "features": {
      "@container": "@set",
      "@id": "geojson:features"
    },
    "properties": "@nest",
    "geometry": "geojson:geometry",
    "bbox": {
      "@container": "@list",
      "@id": "geojson:bbox"
    },
    "time": {
      "@context": {
        "date": {
          "@id": "owlTime:hasTime",
          "@type": "xsd:date"
        },
        "timestamp": {
          "@id": "owlTime:hasTime",
          "@type": "xsd:dateTime"
        },
        "interval": {
          "@id": "owlTime:hasTime",
          "@container": "@list"
        }
      },
      "@id": "dct:time"
    },
    "coordRefSys": "http://www.opengis.net/def/glossary/term/CoordinateReferenceSystemCRS",
    "place": "dct:spatial",
    "Polyhedron": "geojson:Polyhedron",
    "MultiPolyhedron": "geojson:MultiPolyhedron",
    "Prism": {
      "@id": "geojson:Prism",
      "@context": {
        "base": "geojson:prismBase",
        "lower": "geojson:prismLower",
        "upper": "geojson:prismUpper"
      }
    },
    "MultiPrism": {
      "@id": "geojson:MultiPrism",
      "@context": {
        "prisms": "geojson:prisms"
      }
    },
    "coordinates": {
      "@container": "@list",
      "@id": "geojson:coordinates"
    },
    "geometries": {
      "@id": "geojson:geometry",
      "@container": "@list"
    },
    "topology": {
      "@context": {
        "references": {
          "@id": "topo:relatedFeatures",
          "@type": "@id",
          "@container": "@list"
        },
        "directed_references": {
          "@context": {
            "ref": {
              "@type": "@id",
              "@id": "topo:ref"
            }
          },
          "@id": "topo:directedReferences",
          "@container": "@list"
        },
        "relationships": {
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
            "length": "dct:extent",
            "role": {
              "@id": "prof:hasRole",
              "@type": "@id"
            },
            "conformsTo": {
              "@id": "dct:conformsTo",
              "@type": "@id"
            }
          },
          "@id": "topo:relatedFeatures",
          "@type": "@id",
          "@container": "@list"
        }
      },
      "@type": "@id",
      "@id": "geojson:topology"
    },
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
    "choType": "@type",
    "DigitalRepresentation": "crm:E73_Information_Object",
    "Arc": "geojson:Arc",
    "ArcWithCenter": "geojson:ArcWithCenter",
    "ArcByChord": "geojson:ArcByChord",
    "CircleByCenter": "geojson:CircleByCenter",
    "CubicSpline": "geojson:CubicSpline",
    "radius": "geojson:radius",
    "arcLength": "geojson:arcLength",
    "startTangentVector": "geojson:startTangentVector",
    "endTangentVector": "geojson:endTangentVector",
    "ref": "topo:ref",
    "orientation": "topo:orientation",
    "Edge": "topo:Edge",
    "Face": "topo:Face",
    "Ring": "topo:Ring",
    "Shell": "topo:Shell",
    "Solid": "topo:Solid",
    "rings": {
      "@id": "topo:rings",
      "@container": "@list"
    },
    "shells": {
      "@id": "topo:shells",
      "@container": "@list"
    },
    "faces": {
      "@id": "topo:faces",
      "@container": "@list"
    },
    "prov": "http://www.w3.org/ns/prov#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "dct": "http://purl.org/dc/terms/",
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "oa": "http://www.w3.org/ns/oa#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "geojson": "https://purl.org/geojson/vocab#",
    "owlTime": "http://www.w3.org/2006/time#",
    "topo": "https://purl.org/geojson/topo#",
    "prof": "http://www.w3.org/ns/dx/prof/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/digital-representation-feature/context.jsonld)

## Sources

* [CIDOC-CRM E73 Information Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E73)
* [PROV-O](https://www.w3.org/TR/prov-o/)
* [OGC topo-feature](https://github.com/opengeospatial/topo-feature)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/digital-representation-feature`

