
# Monitoring Threshold (Schema)

`ogc.heritage.monitoring-threshold` *v0.1*

A conservation threshold rule bounding an environmental parameter at a heritage monitoring point or zone, modelled as a SOSA ObservableProperty with QUDT unit and severity annotations (CRRS-011).

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Monitoring Threshold

A **Monitoring Threshold** records a conservation threshold rule: a bounded acceptable value range
for a specific environmental parameter at a heritage monitoring point or zone. Threshold records are
linked to `monitoring-point` or `heritage-site` instances and drive conservation alerts when sensor
readings breach the defined limits.

## Semantic anchor

The block is modelled as a **SOSA ObservableProperty** (`sosa:ObservableProperty`), representing the
parameterised environmental quantity together with its acceptable bounds and severity classification.
QUDT unit URIs express the unit of measurement; CIDOC-CRM predicates record type classification,
severity and spatial linkage.

## Property table

| Property | Required | CRM / standard mapping | Description |
|---|---|---|---|
| `id` | no | `@id` | Persistent URI for the threshold rule |
| `type` | yes | `@type` → `sosa:ObservableProperty` | Fixed token `MonitoringThreshold` |
| `parameter` | yes | `sosa:observes` (@id) | URI of the observable parameter (GEOSS ECV or QUDT QuantityKind) |
| `thresholdType` | yes | `crm:P2_has_type` | Constraint kind: `minValue`, `maxValue`, `range`, `rateOfChange` |
| `minValue` | cond. | *(unmapped — JSON payload)* | Lower bound; required when `thresholdType` is `minValue` or `range` |
| `maxValue` | cond. | *(unmapped — JSON payload)* | Upper bound; required when `thresholdType` is `maxValue` or `range` |
| `rateValue` | cond. | *(unmapped — JSON payload)* | Rate limit; required when `thresholdType` is `rateOfChange` |
| `unit` | no | `qudt:unit` (@id) | QUDT unit URI (e.g. `qudt:unit/DEG_C`, `qudt:unit/PERCENT`) |
| `severity` | yes | `crm:P44_has_condition` | Alert level: `advisory`, `warning`, `alarm`, `critical` |
| `appliesTo` | yes | `crm:P53_has_former_or_current_location` (@id) | URI of the monitoring point or heritage zone |
| `activePeriod` | no | `crm:P4_has_time-span` (@json) | `{ "start": "YYYY-MM-DD", "end": "YYYY-MM-DD" }` |
| `source` | yes | `dct:source` | Standard, protocol or expert opinion URI or label |
| `rationale` | no | `crm:P3_has_note` | Free-text justification for the threshold value |
| `reviewDate` | no | `dct:modified` (`xsd:date`) | Scheduled review date (ISO 8601) |

## Design notes

- `minValue`, `maxValue` and `rateValue` are left unmapped in the JSON-LD context: they carry
  numeric payload that has no suitable CRM predicate and are validated by JSON Schema alone.
- `thresholdType` uses `crm:P2_has_type` with a literal value; this is a pragmatic simplification
  (CRM strictly expects an `E55 Type` object) that keeps the JSON flat.
- `sh:targetClass sosa:ObservableProperty` is safe: no other block in the register instantiates
  that class.
- `activePeriod` is mapped as `@type: "@json"` to keep the start/end pair opaque in RDF.

## Standard alignments

- **SOSA/SSN** (● mandatory): `sosa:ObservableProperty` as the semantic anchor; `sosa:observes`
  links the threshold to the measured parameter.
- **QUDT** (● mandatory): `qudt:unit` for the physical unit; parameter URIs from QUDT QuantityKind.
- **GEOSS Essential Climate Variables**: preferred URI set for the `parameter` field.
- **CIDOC-CRM**: P2 type classification, P44 condition/severity, P53 spatial linkage, P4 time-span,
  P3 note/rationale.
- **DCTerms**: `dct:source` for protocol provenance; `dct:modified` for review date.
- **schema.org ActionAccessSpecification** (○ optional): can be used as an extended rule-semantics
  overlay alongside this block where rule-engine integration is required.

## Use cases

- Reggia di Venaria (CRRS-011): define RH and temperature thresholds per gallery zone, linked to
  `monitoring-point` instances installed in the Galleria Grande.
