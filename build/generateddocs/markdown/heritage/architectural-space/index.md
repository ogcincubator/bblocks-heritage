
# Architectural Space (Schema)

`ogc.heritage.architectural-space` *v0.1*

An interior room, bay, hall, zone or level within a building, modelled as CIDOC-CRM E22 Man-Made Object. Profile of heritage-object adding containment hierarchy, historical name crosswalk, floor/bay identifiers and optional IFC space reference.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Architectural Space

An `ArchitecturalSpace` is an interior room, bay, hall, corridor, vault, opening or zone
within a building, modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object-feature`](../heritage-object-feature) and adds a containment
hierarchy, historical name crosswalk, floor/bay identifiers, and an optional IFC IfcSpace
reference (all nested under `properties`).

The space category is conveyed through `properties.objectType` pointing to a Getty AAT spatial
concept (e.g. `aat:300004829` *room*). `properties.choType` is fixed to `"HeritageObject"` from
the parent (a second alias to `@type`, alongside the fixed `type: "Feature"`).

Properties added by this block (all nested under `properties`):

| Property | CRM mapping | Notes |
|---|---|---|
| `parentBuilding` | `crm:P46i_forms_part_of` (@id) | URI of the containing `building` |
| `partOf` | `crm:P46i_forms_part_of` (@id) | URI of a broader space (multi-level hierarchy) |
| `historicalNames` | `crm:P1_is_identified_by` (@json) | Array of `{name, datePeriod?, source?}` objects |
| `floorLevel` | `crm:P1_is_identified_by` | Floor/storey identifier (e.g. "piano nobile") |
| `bayCode` | `crm:P1_is_identified_by` | Bay/module code from a survey or legacy inventory |
| `accessibilityStatus` | `crm:P44_has_condition` | Open/restricted/closed status string |
| `monitoringRelevance` | *(unmapped)* | Boolean flag — no CRM predicate; survives in JSON |
| `ifcSpaceRef` | `crm:P1_is_identified_by` | IFC IfcSpace GUID for HBIM linkage |

Geometry is inherited from `heritage-object-feature`: a top-level `geometry` (GeoJSON Polygon
floor plan or volumetric extent, or omitted entirely), or `geometry: null` with a `topology`
reference into a shared/topological geometry (`ogc.geo.topo.features.topo-feature`).

### Design notes

`historicalNames` stores legacy inventory codes (Guarini room numbers, 19th-century archival
labels) as an opaque JSON array (`@type: "@json"`), avoiding blank-node complexity while
preserving the full structured record for display and search.

`monitoringRelevance` is a boolean operational flag with no natural CRM predicate; it is
intentionally left unmapped in the JSON-LD context so it survives in the JSON payload without
polluting the RDF graph. [`monitoring-point`](../monitoring-point) records reference the space
they are deployed in.

### Use cases

- **REQ-003** — Galleria Grande bays as monitoring and historical location anchors (Venaria)
- **MT-02** — Interior rooms of Villa Portelli with dynamic attributes and oral history links

## Examples

### Galleria Grande bay with historical name crosswalk (REQ-003)
A bay within the Galleria Grande, linked to its parent building, with Guarini inventory names and 19th-century archival labels in historicalNames, floor/bay identifiers, monitoring hotspot flag, and a footprint polygon. Covers REQ-003 (space / room / bay as monitoring and historical location anchor).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [7.6272, 45.1343],
        [7.6275, 45.1343],
        [7.6275, 45.1345],
        [7.6272, 45.1345],
        [7.6272, 45.1343]
      ]
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-SPC-GG-B07",
    "title": "Galleria Grande — Bay 7",
    "description": "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate.",
    "objectType": "http://vocab.getty.edu/aat/300004829",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/galleria-grande",
    "partOf": "https://heritalise-eccch.eu/resource/space/galleria-grande",
    "historicalNames": [
      {
        "name": "Sala VII",
        "datePeriod": "1714/1798",
        "source": "Guarini inventory 1714"
      },
      {
        "name": "Baia Settima",
        "datePeriod": "1800/1870",
        "source": "Archivio di Stato di Torino, sec. XIX"
      }
    ],
    "floorLevel": "piano nobile",
    "bayCode": "B07",
    "accessibilityStatus": "open to public",
    "monitoringRelevance": true
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          7.6272,
          45.1343
        ],
        [
          7.6275,
          45.1343
        ],
        [
          7.6275,
          45.1345
        ],
        [
          7.6272,
          45.1345
        ],
        [
          7.6272,
          45.1343
        ]
      ]
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-SPC-GG-B07",
    "title": "Galleria Grande \u2014 Bay 7",
    "description": "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate.",
    "objectType": "http://vocab.getty.edu/aat/300004829",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/galleria-grande",
    "partOf": "https://heritalise-eccch.eu/resource/space/galleria-grande",
    "historicalNames": [
      {
        "name": "Sala VII",
        "datePeriod": "1714/1798",
        "source": "Guarini inventory 1714"
      },
      {
        "name": "Baia Settima",
        "datePeriod": "1800/1870",
        "source": "Archivio di Stato di Torino, sec. XIX"
      }
    ],
    "floorLevel": "piano nobile",
    "bayCode": "B07",
    "accessibilityStatus": "open to public",
    "monitoringRelevance": true
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    crm:P102_has_title "Galleria Grande — Bay 7" ;
    crm:P1_is_identified_by "[{\"datePeriod\":\"1714/1798\",\"name\":\"Sala VII\",\"source\":\"Guarini inventory 1714\"},{\"datePeriod\":\"1800/1870\",\"name\":\"Baia Settima\",\"source\":\"Archivio di Stato di Torino, sec. XIX\"}]"^^rdf:JSON,
        "B07",
        "RV-SPC-GG-B07",
        "piano nobile" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004829> ;
    crm:P3_has_note "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate." ;
    crm:P44_has_condition "open to public" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/building/galleria-grande>,
        <https://heritalise-eccch.eu/resource/space/galleria-grande> ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 7.6272e+00 4.51343e+01 ) ( 7.6275e+00 4.51343e+01 ) ( 7.6275e+00 4.51345e+01 ) ( 7.6272e+00 4.51345e+01 ) ( 7.6272e+00 4.51343e+01 ) ) ) ] .


```


