
# Monitoring Point (Schema)

`ogc.heritage.monitoring-point` *v0.1*

A physical sensor or monitoring device installed at a heritage resource, modelled as a SOSA Sensor with a CIDOC-CRM E22 Man-Made Object identity layer. Profiles ogc.sosa.properties.sensor.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

## Monitoring Point

A `MonitoringPoint` is a physical sensor or monitoring device installed at a heritage resource
for environmental and condition monitoring. It profiles
[`ogc.sosa.properties.sensor`](https://www.w3.org/TR/vocab-ssn/#SOSASensor) — the SOSA/SSN
Sensor entity representing a device that can generate observations pertaining to an observable
property — and adds a CIDOC-CRM **E22 Man-Made Object** identity layer.

The `type` field is set to `"MonitoringPoint"`, which the JSON-LD context maps to
`crm:E22_Man-Made_Object`, enabling SHACL targeting by class.

Properties added by this block:

| Property | CRM mapping | Notes |
|---|---|---|
| `type` (required) | `crm:E22_Man-Made_Object` | Fixed const; enables `sh:targetClass` |
| `identifier` (required) | `crm:P1_is_identified_by` | Local pilot inventory code |
| `sensorType` (required) | `crm:P2_has_type` (@id) | Getty AAT sensor/equipment category URI |
| `locatedIn` (required) | `crm:P53_has_former_or_current_location` (@id) | URI of containing `architectural-space`, `heritage-object`, or `place` |
| `position` (required) | `geojson:geometry` (@json) | GeoJSON Point; use 3D coordinates where known |
| `operationalStatus` | `crm:P44_has_condition` | "active", "decommissioned", "under maintenance" |
| `responsibleOrganisation` | `crm:P50_has_current_keeper` | Operating body name or URI |

SOSA properties inherited from `ogc.sosa.properties.sensor` (e.g. `sosa:observes`,
`sosa:isHostedBy`, `sosa:madeObservation`) may be used alongside these CRM properties.

### Observation integration

Individual sensor readings are represented using the [`ogc.heritage.observation`](../observation)
block (SOSA/SSN Observation). The `locatedIn` property on this block provides the static CRM
location anchor. `sosa:hasFeatureOfInterest` on the observation should reference the heritage
resource being monitored (the `architectural-space`, `heritage-object`, or `place` identified
in `locatedIn`), not the sensor itself.

### Use cases

- **REQ-009** — Physical sensor positions and MHS hotspots at Reggia di Venaria (microclimate
  monitoring in the Galleria Grande and other principal spaces)
- **REQ-010** — Anchor for `ogc.heritage.observation` records: `hasFeatureOfInterest` on an
  observation can reference the `locatedIn` resource from this block

## Examples

### Relative humidity sensor, Galleria Grande Bay 7 (REQ-009)
A capacitive RH sensor mounted in bay 7 of the Galleria Grande, linked to the architectural-space record for that bay and positioned with a 3D GeoJSON point. Covers REQ-009 (monitoring point / MHS hotspot at Venaria).
#### json
```json
{
  "type": "MonitoringPoint",
  "name": "Galleria Grande Bay 7 — Relative Humidity Sensor RH01",
  "description": "Capacitive relative humidity sensor mounted at 2.5 m height on the north wall of bay 7, Galleria Grande. Primary MHS hotspot for microclimate monitoring in the painted vault zone.",
  "identifier": "RV-MP-GG-B07-RH01",
  "sensorType": "http://vocab.getty.edu/aat/300379649",
  "locatedIn": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "operationalStatus": "active",
  "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
  "position": {
    "type": "Point",
    "coordinates": [7.6273, 45.1344, 2.5]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-point/context.jsonld",
  "type": "MonitoringPoint",
  "name": "Galleria Grande Bay 7 \u2014 Relative Humidity Sensor RH01",
  "description": "Capacitive relative humidity sensor mounted at 2.5 m height on the north wall of bay 7, Galleria Grande. Primary MHS hotspot for microclimate monitoring in the painted vault zone.",
  "identifier": "RV-MP-GG-B07-RH01",
  "sensorType": "http://vocab.getty.edu/aat/300379649",
  "locatedIn": "https://example.org/heritalise/space/galleria-grande-bay-7",
  "operationalStatus": "active",
  "responsibleOrganisation": "Consorzio delle Residenze Reali Sabaude",
  "position": {
    "type": "Point",
    "coordinates": [
      7.6273,
      45.1344,
      2.5
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

[] a crm:E22_Man-Made_Object ;
    crm:P1_is_identified_by "RV-MP-GG-B07-RH01" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300379649> ;
    crm:P44_has_condition "active" ;
    crm:P50_has_current_keeper "Consorzio delle Residenze Reali Sabaude" ;
    crm:P53_has_former_or_current_location <https://example.org/heritalise/space/galleria-grande-bay-7> ;
    geojson:geometry "{\"coordinates\":[7.6273,45.1344,2.5],\"type\":\"Point\"}"^^rdf:JSON .


```


### Temperature sensor, Villa Portelli Grand Salon (MT, REQ-009)
A resistance temperature detector in the Grand Salon of Villa Portelli, linked to the architectural-space record for that room. Demonstrates cross-pilot reuse of the same block with different sensorType and locatedIn values.
#### json
```json
{
  "type": "MonitoringPoint",
  "name": "Villa Portelli Grand Salon — Temperature Sensor T01",
  "description": "Resistance temperature detector installed near the main entrance of the Grand Salon to monitor visitor-induced thermal loads.",
  "identifier": "MT-MP-VP-SALON-T01",
  "sensorType": "http://vocab.getty.edu/aat/300379652",
  "locatedIn": "https://example.org/heritalise/space/villa-portelli-salon",
  "operationalStatus": "active",
  "responsibleOrganisation": "Heritage Malta",
  "position": {
    "type": "Point",
    "coordinates": [14.5134, 35.8964, 1.8]
  }
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-point/context.jsonld",
  "type": "MonitoringPoint",
  "name": "Villa Portelli Grand Salon \u2014 Temperature Sensor T01",
  "description": "Resistance temperature detector installed near the main entrance of the Grand Salon to monitor visitor-induced thermal loads.",
  "identifier": "MT-MP-VP-SALON-T01",
  "sensorType": "http://vocab.getty.edu/aat/300379652",
  "locatedIn": "https://example.org/heritalise/space/villa-portelli-salon",
  "operationalStatus": "active",
  "responsibleOrganisation": "Heritage Malta",
  "position": {
    "type": "Point",
    "coordinates": [
      14.5134,
      35.8964,
      1.8
    ]
  }
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix geojson: <https://purl.org/geojson/vocab#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

[] a crm:E22_Man-Made_Object ;
    crm:P1_is_identified_by "MT-MP-VP-SALON-T01" ;
    crm:P2_has_type <http://vocab.getty.edu/aat/300379652> ;
    crm:P44_has_condition "active" ;
    crm:P50_has_current_keeper "Heritage Malta" ;
    crm:P53_has_former_or_current_location <https://example.org/heritalise/space/villa-portelli-salon> ;
    geojson:geometry "{\"coordinates\":[14.5134,35.8964,1.8],\"type\":\"Point\"}"^^rdf:JSON .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Monitoring Point
description: 'A physical sensor or monitoring device installed at a heritage resource.
  Profiles

  ogc.sosa.properties.sensor and adds a CIDOC-CRM E22 Man-Made Object identity layer

  (local inventory identifier, sensor category, heritage location link) and a mandatory

  GeoJSON point for the sensor''s physical position.

  '
allOf:
- $ref: https://opengeospatial.github.io/ogcapi-sosa/build/annotated/sosa/properties/sensor/schema.yaml
- type: object
  properties:
    type:
      const: MonitoringPoint
      description: Fixed type token. Maps to crm:E22_Man-Made_Object in the JSON-LD
        context, enabling SHACL shapes to target monitoring points by class.
      x-jsonld-id: '@type'
    identifier:
      type: string
      description: Local inventory code for this sensor or hotspot within the pilot
        system (crm:P1_is_identified_by). Distinct from the STA @iot.id.
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by
    sensorType:
      type: string
      format: uri
      description: 'Category of sensor or monitoring equipment as a Getty AAT URI
        (crm:P2_has_type). Examples: temperature sensor, relative-humidity sensor,
        crack monitor.'
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
      x-jsonld-type: '@id'
    locatedIn:
      type: string
      format: uri
      description: URI of the heritage resource (architectural-space, heritage-object,
        or place) where this monitoring point is installed (crm:P53_has_former_or_current_location).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P53_has_former_or_current_location
      x-jsonld-type: '@id'
    operationalStatus:
      type: string
      description: Current operational state, e.g. "active", "decommissioned", "under
        maintenance" (crm:P44_has_condition).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
    responsibleOrganisation:
      type: string
      description: Name or URI of the body responsible for operating this monitoring
        point (crm:P50_has_current_keeper).
      x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P50_has_current_keeper
    position:
      type: object
      description: GeoJSON Point giving the sensor's physical position. Use 3D coordinates
        [longitude, latitude, elevation_m] where known.
      properties:
        type:
          type: string
          enum:
          - Point
          x-jsonld-id: '@type'
        coordinates:
          type: array
          minItems: 2
          maxItems: 3
          items:
            type: number
      required:
      - type
      - coordinates
      x-jsonld-id: https://purl.org/geojson/vocab#geometry
      x-jsonld-type: '@json'
  required:
  - type
  - identifier
  - sensorType
  - locatedIn
  - position
x-jsonld-extra-terms:
  MonitoringPoint: http://www.cidoc-crm.org/cidoc-crm/E22_Man-Made_Object
x-jsonld-prefixes:
  crm: http://www.cidoc-crm.org/cidoc-crm/
  geojson: https://purl.org/geojson/vocab#
  sosa: http://www.w3.org/ns/sosa/

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-point/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-point/schema.yaml)


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
    "hasFeatureOfInterest": {
      "@id": "sosa:hasFeatureOfInterest",
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
    "madeBySensor": {
      "@id": "sosa:madeBySensor",
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
    "observedProperty": {
      "@id": "sosa:observedProperty",
      "@type": "@id"
    },
    "observes": {
      "@id": "sosa:observes",
      "@type": "@id"
    },
    "phenomenonTime": {
      "@id": "sosa:phenomenonTime",
      "@type": "@id"
    },
    "resultTime": "sosa:resultTime",
    "usedProcedure": {
      "@id": "sosa:usedProcedure",
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
    "type": "@type",
    "identifier": "crm:P1_is_identified_by",
    "sensorType": {
      "@id": "crm:P2_has_type",
      "@type": "@id"
    },
    "locatedIn": {
      "@id": "crm:P53_has_former_or_current_location",
      "@type": "@id"
    },
    "operationalStatus": "crm:P44_has_condition",
    "responsibleOrganisation": "crm:P50_has_current_keeper",
    "position": {
      "@id": "geojson:geometry",
      "@type": "@json"
    },
    "MonitoringPoint": "crm:E22_Man-Made_Object",
    "sosa": "http://www.w3.org/ns/sosa/",
    "ssn-system": "ssn:systems/",
    "ssn": "http://www.w3.org/ns/ssn/",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "geojson": "https://purl.org/geojson/vocab#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-point/context.jsonld)

## Sources

* [W3C/OGC SOSA Sensor](https://www.w3.org/TR/vocab-ssn/#SOSASensor)
* [CIDOC-CRM E22 Man-Made Object](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E22)
* [HERITALISE D8.2 REQ-009](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/monitoring-point`