- Villa Portelli Malta: define temperature and light-level thresholds for gallery rooms, with
  seasonal `activePeriod` for winter/summer profiles.

## Examples

### Relative-humidity range threshold, Galleria Grande Bay 7 — Venaria (CRRS-011)
A bilateral RH threshold (45–65 %) for the sensor at bay 7 of the Galleria Grande, Reggia di Venaria. Derived from EN 15757 and adapted to the site's acclimatisation history. A warning-level alert is triggered if the reading falls outside the range. Covers CRRS-011 (monitoring threshold / conservation alert).
#### json
```json
{
  "id": "https://heritalise.eu/venaria/threshold/rh-galleria-grande-bay7",
  "type": "MonitoringThreshold",
  "parameter": "https://qudt.org/vocab/quantitykind/RelativeHumidity",
  "thresholdType": "range",
  "minValue": 45,
  "maxValue": 65,
  "unit": "https://qudt.org/vocab/unit/PERCENT",
  "severity": "warning",
  "appliesTo": "https://heritalise.eu/venaria/sensor/S-GG-07",
  "activePeriod": {
    "start": "2024-01-01",
    "end": "2024-12-31"
  },
  "source": "EN 15757:2010 — Conservation of cultural property. Specifications for temperature and relative humidity to limit climate-induced mechanical damage in organic hygroscopic materials.",
  "rationale": "The Galleria Grande contains organic materials (painted canvas, gilded wood) sensitive to moisture fluctuations. EN 15757 recommends a 50 ± 10 % RH band. The 45–65 % range reflects site-specific acclimatisation history.",
  "reviewDate": "2025-06-30"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-threshold/context.jsonld",
  "id": "https://heritalise.eu/venaria/threshold/rh-galleria-grande-bay7",
  "type": "MonitoringThreshold",
  "parameter": "https://qudt.org/vocab/quantitykind/RelativeHumidity",
  "thresholdType": "range",
  "minValue": 45,
  "maxValue": 65,
  "unit": "https://qudt.org/vocab/unit/PERCENT",
  "severity": "warning",
  "appliesTo": "https://heritalise.eu/venaria/sensor/S-GG-07",
  "activePeriod": {
    "start": "2024-01-01",
    "end": "2024-12-31"
  },
  "source": "EN 15757:2010 \u2014 Conservation of cultural property. Specifications for temperature and relative humidity to limit climate-induced mechanical damage in organic hygroscopic materials.",
  "rationale": "The Galleria Grande contains organic materials (painted canvas, gilded wood) sensitive to moisture fluctuations. EN 15757 recommends a 50 \u00b1 10 % RH band. The 45\u201365 % range reflects site-specific acclimatisation history.",
  "reviewDate": "2025-06-30"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix qudt: <http://qudt.org/schema/qudt/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix sosa: <http://www.w3.org/ns/sosa/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise.eu/venaria/threshold/rh-galleria-grande-bay7> a sosa:ObservableProperty ;
    dct:modified "2025-06-30"^^xsd:date ;
    dct:source "EN 15757:2010 — Conservation of cultural property. Specifications for temperature and relative humidity to limit climate-induced mechanical damage in organic hygroscopic materials." ;
    qudt:unit <https://qudt.org/vocab/unit/PERCENT> ;
    crm:P2_has_type "range" ;
    crm:P3_has_note "The Galleria Grande contains organic materials (painted canvas, gilded wood) sensitive to moisture fluctuations. EN 15757 recommends a 50 ± 10 % RH band. The 45–65 % range reflects site-specific acclimatisation history." ;
    crm:P44_has_condition "warning" ;
    crm:P4_has_time-span "{\"end\":\"2024-12-31\",\"start\":\"2024-01-01\"}"^^rdf:JSON ;
    crm:P53_has_former_or_current_location <https://heritalise.eu/venaria/sensor/S-GG-07> ;
    sosa:observes <https://qudt.org/vocab/quantitykind/RelativeHumidity> .


```


