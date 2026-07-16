
# Architectural Space (Schema)

`ogc.heritage.architectural-space` *v0.1*

An interior room, bay, hall, zone or level within a building, modelled as CIDOC-CRM E22 Man-Made Object. Profile of heritage-object adding containment hierarchy, historical name crosswalk, floor/bay identifiers and optional IFC space reference.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Architectural Space

An `ArchitecturalSpace` is an interior room, bay, hall, corridor, vault, opening or zone
within a building, modelled as CIDOC-CRM **E22 Man-Made Object**. It profiles
[`ogc.heritage.heritage-object`](../heritage-object) and adds a containment hierarchy,
historical name crosswalk, floor/bay identifiers, an optional IFC IfcSpace reference, and an
optional GeoJSON footprint for the floor plan or volumetric extent.

The space category is conveyed through `objectType` pointing to a Getty AAT spatial concept
(e.g. `aat:300004829` *room*). The `type` field is fixed to `"HeritageObject"` from the parent.

Properties added by this block:

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
| `footprint` | `geojson:geometry` (@json) | GeoJSON Polygon floor plan or volumetric extent |

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
  "id": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "type": "HeritageObject",
  "identifier": "RV-SPC-GG-B07",
  "title": "Galleria Grande — Bay 7",
  "description": "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate.",
  "objectType": "http://vocab.getty.edu/aat/300004829",
  "parentBuilding": "https://example.org/heritalise/building/galleria-grande",
  "partOf": "https://example.org/heritalise/space/galleria-grande",
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
  "monitoringRelevance": true,
  "footprint": {
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
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld",
  "id": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "type": "HeritageObject",
  "identifier": "RV-SPC-GG-B07",
  "title": "Galleria Grande \u2014 Bay 7",
  "description": "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate.",
  "objectType": "http://vocab.getty.edu/aat/300004829",
  "parentBuilding": "https://example.org/heritalise/building/galleria-grande",
  "partOf": "https://example.org/heritalise/space/galleria-grande",
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
  "monitoringRelevance": true,
  "footprint": {
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
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://example.org/heritalise/space/galleria-grande-bay-7> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Galleria Grande — Bay 7" ;
    crm:P1_is_identified_by "[{\"datePeriod\":\"1714/1798\",\"name\":\"Sala VII\",\"source\":\"Guarini inventory 1714\"},{\"datePeriod\":\"1800/1870\",\"name\":\"Baia Settima\",\"source\":\"Archivio di Stato di Torino, sec. XIX\"}]"^^rdf:JSON,
        "B07",
        "RV-SPC-GG-B07",
        "piano nobile" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004829> ;
    crm:P3_has_note "The seventh bay of the Galleria Grande, featuring a painted vault and lateral niches. Key monitoring hotspot for humidity and microclimate." ;
    crm:P44_has_condition "open to public" ;
    crm:P46i_forms_part_of <https://example.org/heritalise/building/galleria-grande>,
        <https://example.org/heritalise/space/galleria-grande> ;
    geojson:geometry "{\"coordinates\":[[[7.6272,45.1343],[7.6275,45.1343],[7.6275,45.1345],[7.6272,45.1345],[7.6272,45.1343]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```


### Villa Portelli Grand Salon with IFC space reference (MT-02)
The Grand Salon of Villa Portelli as an architectural space linked to its parent building, with IFC IfcSpace reference for HBIM cross-referencing and a monitoring relevance flag (visitor flow and occupancy sensors planned). Covers MT-02 (interior rooms — dynamic attributes and linked oral histories).
#### json
```json
{
  "id": "https://example.org/heritalise/space/villa-portelli-salon",
  "type": "HeritageObject",
  "identifier": "MT-SPC-VP-SALON",
  "title": "Villa Portelli — Grand Salon",
  "description": "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space.",
  "objectType": "http://vocab.getty.edu/aat/300004733",
  "parentBuilding": "https://example.org/heritalise/building/villa-portelli-main",
  "floorLevel": "ground floor",
  "accessibilityStatus": "open to public",
  "monitoringRelevance": true,
  "ifcSpaceRef": "3DkP9qRsTwUv2xYzAcBdEf",
  "footprint": {
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
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/context.jsonld",
  "id": "https://example.org/heritalise/space/villa-portelli-salon",
  "type": "HeritageObject",
  "identifier": "MT-SPC-VP-SALON",
  "title": "Villa Portelli \u2014 Grand Salon",
  "description": "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space.",
  "objectType": "http://vocab.getty.edu/aat/300004733",
  "parentBuilding": "https://example.org/heritalise/building/villa-portelli-main",
  "floorLevel": "ground floor",
  "accessibilityStatus": "open to public",
  "monitoringRelevance": true,
  "ifcSpaceRef": "3DkP9qRsTwUv2xYzAcBdEf",
  "footprint": {
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
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://example.org/heritalise/space/villa-portelli-salon> a crm:E22_Man-Made_Object ;
    crm:P102_has_title "Villa Portelli — Grand Salon" ;
    crm:P1_is_identified_by "3DkP9qRsTwUv2xYzAcBdEf",
        "MT-SPC-VP-SALON",
        "ground floor" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300004733> ;
    crm:P3_has_note "The principal reception room of Villa Portelli, used for cultural events and visitor interpretation. Linked oral histories are associated with this space." ;
    crm:P44_has_condition "open to public" ;
    crm:P46i_forms_part_of <https://example.org/heritalise/building/villa-portelli-main> ;
    geojson:geometry "{\"coordinates\":[[[14.5132,35.8962],[14.5138,35.8962],[14.5138,35.8966],[14.5132,35.8966],[14.5132,35.8962]]],\"type\":\"Polygon\"}"^^rdf:JSON .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Architectural Space
description: 'An interior room, bay, hall, corridor, vault, opening or zone within
  a building, modelled

  as a CIDOC-CRM E22 Man-Made Object. Profiles ogc.heritage.heritage-object and adds
  the

  containment link to a parent building, historical name crosswalk (for legacy Guarini-style

  inventories), floor/bay subdivision identifiers, optional IFC IfcSpace reference,
  and an

  optional geometry for the spatial footprint or volume.


  Instances have type "HeritageObject" (inherited). Space category is conveyed via
  objectType

  (Getty AAT room/space concept).

  '
allOf:
- $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.yaml
- type: object
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
      description: URI of a broader space this space is contained within, for multi-level
        hierarchies (e.g. a bay within a gallery wing). Also crm:P46i_forms_part_of.
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
            description: Reference to the inventory or document where this name appears.
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
      description: True when environmental or condition monitoring equipment is deployed
        in this space (links to monitoring-point records).
    ifcSpaceRef:
      type: string
      description: IFC Global ID (GUID) of the corresponding IfcSpace element in an
        HBIM model, when available (crm:P1_is_identified_by).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    footprint:
      type: object
      description: Optional GeoJSON geometry representing the space's floor plan polygon
        or volumetric extent.
      properties:
        type:
          type: string
          enum:
          - Point
          - Polygon
          - MultiPolygon
        coordinates:
          type: array
      required:
      - type
      - coordinates
      x-jsonld-id: https://purl.org/geojson/vocab#geometry
      x-jsonld-type: '@json'
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  geojson: https://purl.org/geojson/vocab#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/architectural-space/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "HeritageObject": "crm:E22_Man-Made_Object",
    "parentSpace": {
      "@id": "crm:P46i_forms_part_of",
      "@type": "@id"
    },
    "movementHistory": {
      "@id": "prov:wasUsedBy",
      "@type": "@id",
      "@container": "@set"
    },
    "id": "@id",
    "type": "@type",
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
    "footprint": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "prov": "http://www.w3.org/ns/prov#",
    "geojson": "https://purl.org/geojson/vocab#",
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

