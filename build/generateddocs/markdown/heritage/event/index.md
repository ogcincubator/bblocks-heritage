
# Event (Schema)

`ogc.heritage.event` *v0.3*

An event in the history of a heritage object — production, restoration, modification, acquisition — modelled as a CIDOC-CRM E5 Event (generalising E12 Production and E11 Modification) and profiling the PROV-O Activity.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Event

An `Event` is an occurrence in the history of a heritage object — its production, a restoration
or modification campaign, an acquisition — modelled as a CIDOC-CRM `E5_Event` (which generalises
the more specific `E12_Production` and `E11_Modification`).

Like [`actor`](../actor), this block profiles a PROV-O class rather than reinventing event
structure: an `Event` *is* a PROV-O `Activity`. `startedAtTime`/`endedAtTime` carry the
time-span, `wasAssociatedWith` the actor(s) who carried it out, `used`/`generated` the objects
consumed or produced, and `atLocation` where it took place. This block adds only `identifier`
(a local reference, e.g. to a conservation log) and `eventType` (a Getty AAT concept identifying
the kind of event — production, restoration, acquisition...).

As with `place` and `actor`, the uplifted `rdf:type` comes from PROV-O (`prov:Activity`) rather
than `crm:E5_Event` — the SHACL shape here targets by predicate rather than by class.

A `heritage-object`'s production or restoration history is expressed as a set of `Event`s that
reference it via `used` (object worked on) or `generated` (object produced), rather than as an
embedded list on the object itself — so the object record stays stable as new events accumulate.

## HDTO alignment: reused for HC11 Digital Twin Maintenance / HC13 Project

Rather than authoring two more new blocks, this block is reused for HDTO's **HC11 Digital Twin
Maintenance** and **HC13 Project** classes (both reduce to `crm:E5_Event` in the CRM hierarchy —
HC11 via `crm:E65_Creation` ⊑ `E7_Activity` ⊑ `E5_Event`, HC13 via `crm:E7_Activity` ⊑ `E5_Event`;
see `eccch-integration/hdto/03-digital-twin-infrastructure.md`). Unlike the HC3/HC5-8 co-typing
elsewhere in this register, this is **not** derivable from any class an `Event` already asserts —
nothing distinguishes "this is a digital-twin-maintenance activity" from an ordinary
production/restoration event by CRM type alone, so `hdtoType` isn't unconditionally required like
it is on `heritage-object`/`digital-representation`. Instead: set `eventType` to the HDTO class URI
itself (`http://isl.ics.forth.gr/ontology/echoes/HC11_Digital_Twin_Maintenance` or
`.../HC13_Project`) to opt an instance in, and the schema's own `if`/`then` blocks then *require*
`hdtoType` to be set to the matching value — both map via context directly to their respective
predicates (`eventType` → `crm:P2_has_type`, `hdtoType` → `rdf:type`) on a plain JSON-LD parse, no
post-processing step needed. See
[`heritage-digital-twin`](../heritage-digital-twin)/[`heritage-proposition-set`](../heritage-proposition-set)
for how these co-typed `event` instances get referenced (HP19 has composed, HP30 added content).

## Examples

### A documented restoration intervention
A 2024 restoration of the Great Gallery ceiling painting, with a time-span, the object worked on, the conservator who carried it out, and the place where it happened.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/event/restoration-2024",
  "identifier": "RV-INT-2024-03",
  "eventType": "http://vocab.getty.edu/aat/300053683",
  "startedAtTime": "2024-03-04T00:00:00Z",
  "endedAtTime": "2024-06-21T00:00:00Z",
  "used": [
    "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014"
  ],
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "atLocation": "https://heritalise-eccch.eu/resource/place/great-gallery"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/event/restoration-2024",
  "identifier": "RV-INT-2024-03",
  "eventType": "http://vocab.getty.edu/aat/300053683",
  "startedAtTime": "2024-03-04T00:00:00Z",
  "endedAtTime": "2024-06-21T00:00:00Z",
  "used": [
    "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014"
  ],
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "atLocation": "https://heritalise-eccch.eu/resource/place/great-gallery"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/event/restoration-2024> crm:P1_is_identified_by "RV-INT-2024-03" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300053683> ;
    prov:atLocation <https://heritalise-eccch.eu/resource/place/great-gallery> ;
    prov:endedAtTime "2024-06-21T00:00:00+00:00"^^xsd:dateTime ;
    prov:startedAtTime "2024-03-04T00:00:00+00:00"^^xsd:dateTime ;
    prov:used <https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> ;
    prov:wasAssociatedWith <https://heritalise-eccch.eu/resource/actor/giulia-bianchi> .