### Minimum temperature threshold, Grand Salon — Villa Portelli, Malta
A minimum-temperature threshold (10 °C) for the Grand Salon at Villa Portelli. An alarm-level alert triggers immediate HVAC intervention if the temperature drops below the condensation-risk boundary. A minimal example — no activePeriod or rationale — showing only the required fields.
#### json
```json
{
  "id": "https://heritalise.eu/malta/threshold/temp-grand-salon-min",
  "type": "MonitoringThreshold",
  "parameter": "https://qudt.org/vocab/quantitykind/Temperature",
  "thresholdType": "minValue",
  "minValue": 10,
  "unit": "https://qudt.org/vocab/unit/DEG_C",
  "severity": "alarm",
  "appliesTo": "https://heritalise.eu/malta/sensor/S-VP-GS-01",
  "source": "https://www.iccrom.org/sites/default/files/2017-12/guidelines_preventive_conservation.pdf",
  "rationale": "Temperatures below 10 °C risk condensation on cold wall surfaces in the Grand Salon, which could damage the painted plaster. An alarm threshold triggers immediate HVAC intervention.",
  "reviewDate": "2025-09-01"
}

```

#### jsonld
```jsonld
{
  "@context": "https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-threshold/context.jsonld",
  "id": "https://heritalise.eu/malta/threshold/temp-grand-salon-min",
  "type": "MonitoringThreshold",
  "parameter": "https://qudt.org/vocab/quantitykind/Temperature",
  "thresholdType": "minValue",
  "minValue": 10,
  "unit": "https://qudt.org/vocab/unit/DEG_C",
  "severity": "alarm",
  "appliesTo": "https://heritalise.eu/malta/sensor/S-VP-GS-01",
  "source": "https://www.iccrom.org/sites/default/files/2017-12/guidelines_preventive_conservation.pdf",
  "rationale": "Temperatures below 10 \u00b0C risk condensation on cold wall surfaces in the Grand Salon, which could damage the painted plaster. An alarm threshold triggers immediate HVAC intervention.",
  "reviewDate": "2025-09-01"
}
```