### Villa Portelli Grand Salon with IFC space reference (MT-02)
The Grand Salon of Villa Portelli as an architectural space linked to its parent building, with IFC IfcSpace reference for HBIM cross-referencing and a monitoring relevance flag (visitor flow and occupancy sensors planned). Covers MT-02 (interior rooms — dynamic attributes and linked oral histories).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/space/villa-portelli-salon",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [14.5132, 35.8962],
        [14.5138, 35.8962],
        [14.5138, 35.8966],
        [14.5132, 35.8966],
        [14.5132, 35.8962]
      ]
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "MT-SPC-VP-SALON",
    "title": "Villa Portelli — Grand Salon",
    "description": "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space.",
    "objectType": "http://vocab.getty.edu/aat/300004733",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
    "floorLevel": "ground floor",
    "accessibilityStatus": "open to public",
    "monitoringRelevance": true,
    "ifcSpaceRef": "3DkP9qRsTwUv2xYzAcBdEf"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/space/villa-portelli-salon",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          14.5132,
          35.8962
        ],
        [
          14.5138,
          35.8962
        ],
        [
          14.5138,
          35.8966
        ],
        [
          14.5132,
          35.8966
        ],
        [
          14.5132,
          35.8962
        ]
      ]
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "MT-SPC-VP-SALON",
    "title": "Villa Portelli \u2014 Grand Salon",
    "description": "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space.",
    "objectType": "http://vocab.getty.edu/aat/300004733",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/villa-portelli-main",
    "floorLevel": "ground floor",
    "accessibilityStatus": "open to public",
    "monitoringRelevance": true,
    "ifcSpaceRef": "3DkP9qRsTwUv2xYzAcBdEf"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/space/villa-portelli-salon> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    crm:P102_has_title "Villa Portelli — Grand Salon" ;
    crm:P1_is_identified_by "3DkP9qRsTwUv2xYzAcBdEf",
        "MT-SPC-VP-SALON",
        "ground floor" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004733> ;
    crm:P3_has_note "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space." ;
    crm:P44_has_condition "open to public" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/building/villa-portelli-main> ;
    geojson:geometry [ a geojson:Polygon ;
            geojson:coordinates ( ( ( 1.45132e+01 3.58962e+01 ) ( 1.45138e+01 3.58962e+01 ) ( 1.45138e+01 3.58966e+01 ) ( 1.45132e+01 3.58966e+01 ) ( 1.45132e+01 3.58962e+01 ) ) ) ] .


