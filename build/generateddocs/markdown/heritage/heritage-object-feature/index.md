
# Heritage Object Feature (Schema)

`ogc.heritage.heritage-object-feature` *v0.1*

Feature-envelope wrapper for Heritage Object: a spatially located CIDOC-CRM E22 Man-Made Object encoded as a GeoJSON/JSON-FG Feature, or as a topo-feature when geometry is defined by reference/topology.

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### North gallery vault, bay 3 — located by embedded point geometry
A heritage object with an embedded GeoJSON point marking its location. `properties` nests the CIDOC-CRM attributes from heritage-object; `choType` carries the crm:E22_Man-Made_Object class as a second alias to `@type`, alongside the fixed `type: "Feature"`.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [7.6187, 45.1337]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-ARCH-NV-003",
    "title": "North gallery vault, bay 3",
    "objectType": "http://vocab.getty.edu/aat/300002862",
    "material": [
      "http://vocab.getty.edu/aat/300010439",
      "http://vocab.getty.edu/aat/300014130"
    ],
    "description": "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown."
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3",
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      7.6187,
      45.1337
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-ARCH-NV-003",
    "title": "North gallery vault, bay 3",
    "objectType": "http://vocab.getty.edu/aat/300002862",
    "material": [
      "http://vocab.getty.edu/aat/300010439",
      "http://vocab.getty.edu/aat/300014130"
    ],
    "description": "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown."
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/object/north-vault-bay-3> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    crm:P102_has_title "North gallery vault, bay 3" ;
    crm:P1_is_identified_by "RV-ARCH-NV-003" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300002862> ;
    crm:P3_has_note "Decorated barrel vault section in the north gallery, third bay from the entrance. Load-bearing masonry with fresco decoration, exhibiting moisture ingress at the crown." ;
    crm:P45_consists_of <http://vocab.getty.edu/aat/300010439>,
        <http://vocab.getty.edu/aat/300014130> ;
    geojson:geometry [ a geojson:Point ;
            geojson:coordinates ( 7.6187e+00 4.51337e+01 ) ] .


```


### North gallery vault, bay 3 — location by topology reference
The same object located by reference instead of embedded coordinates: `geometry` is `null` and `topology.references` points at the parent space's own record, using ogc.geo.topo.features.topo-feature to avoid duplicating coordinates.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3-topo",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/north-gallery"
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-ARCH-NV-003",
    "title": "North gallery vault, bay 3",
    "objectType": "http://vocab.getty.edu/aat/300002862"
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/object/north-vault-bay-3-topo",
  "type": "Feature",
  "geometry": null,
  "topology": {
    "type": "Polygon",
    "references": [
      "https://heritalise-eccch.eu/resource/space/north-gallery"
    ]
  },
  "properties": {
    "choType": "HeritageObject",
    "identifier": "RV-ARCH-NV-003",
    "title": "North gallery vault, bay 3",
    "objectType": "http://vocab.getty.edu/aat/300002862"
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix topo: <https://purl.org/geojson/topo#> .

<https://heritalise-eccch.eu/resource/object/north-vault-bay-3-topo> a crm:E22_Man-Made_Object,
        geojson:Feature ;
    crm:P102_has_title "North gallery vault, bay 3" ;
    crm:P1_is_identified_by "RV-ARCH-NV-003" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300002862> ;
    geojson:topology [ a geojson:Polygon ;
            topo:relatedFeatures ( <https://heritalise-eccch.eu/resource/space/north-gallery> ) ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Heritage Object Feature
description: "Feature-envelope wrapper for ogc.heritage.heritage-object: a spatially
  located heritage object encoded as a GeoJSON/JSON-FG Feature, or as an ogc.geo.topo.features.topo-feature
  when its geometry is defined by reference/topology rather than embedded coordinates.
  heritage-object's CIDOC-CRM attributes nest under `properties` (GeoJSON convention);
  `properties.choType` carries the CIDOC-CRM class (Cultural Heritage Object type)
  as a second alias to `@type`, alongside the fixed `type: \"Feature\"` \u2014 the
  same secondary-typing pattern PROV-O's own prov-entity block uses for its `provType`
  property, kept as a distinct name here since this block has no PROV-O involvement."
allOf:
- oneOf:
  - allOf:
    - $ref: https://opengeospatial.github.io/bblocks/annotated-schemas/geo/json-fg/feature-lenient/schema.yaml
    - type: object
      description: Object with its own embedded footprint geometry (or none at all).
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
        `id`, which also allows a bare number).
    properties:
      allOf:
      - $ref: https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object/schema.yaml#/$defs/properties
      - type: object
        required:
        - choType
        properties:
          choType:
            const: HeritageObject
            description: 'CIDOC-CRM class discriminator for this record (maps to crm:E22_Man-Made_Object
              via a second alias to @type, alongside the fixed GeoJSON `type: "Feature"`).'
            x-jsonld-id: '@type'
x-jsonld-extra-terms:
  HeritageObject: http://www.cidoc-crm.org/cidoc-crm/E22_Man-Made_Object
  identifier: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
  title: http://www.cidoc-crm.org/cidoc-crm/P102_has_title
  description: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  objectType:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
    x-jsonld-type: '@id'
  material:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P45_consists_of
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  currentLocation:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P53_has_former_or_current_location
    x-jsonld-type: '@id'
  persistentIdentifier:
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    x-jsonld-type: '@id'
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/schema.yaml)


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
    "choType": "@type",
    "HeritageObject": "crm:E22_Man-Made_Object",
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
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/heritage-object-feature/context.jsonld)

## Sources

* [CIDOC-CRM](https://www.cidoc-crm.org/)
* [OGC topo-feature](https://github.com/opengeospatial/topo-feature)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/heritage-object-feature`