```


### A minimal record for an undated legacy intervention
Only `identifier` is required at this level — older archive records (UC-V-3) may carry nothing more than a reference to a paper conservation log, with detail added incrementally.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/event/rv-int-1987-legacy",
  "identifier": "RV-INT-1987-LEGACY"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/event/rv-int-1987-legacy",
  "identifier": "RV-INT-1987-LEGACY"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .

<https://heritalise-eccch.eu/resource/event/rv-int-1987-legacy> crm:P1_is_identified_by "RV-INT-1987-LEGACY" .


```


### Malta historical foundation event (HM-09)
The 1820 foundation of the Villa Portelli estate, typed as a ceremony/foundation event (Getty AAT), with the Portelli family as the associated actor and the villa site as the location. `description` carries a free-text historical significance note. Demonstrates use of the event block for historical (non-conservation) events, covering the Malta pilot's HM-09 requirement for historical events with organisational and significance context.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/event/villa-portelli-founding-1820",
  "identifier": "MT-EVT-1820-FOUND",
  "eventType": "http://vocab.getty.edu/aat/300069748",
  "startedAtTime": "1820-01-01T00:00:00Z",
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/house-of-portelli"
  ],
  "atLocation": "https://heritalise-eccch.eu/resource/site/villa-portelli",
  "description": "Foundation ceremony for the Villa Portelli estate, attended by the Portelli family and local clergy. Marks the beginning of the property's documented history as a noble residence."
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/event/villa-portelli-founding-1820",
  "identifier": "MT-EVT-1820-FOUND",
  "eventType": "http://vocab.getty.edu/aat/300069748",
  "startedAtTime": "1820-01-01T00:00:00Z",
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/house-of-portelli"
  ],
  "atLocation": "https://heritalise-eccch.eu/resource/site/villa-portelli",
  "description": "Foundation ceremony for the Villa Portelli estate, attended by the Portelli family and local clergy. Marks the beginning of the property's documented history as a noble residence."
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/event/villa-portelli-founding-1820> crm:P1_is_identified_by "MT-EVT-1820-FOUND" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300069748> ;
    prov:atLocation <https://heritalise-eccch.eu/resource/site/villa-portelli> ;
    prov:startedAtTime "1820-01-01T00:00:00+00:00"^^xsd:dateTime ;
    prov:wasAssociatedWith <https://heritalise-eccch.eu/resource/actor/house-of-portelli> .