```


### Galleria Grande bay with footprint by reference (geometry-by-reference / topology)
Bay 8 of the Galleria Grande is structurally symmetric with bay 7 (see galleria-grande-bay.json) and shares exactly the same floor-plan footprint. Rather than re-embedding identical coordinates, `geometry` is `null` and `topology.references` points at bay 7's own record — the topology-by-reference pattern from ogc.geo.topo.features.topo-feature, reused here to avoid duplicating shared coordinates.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-8",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-SPC-GG-B08",
    "title": "Galleria Grande — Bay 8",
    "description": "The eighth bay of the Galleria Grande, structurally symmetric with bay 7. Its floor-plan footprint coincides exactly with bay 7's, so it is expressed by reference instead of re-embedding the same coordinates.",
    "objectType": "http://vocab.getty.edu/aat/300004829",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/galleria-grande",
    "partOf": "https://heritalise-eccch.eu/resource/space/galleria-grande",
    "floorLevel": "piano nobile",
    "bayCode": "B08",
    "accessibilityStatus": "open to public"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-8",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7"
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-SPC-GG-B08",
    "title": "Galleria Grande \u2014 Bay 8",
    "description": "The eighth bay of the Galleria Grande, structurally symmetric with bay 7. Its floor-plan footprint coincides exactly with bay 7's, so it is expressed by reference instead of re-embedding the same coordinates.",
    "objectType": "http://vocab.getty.edu/aat/300004829",
    "parentBuilding": "https://heritalise-eccch.eu/resource/building/galleria-grande",
    "partOf": "https://heritalise-eccch.eu/resource/space/galleria-grande",
    "floorLevel": "piano nobile",
    "bayCode": "B08",
    "accessibilityStatus": "open to public"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix topo: <https://purl.org/geojson/topo#> .

<https://heritalise-eccch.eu/resource/space/galleria-grande-bay-8> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    crm:P102_has_title "Galleria Grande — Bay 8" ;
    crm:P1_is_identified_by "B08",
        "RV-SPC-GG-B08",
        "piano nobile" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004829> ;
    crm:P3_has_note "The eighth bay of the Galleria Grande, structurally symmetric with bay 7. Its floor-plan footprint coincides exactly with bay 7's, so it is expressed by reference instead of re-embedding the same coordinates." ;
    crm:P44_has_condition "open to public" ;
    crm:P46i_forms_part_of <https://heritalise-eccch.eu/resource/building/galleria-grande>,
        <https://heritalise-eccch.eu/resource/space/galleria-grande> ;
    geojson:topology [ a geojson:Polygon ;
            topo:relatedFeatures ( <https://heritalise-eccch.eu/resource/space/galleria-grande-bay-7> ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Architectural Space
description: 'An interior room, bay, hall, corridor, vault, opening or zone within
  a building, modelled

  as a CIDOC-CRM E22 Man-Made Object. Profiles ogc.heritage.heritage-object-feature
  and adds

  the containment link to a parent building, historical name crosswalk (for legacy

  Guarini-style inventories), floor/bay subdivision identifiers and an optional IFC
  IfcSpace

  reference. Geometry (footprint) is inherited from heritage-object-feature: embedded

  directly, given by reference/topology, or omitted entirely.


  Instances carry `properties.choType: "HeritageObject"` (inherited). Space category
  is

  conveyed via `properties.objectType` (Getty AAT room/space concept).

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/schema.yaml
- type: object
  properties:
    properties:
      type: object
      properties:
        parentBuilding:
          type: string
          format: uri
          description: URI of the building this space belongs to (crm:P46i_forms_part_of).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P46i_forms_part_of
          x-jsonld-type: '@id'
        partOf:
          type: string
          format: uri
          description: URI of a broader space this space is contained within, for
            multi-level hierarchies (e.g. a bay within a gallery wing). Also crm:P46i_forms_part_of.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P46i_forms_part_of
          x-jsonld-type: '@id'
        historicalNames:
          type: array
          description: Historical names or codes for this space from legacy inventories
            (e.g. Guarini room numbers, archival room labels). Each entry has a name,
            an optional date period and an optional source reference.
          items:
            type: object
            properties:
              name:
                type: string
              datePeriod:
                type: string
                description: ISO 8601 date or period string (e.g. "1700/1750").
              source:
                type: string
                description: Reference to the inventory or document where this name
                  appears.
            required:
            - name
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
          x-jsonld-type: '@json'
        floorLevel:
          type: string
          description: Floor or storey identifier (e.g. "ground floor", "piano nobile",
            "1") (crm:P1_is_identified_by).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
        bayCode:
          type: string
          description: Bay or module code within the parent space, from a legacy inventory
            or survey (crm:P1_is_identified_by).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
        accessibilityStatus:
          type: string
          description: Current accessibility status (e.g. "open to public", "restricted",
            "closed for restoration"). Maps to crm:P44_has_condition.
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
        monitoringRelevance:
          type: boolean
          description: True when environmental or condition monitoring equipment is
            deployed in this space (links to monitoring-point records).
        ifcSpaceRef:
          type: string
          description: IFC Global ID (GUID) of the corresponding IfcSpace element
            in an HBIM model, when available (crm:P1_is_identified_by).
          x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
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
    "type": "@type",
    "id": "@id",
    "properties": "@nest",
    "geometry": "geojson:geometry",
    "bbox": {
      "@container": "@list",
      "@id": "geojson:bbox"
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
    "featureType": "@type",
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
    "HeritageObject": "crm:E22_Man-Made_Object",
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
    "parentBuilding": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "partOf": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "historicalNames": {
      "@id": "crm:P1_is_identified_by",
      "@type": "@json"
    },
    "floorLevel": "crm:P1_is_identified_by",
    "bayCode": "crm:P1_is_identified_by",
    "accessibilityStatus": "crm:P44_has_condition",
    "ifcSpaceRef": "crm:P1_is_identified_by",
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
    "parentSpace": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "movementHistory": {
      "@id": "prov:wasUsedBy",
      "@type": "@id",
      "@container": "@set"
    },
    "geojson": "https://purl.org/geojson/vocab#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "oa": "http://www.w3.org/ns/oa#",
    "dct": "http://purl.org/dc/terms/",
    "owlTime": "http://www.w3.org/2006/time#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "topo": "https://purl.org/geojson/topo#",
    "prof": "http://www.w3.org/ns/dx/prof/",
    "prov": "http://www.w3.org/ns/prov#",
    "choType": "@type",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld)

## Sources

* [CIDOC-CRM E22 Man-Made Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E22)
* [HERITALISE D8.2 REQ-003](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/architectural-space`

