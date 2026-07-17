
# Observation (Schema)

`ogc.heritage.observation` *v0.1*

A scientific observation or measurement of a heritage object or place — environmental monitoring, condition assessment, conservation science — profiling the SOSA/SSN Observation. `hasFeatureOfInterest` ties the reading back to the heritage object or place being measured (CIDOC-CRM/CRMsci's scientific observation context); `madeBySensor` is expected to reference an OGC SensorThings API `Sensor`.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Observation

An `Observation` is a scientific measurement or environmental reading taken of a heritage
object or place — microclimate monitoring, condition assessment, conservation science — using
the W3C SOSA/SSN vocabulary as its primary standard.

This block profiles
[`ogc.sosa.properties.observation`](https://opengeospatial.github.io/ogcapi-sosa/bblock/ogc.sosa.properties.observation),
inheriting `observedProperty`, `madeBySensor`, `hasFeatureOfInterest`,
`hasResult`/`hasSimpleResult`, `resultTime`, and `phenomenonTime`. The only additions are:

- `type` is fixed to `"Observation"` (mapped to `sosa:Observation` in the context) so that
  uplifted RDF is properly typed — the profiled block's own schema does not set this.
- `hasFeatureOfInterest` is tightened from optional to **required**: every reading in this
  register is expected to link back to the `heritage-object` or `place` it was taken of or at.

This narrowing reflects the CIDOC-CRM/CRMsci *scientific observation context*: the observation
is meaningful only in relation to the heritage resource being studied or monitored.

### Integration with SensorThings API

D8.2 profile A mandates OGC SensorThings API (STA) as the operational monitoring API. The
`madeBySensor` property should reference an `ogc.api.sta.Sensor` resource; the
`hasFeatureOfInterest` should reference the `ogc.api.sta.FeatureOfInterest` or, for the
heritage domain link, a [`place`](../place) or [`heritage-object`](../heritage-object) URI.
Actual sensor readings (values, timestamps, units) live in STA `Observation` resources; this
block is the CRM-anchored representation linking them back to the heritage graph.

For observations that need an independent geometry (a sensor not co-located with any existing
`place`), wrap the properties from this block inside `ogc.sosa.features.observation` instead.

### Use cases

- **Profile A** — environmental monitoring (temperature, humidity, light) at Venaria and Malta
- **REQ-010** — sensor reading linked to a `monitoring-point` via `hasFeatureOfInterest`

## Examples

### A relative humidity reading linked back to its CIDOC-CRM context
A humidity measurement of the Great Gallery, taken by an OGC SensorThings API `Sensor`. `hasFeatureOfInterest`, `observedProperty`, `madeBySensor`, `hasSimpleResult` and `resultTime` all come straight from the profiled SOSA Observation; only `hasFeatureOfInterest` (here pointing at the `place` block's URI) is required by this profile.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/observation/great-gallery-humidity-2024-03-04t12-00-00z",
  "type": "Observation",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/place/great-gallery",
  "observedProperty": "http://vocab.getty.edu/aat/300055680",
  "madeBySensor": "https://heritalise-eccch.eu/resource/sta/v1.1/Sensors(12)",
  "hasSimpleResult": 54.2,
  "resultTime": "2024-03-04T12:00:00Z"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/observation/great-gallery-humidity-2024-03-04t12-00-00z",
  "type": "Observation",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/place/great-gallery",
  "observedProperty": "http://vocab.getty.edu/aat/300055680",
  "madeBySensor": "https://heritalise-eccch.eu/resource/sta/v1.1/Sensors(12)",
  "hasSimpleResult": 54.2,
  "resultTime": "2024-03-04T12:00:00Z"
}
```

#### ttl
```ttl
@prefix sosa: <http://www.w3.org/ns/sosa/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/observation/great-gallery-humidity-2024-03-04t12-00-00z> a sosa:Observation ;
    sosa:hasFeatureOfInterest <https://heritalise-eccch.eu/resource/place/great-gallery> ;
    sosa:hasSimpleResult 5.42e+01 ;
    sosa:madeBySensor <https://heritalise-eccch.eu/resource/sta/v1.1/Sensors(12)> ;
    sosa:observedProperty <http://vocab.getty.edu/aat/300055680> ;
    sosa:resultTime "2024-03-04T12:00:00Z" .


```


### A minimal record for an undated legacy reading
Only `hasFeatureOfInterest` and one of `hasResult`/`hasSimpleResult` are required — useful for migrating legacy monitoring logs that predate the SensorThings API deployment.
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/observation/legacy-rh-reading-001",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/place/great-gallery",
  "observedProperty": "http://vocab.getty.edu/aat/300379098",
  "hasSimpleResult": "RH within tolerance, see paper log"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/observation/legacy-rh-reading-001",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/place/great-gallery",
  "observedProperty": "http://vocab.getty.edu/aat/300379098",
  "hasSimpleResult": "RH within tolerance, see paper log"
}
```

#### ttl
```ttl
@prefix sosa: <http://www.w3.org/ns/sosa/> .

<https://heritalise-eccch.eu/resource/observation/legacy-rh-reading-001> sosa:hasFeatureOfInterest <https://heritalise-eccch.eu/resource/place/great-gallery> ;
    sosa:hasSimpleResult <file:///github/workspace/> ;
    sosa:observedProperty <http://vocab.getty.edu/aat/300379098> .


```


### Sensor reading linked to a monitoring point (CRRS-010)
A relative humidity reading taken by a monitoring point in the Reggia di Venaria north gallery. `hasFeatureOfInterest` and `madeBySensor` both reference the monitoring-point URI (the point is both the feature being monitored and the sensor host), `observedProperty` carries a Getty AAT URI for relative humidity, and `resultTime` is the ISO 8601 timestamp of the reading. Covers CRRS-010 (sensor readings linked to CRRS-009 monitoring points).
#### json
```json
{
  "id": "https://heritalise-eccch.eu/resource/observation/north-gallery-mp-01-2024-09-15t08-30-00z",
  "type": "Observation",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01",
  "observedProperty": "http://vocab.getty.edu/aat/300055680",
  "madeBySensor": "https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01",
  "hasSimpleResult": 61.4,
  "resultTime": "2024-09-15T08:30:00Z"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/context.jsonld",
  "id": "https://heritalise-eccch.eu/resource/observation/north-gallery-mp-01-2024-09-15t08-30-00z",
  "type": "Observation",
  "hasFeatureOfInterest": "https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01",
  "observedProperty": "http://vocab.getty.edu/aat/300055680",
  "madeBySensor": "https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01",
  "hasSimpleResult": 61.4,
  "resultTime": "2024-09-15T08:30:00Z"
}
```

#### ttl
```ttl
@prefix sosa: <http://www.w3.org/ns/sosa/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise-eccch.eu/resource/observation/north-gallery-mp-01-2024-09-15t08-30-00z> a sosa:Observation ;
    sosa:hasFeatureOfInterest <https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01> ;
    sosa:hasSimpleResult 6.14e+01 ;
    sosa:madeBySensor <https://heritalise-eccch.eu/resource/monitoring-point/north-gallery-mp-01> ;
    sosa:observedProperty <http://vocab.getty.edu/aat/300055680> ;
    sosa:resultTime "2024-09-15T08:30:00Z" .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Observation
description: 'A scientific observation or measurement, profiling the SOSA/SSN Observation
  rather than

  redefining it: `observedProperty`, `madeBySensor`, `hasFeatureOfInterest`,

  `hasResult`/`hasSimpleResult` and `resultTime`/`phenomenonTime` all come from

  `ogc.sosa.properties.observation`. This block only narrows `hasFeatureOfInterest`
  from

  optional to required, since every reading in this register is expected to reference
  the

  `heritage-object` or `place` it was taken of/at by URI (the CIDOC-CRM/CRMsci "scientific

  observation context"), and sets `type` so the uplifted graph is actually typed

  `sosa:Observation` (the profiled block''s own examples never set it). For observations
  that

  need their own geometry independent of an existing `place`, compose the properties
  from this

  block inside `ogc.sosa.features.observation` instead.

  '
allOf:
- $ref: https://opengeospatial.github.io/ogcapi-sosa/build/annotated/sosa/properties/observation/schema.yaml
- type: object
  required:
  - id
  - hasFeatureOfInterest
  properties:
    type:
      const: Observation
      x-jsonld-id: '@type'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "id": "@id",
    "properties": "@nest",
    "featureType": "@type",
    "ActuatableProperty": {
      "@id": "sosa:ActuatableProperty",
      "@type": "@id"
    },
    "Actuation": {
      "@id": "sosa:Actuation",
      "@type": "@id"
    },
    "ActuationCollection": {
      "@id": "sosa:ActuationCollection",
      "@type": "@id"
    },
    "Actuator": {
      "@id": "sosa:Actuator",
      "@type": "@id"
    },
    "Deployment": {
      "@id": "sosa:Deployment",
      "@type": "@id"
    },
    "Execution": {
      "@id": "sosa:Execution",
      "@type": "@id"
    },
    "FeatureOfInterest": {
      "@id": "sosa:FeatureOfInterest",
      "@type": "@id"
    },
    "ObservableProperty": {
      "@id": "sosa:ObservableProperty",
      "@type": "@id"
    },
    "Observation": {
      "@id": "sosa:Observation",
      "@type": "@id"
    },
    "ObservationCollection": {
      "@id": "sosa:ObservationCollection",
      "@type": "@id"
    },
    "Platform": {
      "@id": "sosa:Platform",
      "@type": "@id"
    },
    "Property": {
      "@id": "sosa:Property",
      "@type": "@id"
    },
    "Procedure ": {
      "@id": "sosa:Procedure",
      "@type": "@id"
    },
    "Sample": {
      "@id": "sosa:Sample",
      "@type": "@id"
    },
    "SampleCollection": {
      "@id": "sosa:SampleCollection",
      "@type": "@id"
    },
    "Sampler": {
      "@id": "sosa:Sampler",
      "@type": "@id"
    },
    "Sampling": {
      "@id": "sosa:Sampling",
      "@type": "@id"
    },
    "Sensor": {
      "@id": "sosa:Sensor",
      "@type": "@id"
    },
    "Stimulus": {
      "@id": "sosa:Stimulus",
      "@type": "@id"
    },
    "System": {
      "@id": "sosa:System",
      "@type": "@id"
    },
    "actsOnProperty": {
      "@id": "sosa:actsOnProperty",
      "@type": "@id"
    },
    "deployedOnPlatform": {
      "@id": "sosa:deployedOnPlatform",
      "@type": "@id"
    },
    "deployedSystem": {
      "@id": "sosa:deployedSystem",
      "@type": "@id"
    },
    "detects": {
      "@id": "sosa:detects",
      "@type": "@id"
    },
    "features": {
      "@id": "sosa:hasMember",
      "@type": "@id"
    },
    "forProperty": {
      "@id": "sosa:forProperty",
      "@type": "@id"
    },
    "hasDeployment": {
      "@id": "sosa:hasDeployment",
      "@type": "@id"
    },
    "hasInput": {
      "@id": "sosa:hasInput",
      "@type": "@id"
    },
    "hasMember": {
      "@id": "sosa:hasMember",
      "@type": "@id"
    },
    "hasOriginalSample": {
      "@id": "sosa:hasOriginalSample",
      "@type": "@id"
    },
    "hasOutput": {
      "@id": "sosa:hasOutput",
      "@type": "@id"
    },
    "hasProperty": {
      "@id": "sosa:hasProperty",
      "@type": "@id"
    },
    "hasResult": {
      "@id": "sosa:hasResult",
      "@type": "@id"
    },
    "hasResultQuality": {
      "@id": "sosa:hasResultQuality",
      "@type": "@id"
    },
    "hasSample": {
      "@id": "sosa:hasSample",
      "@type": "@id"
    },
    "hasSampledFeature": {
      "@id": "sosa:hasSampledFeature",
      "@type": "@id"
    },
    "hasSimpleResult": {
      "@id": "sosa:hasSimpleResult",
      "@type": "@id"
    },
    "hasSubSystem": {
      "@id": "sosa:hasSubSystem",
      "@type": "@id",
      "@container": "@set"
    },
    "hasUltimateFeatureOfInterest": {
      "@id": "sosa:hasUltimateFeatureOfInterest",
      "@type": "@id"
    },
    "hosts": {
      "@id": "sosa:hosts",
      "@type": "@id",
      "@container": "@set"
    },
    "implementedBy": {
      "@id": "sosa:implementedBy",
      "@type": "@id"
    },
    "implements": {
      "@id": "sosa:implements",
      "@type": "@id"
    },
    "inDeployment": {
      "@id": "sosa:inDeployment",
      "@type": "@id"
    },
    "isActedOnBy": {
      "@id": "sosa:isActedOnBy",
      "@type": "@id"
    },
    "isFeatureOfInterestOf": {
      "@id": "sosa:isFeatureOfInterestOf",
      "@type": "@id"
    },
    "isHostedBy": {
      "@id": "sosa:isHostedBy",
      "@type": "@id"
    },
    "isObservedBy": {
      "@id": "sosa:isObservedBy",
      "@type": "@id"
    },
    "isPropertyOf": {
      "@id": "sosa:isPropertyOf",
      "@type": "@id"
    },
    "isProxyFor": {
      "@id": "sosa:isProxyFor",
      "@type": "@id"
    },
    "isResultOf": {
      "@id": "sosa:isResultOf",
      "@type": "@id"
    },
    "isResultOfMadeBySampler": {
      "@id": "sosa:isResultOfMadeBySampler",
      "@type": "@id"
    },
    "isResultOfUsedProcedure": {
      "@id": "sosa:isResultOfUsedProcedure",
      "@type": "@id"
    },
    "isSampleOf": {
      "@id": "sosa:isSampleOf",
      "@type": "@id"
    },
    "madeActuation": {
      "@id": "sosa:madeActuation",
      "@type": "@id"
    },
    "madeByActuator": {
      "@id": "sosa:madeByActuator",
      "@type": "@id"
    },
    "madeBySampler": {
      "@id": "sosa:madeBySampler",
      "@type": "@id"
    },
    "madeObservation": {
      "@id": "sosa:madeObservation",
      "@type": "@id"
    },
    "madeSampling": {
      "@id": "sosa:madeSampling",
      "@type": "@id"
    },
    "observes": {
      "@id": "sosa:observes",
      "@type": "@id"
    },
    "wasOriginatedBy": {
      "@id": "sosa:wasOriginatedBy",
      "@type": "@id"
    },
    "Accuracy": {
      "@id": "ssn-system:Accuracy",
      "@type": "@id"
    },
    "ActuationRange": {
      "@id": "ssn-system:ActuationRange",
      "@type": "@id"
    },
    "BatteryLifetime": {
      "@id": "ssn-system:BatteryLifetime",
      "@type": "@id"
    },
    "DetectionLimit": {
      "@id": "ssn-system:DetectionLimit",
      "@type": "@id"
    },
    "Drift": {
      "@id": "ssn-system:Drift",
      "@type": "@id"
    },
    "Frequency": {
      "@id": "ssn-system:Frequency",
      "@type": "@id"
    },
    "Latency": {
      "@id": "ssn-system:Latency",
      "@type": "@id"
    },
    "MaintenanceSchedule": {
      "@id": "ssn-system:MaintenanceSchedule",
      "@type": "@id"
    },
    "MeasurementRange": {
      "@id": "ssn-system:MeasurementRange",
      "@type": "@id"
    },
    "OperatingPowerRange": {
      "@id": "ssn-system:OperatingPowerRange",
      "@type": "@id"
    },
    "OperatingProperty": {
      "@id": "ssn-system:OperatingProperty",
      "@type": "@id"
    },
    "OperatingRange": {
      "@id": "ssn-system:OperatingRange",
      "@type": "@id"
    },
    "Precision": {
      "@id": "ssn-system:Precision",
      "@type": "@id"
    },
    "Resolution": {
      "@id": "ssn-system:Resolution",
      "@type": "@id"
    },
    "ResponseTime": {
      "@id": "ssn-system:ResponseTime",
      "@type": "@id"
    },
    "Selectivity": {
      "@id": "ssn-system:Selectivity",
      "@type": "@id"
    },
    "Sensitivity": {
      "@id": "ssn-system:Sensitivity",
      "@type": "@id"
    },
    "SurvivalProperty": {
      "@id": "ssn-system:SurvivalProperty",
      "@type": "@id"
    },
    "SystemLifetime": {
      "@id": "ssn-system:SystemLifetime",
      "@type": "@id"
    },
    "SurvivalRange": {
      "@id": "ssn-system:SurvivalRange",
      "@type": "@id"
    },
    "SystemCapability": {
      "@id": "ssn-system:SystemCapability",
      "@type": "@id"
    },
    "SystemProperty": {
      "@id": "ssn-system:SystemProperty",
      "@type": "@id"
    },
    "hasOperatingProperty": {
      "@id": "ssn-system:hasOperatingProperty",
      "@type": "@id"
    },
    "hasOperatingRange": {
      "@id": "ssn-system:hasOperatingRange",
      "@type": "@id"
    },
    "hasSurvivalProperty": {
      "@id": "ssn-system:hasSurvivalProperty",
      "@type": "@id"
    },
    "hasSystemCapability": {
      "@id": "ssn-system:hasSystemCapability",
      "@type": "@id"
    },
    "hasSystemProperty": {
      "@id": "ssn-system:hasSystemProperty",
      "@type": "@id"
    },
    "hasSurvivalRange": {
      "@id": "ssn-system:hasSurvivalRange",
      "@type": "@id"
    },
    "inCondition": {
      "@id": "ssn-system:inCondition",
      "@type": "@id"
    },
    "qualityOfObservation": {
      "@id": "ssn-system:qualityOfObservation",
      "@type": "@id"
    },
    "resultTime": "sosa:resultTime",
    "phenomenonTime": {
      "@id": "sosa:phenomenonTime",
      "@type": "@id"
    },
    "hasFeatureOfInterest": {
      "@id": "sosa:hasFeatureOfInterest",
      "@type": "@id"
    },
    "observedProperty": {
      "@id": "sosa:observedProperty",
      "@type": "@id"
    },
    "usedProcedure": {
      "@id": "sosa:usedProcedure",
      "@type": "@id"
    },
    "madeBySensor": {
      "@id": "sosa:madeBySensor",
      "@type": "@id"
    },
    "type": "@type",
    "sosa": "http://www.w3.org/ns/sosa/",
    "ssn-system": "ssn:systems/",
    "ssn": "http://www.w3.org/ns/ssn/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/observation/context.jsonld)

## Sources

* [SOSA/SSN](https://www.w3.org/TR/vocab-ssn/)
* [CIDOC-CRM](https://www.cidoc-crm.org/)
* [CRMsci](https://www.cidoc-crm.org/crmsci/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/observation`