```


### HDTO digital twin maintenance activity (HC11)
A digital-twin-maintenance session over the Great Gallery painting, with `eventType` set to the HDTO `hdto:HC11_Digital_Twin_Maintenance` class URI — see the "HDTO alignment" section of this block's description for why this is how HC11/HC13 co-typing is triggered. Referenced from `heritage-digital-twin`'s own example via HP19 has composed.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026",
  "identifier": "HDT-MAINT-2026-01",
  "eventType": "http://isl.ics.forth.gr/ontology/echoes/HC11_Digital_Twin_Maintenance",
  "startedAtTime": "2026-09-01T00:00:00Z",
  "endedAtTime": "2026-09-16T00:00:00Z",
  "used": [
    "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014"
  ],
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "hdtoType": "hdto:HC11_Digital_Twin_Maintenance"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026",
  "identifier": "HDT-MAINT-2026-01",
  "eventType": "http://isl.ics.forth.gr/ontology/echoes/HC11_Digital_Twin_Maintenance",
  "startedAtTime": "2026-09-01T00:00:00Z",
  "endedAtTime": "2026-09-16T00:00:00Z",
  "used": [
    "https://heritalise-eccch.eu/resource/object/great-gallery-painting-014"
  ],
  "wasAssociatedWith": [
    "https://heritalise-eccch.eu/resource/actor/giulia-bianchi"
  ],
  "hdtoType": "hdto:HC11_Digital_Twin_Maintenance"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix hdto: <http://isl.ics.forth.gr/ontology/echoes/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/event/hdt-maintenance-great-gallery-painting-014-2026> a hdto:HC11_Digital_Twin_Maintenance ;
    crm:P1_is_identified_by "HDT-MAINT-2026-01" ;
    crm:P2_has_type hdto:HC11_Digital_Twin_Maintenance ;
    prov:endedAtTime "2026-09-16T00:00:00+00:00"^^xsd:dateTime ;
    prov:startedAtTime "2026-09-01T00:00:00+00:00"^^xsd:dateTime ;
    prov:used <https://heritalise-eccch.eu/resource/object/great-gallery-painting-014> ;
    prov:wasAssociatedWith <https://heritalise-eccch.eu/resource/actor/giulia-bianchi> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Event
description: "An event in the history of a heritage object \u2014 production, restoration,
  modification,\nacquisition \u2014 modelled as a CIDOC-CRM E5 Event. Profiles PROV-O's
  Activity: `startedAtTime`/\n`endedAtTime` carry the time-span (P4_has_time-span),
  `wasAssociatedWith` the actors who\ncarried it out (P14_carried_out_by), `used`/`generated`
  the objects involved (P16_used_specific_object\n/ P108_has_produced / P31_has_modified)
  and `atLocation` where it took place (P7_took_place_at).\nThis block adds a local
  identifier and a CIDOC-CRM event type.\n"
allOf:
- $ref: https://ogcincubator.github.io/bblock-prov-schema/build/annotated/ogc-utils/prov-activity/schema.yaml
- type: object
  required:
  - id
  properties:
    identifier:
      type: string
      description: A local identifier for this event (CIDOC-CRM P1_is_identified_by).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    eventType:
      type: string
      format: uri
      description: "The type of event, typically a Getty AAT concept URI, e.g. production
        or restoration (CIDOC-CRM P2_has_type). Set to an HDTO class URI (hdto:HC11_Digital_Twin_Maintenance
        / hdto:HC13_Project) to identify this event as a digital-twin-maintenance
        or project activity \u2014 see `hdtoType` below, required in that case."
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
      x-jsonld-type: '@id'
    hdtoType:
      type: string
      enum:
      - hdto:HC11_Digital_Twin_Maintenance
      - hdto:HC13_Project
      description: "HDTO co-type (D7.1). Maps via context directly to rdf:type. Not
        applicable to most events (an ordinary production/restoration/acquisition
        has no HC11/HC13 counterpart) \u2014 required only when eventType is itself
        set to the matching HDTO class URI (see the `if`/`then` blocks below), since
        nothing about an Event's CRM/PROV-O type alone distinguishes a digital-twin-maintenance
        or project activity from any other event."
      x-jsonld-id: http://www.w3.org/1999/02/22-rdf-syntax-ns#type
      x-jsonld-type: '@id'
- if:
    properties:
      eventType:
        const: http://isl.ics.forth.gr/ontology/echoes/HC11_Digital_Twin_Maintenance
    required:
    - eventType
  then:
    required:
    - hdtoType
    properties:
      hdtoType:
        const: hdto:HC11_Digital_Twin_Maintenance
        x-jsonld-id: http://www.w3.org/1999/02/22-rdf-syntax-ns#type
        x-jsonld-type: '@id'
- if:
    properties:
      eventType:
        const: http://isl.ics.forth.gr/ontology/echoes/HC13_Project
    required:
    - eventType
  then:
    required:
    - hdtoType
    properties:
      hdtoType:
        const: hdto:HC13_Project
        x-jsonld-id: http://www.w3.org/1999/02/22-rdf-syntax-ns#type
        x-jsonld-type: '@id'
x-jsonld-prefixes:
  rdf: http://www.w3.org/1999/02/22-rdf-syntax-ns#
  crm: http://www.cidoc-crm.org/cidoc-crm/
  hdto: http://isl.ics.forth.gr/ontology/echoes/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "wasInfluencedBy": {
      "@context": {
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
        }
      },
      "@id": "prov:wasInfluencedBy",
      "@type": "@id"
    },
    "qualifiedInfluence": {
      "@context": {
        "influencer": {
          "@context": {
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
            }
          },
          "@id": "prov:influencer",
          "@type": "@id"
        },
        "entity": {
          "@context": {
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
            }
          },
          "@id": "prov:entity",
          "@type": "@id"
        }
      },
      "@id": "prov:qualifiedInfluence",
      "@type": "@id"
    },
    "id": "@id",
    "provType": "@type",
    "activityType": "@type",
    "startedAtTime": {
      "@id": "prov:startedAtTime",
      "@type": "xsd:dateTime"
    },
    "endedAtTime": {
      "@id": "prov:endedAtTime",
      "@type": "xsd:dateTime"
    },
    "wasAssociatedWith": {
      "@id": "prov:wasAssociatedWith",
      "@type": "@id"
    },
    "wasInformedBy": {
      "@id": "prov:wasInformedBy",
      "@type": "@id"
    },
    "used": {
      "@context": {
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
        }
      },
      "@id": "prov:used",
      "@type": "@id"
    },
    "wasStartedBy": {
      "@context": {
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
        }
      },
      "@id": "prov:wasStartedBy",
      "@type": "@id"
    },
    "wasEndedBy": {
      "@context": {
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
        }
      },
      "@id": "prov:wasEndedBy",
      "@type": "@id"
    },
    "invalidated": {
      "@context": {
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
        }
      },
      "@id": "prov:invalidated",
      "@type": "@id"
    },
    "generated": {
      "@context": {
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
        }
      },
      "@id": "prov:generated",
      "@type": "@id"
    },
    "atLocation": {
      "@id": "prov:atLocation",
      "@type": "@id"
    },
    "qualifiedUsage": {
      "@id": "prov:qualifiedUsage",
      "@type": "@id"
    },
    "qualifiedCommunication": {
      "@id": "prov:qualifiedCommunication",
      "@type": "@id"
    },
    "qualifiedStart": {
      "@context": {
        "entity": {
          "@context": {
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
            }
          },
          "@id": "prov:entity",
          "@type": "@id"
        }
      },
      "@id": "prov:qualifiedStart",
      "@type": "@id"
    },
    "qualifiedEnd": {
      "@context": {
        "entity": {
          "@context": {
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
            }
          },
          "@id": "prov:entity",
          "@type": "@id"
        }
      },
      "@id": "prov:qualifiedEnd",
      "@type": "@id"
    },
    "qualifiedAssociation": {
      "@id": "prov:qualifiedAssociation",
      "@type": "@id"
    },
    "agentType": "@type",
    "entityType": "@type",
    "featureType": "@type",
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
    "generatedAtTime": {
      "@id": "prov:generatedAtTime",
      "@type": "xsd:dateTime"
    },
    "invalidatedAtTime": {
      "@id": "prov:invalidatedAtTime",
      "@type": "xsd:dateTime"
    },
    "value": "prov:value",
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
    "alternateOf": {
      "@id": "prov:alternateOf",
      "@type": "@id"
    },
    "entity": {
      "@id": "prov:entity",
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
    "hadMember": {
      "@id": "prov:hadMember",
      "@type": "@id"
    },
    "hadPlan": {
      "@id": "prov:hadPlan",
      "@type": "@id"
    },
    "hadPrimarySource": {
      "@id": "prov:hadPrimarySource",
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
    "qualifiedAttribution": {
      "@id": "prov:qualifiedAttribution",
      "@type": "@id"
    },
    "qualifiedDelegation": {
      "@id": "prov:qualifiedDelegation",
      "@type": "@id"
    },
    "qualifiedDerivation": {
      "@id": "prov:qualifiedDerivation",
      "@type": "@id"
    },
    "qualifiedGeneration": {
      "@id": "prov:qualifiedGeneration",
      "@type": "@id"
    },
    "qualifiedInvalidation": {
      "@id": "prov:qualifiedInvalidation",
      "@type": "@id"
    },
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
    "specializationOf": {
      "@id": "prov:specializationOf",
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
    "wasGeneratedBy": {
      "@id": "prov:wasGeneratedBy",
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
    "has_anchor": {
      "@id": "prov:has_anchor",
      "@type": "@id"
    },
    "has_provenance": {
      "@id": "dct:provenance",
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
    "links": "rdfs:seeAlso",
    "identifier": "crm:P1_is_identified_by",
    "eventType": {
      "@id": "crm:P2_has_type",
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
    "hdto": "http://isl.ics.forth.gr/ontology/echoes/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/event/context.jsonld)

## Sources

* [CIDOC-CRM](https://www.cidoc-crm.org/)
* [PROV-O](https://www.w3.org/TR/prov-o/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/event`

