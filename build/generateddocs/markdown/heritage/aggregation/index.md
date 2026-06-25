
# Aggregation (Schema)

`ogc.heritage.aggregation` *v0.1*

An EDM/ORE aggregation wrapping a heritage object together with its digital representations and the provider/rights metadata required to publish it to Europeana / ECCCH, modelled as edm:Aggregation (ore:Aggregation).

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### A Europeana/ECCCH aggregation for a heritage object
An aggregation wrapping the Great Gallery ceiling painting with its primary display image, a landing page at the holding institution, and the provider/rights metadata Europeana requires for ingestion.
#### json
```json
{
  "id": "https://example.org/heritalise/aggregation/great-gallery-painting-014",
  "type": "Aggregation",
  "aggregatedCHO": "https://example.org/heritalise/object/great-gallery-painting-014",
  "isShownBy": "https://example.org/heritalise/digital/great-gallery-painting-014-photo-01",
  "isShownAt": "https://www.lavenaria.it/en/collections/great-gallery-ceiling",
  "object": "https://example.org/heritalise/digital/great-gallery-painting-014-thumb",
  "dataProvider": "Reggia di Venaria",
  "provider": "ECCCH",
  "rights": "https://rightsstatements.org/page/InC/1.0/"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/aggregation/context.jsonld",
  "id": "https://example.org/heritalise/aggregation/great-gallery-painting-014",
  "type": "Aggregation",
  "aggregatedCHO": "https://example.org/heritalise/object/great-gallery-painting-014",
  "isShownBy": "https://example.org/heritalise/digital/great-gallery-painting-014-photo-01",
  "isShownAt": "https://www.lavenaria.it/en/collections/great-gallery-ceiling",
  "object": "https://example.org/heritalise/digital/great-gallery-painting-014-thumb",
  "dataProvider": "Reggia di Venaria",
  "provider": "ECCCH",
  "rights": "https://rightsstatements.org/page/InC/1.0/"
}
```

#### ttl
```ttl
@prefix edm: <http://www.europeana.eu/schemas/edm/> .
@prefix ore: <http://www.openarchives.org/ore/terms/> .

<https://example.org/heritalise/aggregation/great-gallery-painting-014> a ore:Aggregation ;
    edm:aggregatedCHO <https://example.org/heritalise/object/great-gallery-painting-014> ;
    edm:dataProvider "Reggia di Venaria" ;
    edm:isShownAt <https://www.lavenaria.it/en/collections/great-gallery-ceiling> ;
    edm:isShownBy <https://example.org/heritalise/digital/great-gallery-painting-014-photo-01> ;
    edm:object <https://example.org/heritalise/digital/great-gallery-painting-014-thumb> ;
    edm:provider "ECCCH" ;
    edm:rights <https://rightsstatements.org/page/InC/1.0/> .


```


### A minimal aggregation
Only `aggregatedCHO` is required — the rest can be filled in incrementally as the object is prepared for publishing.
#### json
```json
{
  "aggregatedCHO": "https://example.org/heritalise/object/great-gallery-painting-014"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/aggregation/context.jsonld",
  "aggregatedCHO": "https://example.org/heritalise/object/great-gallery-painting-014"
}
```

#### ttl
```ttl
@prefix edm: <http://www.europeana.eu/schemas/edm/> .

[] edm:aggregatedCHO <https://example.org/heritalise/object/great-gallery-painting-014> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Aggregation
description: "An EDM/ORE aggregation: the wrapper Europeana/ECCCH actually ingests,
  pairing a\n`heritage-object` with the `digital-representation`(s) that depict it
  and the\nprovider/rights metadata needed for publishing (CIDOC-CRM and PROV-O don't
  model this \u2014\nit's an EDM/ORE-specific layer over the heritage object and digital
  representation blocks).\n"
type: object
properties:
  id:
    type: string
    format: uri
    description: The persistent or local URI identifying this aggregation.
    x-jsonld-id: '@id'
  type:
    const: Aggregation
    x-jsonld-id: '@type'
  aggregatedCHO:
    type: string
    format: uri
    description: URI of the `heritage-object` (the "Cultural Heritage Object") this
      aggregation wraps (edm:aggregatedCHO).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/aggregatedCHO
    x-jsonld-type: '@id'
  isShownBy:
    type: string
    format: uri
    description: URI of the primary `digital-representation` used to display the object,
      e.g. a high-resolution image or 3D model (edm:isShownBy).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/isShownBy
    x-jsonld-type: '@id'
  isShownAt:
    type: string
    format: uri
    description: URI of a landing page where the object can be viewed in its original
      context, e.g. the providing institution's own website (edm:isShownAt).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/isShownAt
    x-jsonld-type: '@id'
  hasView:
    type: array
    description: URIs of additional `digital-representation`(s) for this object beyond
      the primary one in `isShownBy` (edm:hasView).
    items:
      type: string
      format: uri
    x-jsonld-id: http://www.europeana.eu/schemas/edm/hasView
    x-jsonld-type: '@id'
    x-jsonld-container: '@set'
  object:
    type: string
    format: uri
    description: URI of a thumbnail image representing the aggregation (edm:object).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/object
    x-jsonld-type: '@id'
  dataProvider:
    type: string
    description: Name of the institution that physically holds the object and supplied
      this metadata (edm:dataProvider).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/dataProvider
  provider:
    type: string
    description: Name of the aggregator that submitted the data on the data provider's
      behalf, e.g. an ECCCH national aggregator (edm:provider).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/provider
  rights:
    type: string
    format: uri
    description: URI of a rights statement governing reuse of the aggregated content,
      typically from rightsstatements.org or a Creative Commons license (edm:rights).
    x-jsonld-id: http://www.europeana.eu/schemas/edm/rights
    x-jsonld-type: '@id'
required:
- aggregatedCHO
x-jsonld-extra-terms:
  Aggregation: http://www.openarchives.org/ore/terms/Aggregation
x-jsonld-prefixes:
  ore: http://www.openarchives.org/ore/terms/
  edm: http://www.europeana.eu/schemas/edm/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/aggregation/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/aggregation/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "Aggregation": "ore:Aggregation",
    "id": "@id",
    "type": "@type",
    "aggregatedCHO": {
      "@id": "edm:aggregatedCHO",
      "@type": "@id"
    },
    "isShownBy": {
      "@id": "edm:isShownBy",
      "@type": "@id"
    },
    "isShownAt": {
      "@id": "edm:isShownAt",
      "@type": "@id"
    },
    "hasView": {
      "@id": "edm:hasView",
      "@type": "@id",
      "@container": "@set"
    },
    "object": {
      "@id": "edm:object",
      "@type": "@id"
    },
    "dataProvider": "edm:dataProvider",
    "provider": "edm:provider",
    "rights": {
      "@id": "edm:rights",
      "@type": "@id"
    },
    "ore": "http://www.openarchives.org/ore/terms/",
    "edm": "http://www.europeana.eu/schemas/edm/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/aggregation/context.jsonld)

## Sources

* [Europeana Data Model (EDM)](https://pro.europeana.eu/page/edm-documentation)
* [OAI-ORE](http://www.openarchives.org/ore/1.0/datamodel)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/aggregation`