#### ttl
```ttl
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix qudt: <http://qudt.org/schema/qudt/> .
@prefix sosa: <http://www.w3.org/ns/sosa/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<https://heritalise.eu/malta/threshold/temp-grand-salon-min> a sosa:ObservableProperty ;
    dct:modified "2025-09-01"^^xsd:date ;
    dct:source "https://www.iccrom.org/sites/default/files/2017-12/guidelines_preventive_conservation.pdf" ;
    qudt:unit <https://qudt.org/vocab/unit/DEG_C> ;
    crm:P2_has_type "minValue" ;
    crm:P3_has_note "Temperatures below 10 °C risk condensation on cold wall surfaces in the Grand Salon, which could damage the painted plaster. An alarm threshold triggers immediate HVAC intervention." ;
    crm:P44_has_condition "alarm" ;
    crm:P53_has_former_or_current_location <https://heritalise.eu/malta/sensor/S-VP-GS-01> ;
    sosa:observes <https://qudt.org/vocab/quantitykind/Temperature> .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Monitoring Threshold
description: A conservation threshold rule defining the acceptable value range for
  an environmental parameter at a heritage monitoring point or zone (SOSA ObservableProperty
  + QUDT units).
type: object
required:
- id
- type
- parameter
- thresholdType
- severity
- appliesTo
- source
properties:
  id:
    type: string
    format: uri
    description: Persistent URI identifying this threshold rule.
    x-jsonld-id: '@id'
  type:
    const: MonitoringThreshold
    description: Fixed type token identifying this record as a monitoring threshold
      (maps to sosa:ObservableProperty).
    x-jsonld-id: '@type'
  parameter:
    type: string
    format: uri
    description: URI of the observable environmental parameter, preferably a GEOSS
      Essential Climate Variable (ECV) URI or a QUDT QuantityKind URI (e.g. https://qudt.org/vocab/quantitykind/Temperature
      for temperature).
    x-jsonld-id: http://www.w3.org/ns/sosa/observes
    x-jsonld-type: '@id'
  thresholdType:
    type: string
    enum:
    - minValue
    - maxValue
    - range
    - rateOfChange
    description: 'The kind of threshold constraint: minValue (lower bound only), maxValue
      (upper bound only), range (both bounds), rateOfChange (maximum acceptable rate
      of change per unit time).'
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P2_has_type
  minValue:
    type: number
    description: Lower bound of the acceptable range (required when thresholdType
      is minValue or range).
  maxValue:
    type: number
    description: Upper bound of the acceptable range (required when thresholdType
      is maxValue or range).
  rateValue:
    type: number
    description: Maximum acceptable rate of change per unit time (required when thresholdType
      is rateOfChange).
  unit:
    type: string
    format: uri
    description: QUDT unit URI for the threshold value(s), e.g. https://qudt.org/vocab/unit/DEG_C
      for degrees Celsius or https://qudt.org/vocab/unit/PERCENT for relative humidity.
    x-jsonld-id: http://qudt.org/schema/qudt/unit
    x-jsonld-type: '@id'
  severity:
    type: string
    enum:
    - advisory
    - warning
    - alarm
    - critical
    description: 'Alert severity level triggered when the threshold is breached: advisory
      (informational), warning (attention required), alarm (action required), critical
      (emergency response required).'
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P44_has_condition
  appliesTo:
    type: string
    format: uri
    description: URI of the monitoring point (ogc.heritage.monitoring-point) or heritage
      zone (ogc.heritage.heritage-site) to which this threshold applies.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P53_has_former_or_current_location
    x-jsonld-type: '@id'
  activePeriod:
    type: object
    description: Optional date range during which this threshold is in force (e.g.
      seasonal).
    properties:
      start:
        type: string
        format: date
        description: Start date of the active period (ISO 8601).
      end:
        type: string
        format: date
        description: End date of the active period (ISO 8601).
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P4_has_time-span
    x-jsonld-type: '@json'
  source:
    type: string
    description: URI or label of the technical standard, conservation protocol or
      expert opinion from which the threshold value is derived (e.g. EN 15757 climate
      specification).
    x-jsonld-id: http://purl.org/dc/terms/source
  rationale:
    type: string
    description: Free-text explanation of the basis for the threshold value.
    x-jsonld-id: http://www.cidoc-crm.org/cidoc-crm/P3_has_note
  reviewDate:
    type: string
    format: date
    description: Date on which this threshold is scheduled for review (ISO 8601).
    x-jsonld-id: http://purl.org/dc/terms/modified
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#date
x-jsonld-extra-terms:
  MonitoringThreshold: http://www.w3.org/ns/sosa/ObservableProperty
x-jsonld-prefixes:
  sosa: http://www.w3.org/ns/sosa/
  crm: http://www.cidoc-crm.org/cidoc-crm/
  qudt: http://qudt.org/schema/qudt/
  dct: http://purl.org/dc/terms/
  xsd: http://www.w3.org/2001/XMLSchema#

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-threshold/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-threshold/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "MonitoringThreshold": "sosa:ObservableProperty",
    "id": "@id",
    "type": "@type",
    "parameter": {
      "@id": "sosa:observes",
      "@type": "@id"
    },
    "thresholdType": "crm:P2_has_type",
    "unit": {
      "@id": "qudt:unit",
      "@type": "@id"
    },
    "severity": "crm:P44_has_condition",
    "appliesTo": {
      "@id": "crm:P53_has_former_or_current_location",
      "@type": "@id"
    },
    "activePeriod": {
      "@id": "crm:P4_has_time-span",
      "@type": "@json"
    },
    "source": "dct:source",
    "rationale": "crm:P3_has_note",
    "reviewDate": {
      "@id": "dct:modified",
      "@type": "xsd:date"
    },
    "sosa": "http://www.w3.org/ns/sosa/",
    "crm": "http://www.cidoc-crm.org/cidoc-crm/",
    "qudt": "http://qudt.org/schema/qudt/",
    "dct": "http://purl.org/dc/terms/",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/bblocks-heritage/build/annotated/heritage/monitoring-threshold/context.jsonld)

## Sources

* [W3C/OGC SOSA ObservableProperty](https://www.w3.org/TR/vocab-ssn/#SOSAObservableProperty)
* [QUDT Quantities, Units, Dimensions and Types](https://qudt.org/)
* [CIDOC-CRM E55 Type](https://cidoc-crm.org/html/cidoc_crm_v7.1.3.html#E55)
* [HERITALISE D8.2 CRRS-011](https://heritalise-eccch.eu/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/bblocks-heritage](https://github.com/ogcincubator/bblocks-heritage)
* Path: `_sources/monitoring-threshold`

