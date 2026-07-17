# Cultural Heritage Building Blocks

Reusable OGC Building Blocks for modelling cultural heritage data, covering heritage objects,
digital representations, provenance, sensor observations and dataset metadata.


This register defines reusable, machine-readable specification components for the cultural
heritage domain, building on CIDOC-CRM, PROV-O, SOSA/SSN, EDM and DCAT. It originated from the
standards profiling work of the [HERITALISE](https://heritalise.eu) project (deliverables D8.1
and D8.2), but is intended as a general-purpose register usable beyond that project.


## Building Blocks

### `ogc.heritage.aggregation` — Aggregation

**Type:** schema

An EDM/ORE aggregation wrapping a heritage object together with its digital representations and the provider/rights metadata required to publish it to Europeana / ECCCH, modelled as edm:Aggregation (ore:Aggregation).

### `ogc.heritage.heritage-object` — Heritage Object

**Type:** schema

A physical cultural heritage item — an artwork, building element, garden feature or museum object — modelled as a CIDOC-CRM E22 Man-Made Object.

### `ogc.heritage.historical-statement` — Historical Statement

**Type:** schema

A piece of historical knowledge extracted from a document or legacy record — an attribution, identification, date assignment or annotation — modelled as a CIDOC-CRM E13 Attribute Assignment with an inner E33 Linguistic Object and a W3C Web Annotation evidence locator pointing to the source passage.

### `ogc.heritage.monitoring-threshold` — Monitoring Threshold

**Type:** schema

A conservation threshold rule bounding an environmental parameter at a heritage monitoring point or zone, modelled as a SOSA ObservableProperty with QUDT unit and severity annotations (CRRS-011).

### `ogc.heritage.source-carrier` — Source Carrier

**Type:** schema

A physical original document, photograph, drawing, negative or bound volume that carries heritage information, modelled as CIDOC-CRM E31 Document. Records the physical carrier from which digital surrogates are derived, with repository, archival code, and rights metadata.

### `ogc.heritage.monitoring-point` — Monitoring Point

**Type:** schema

A physical sensor or monitoring device installed at a heritage resource, modelled as a SOSA Sensor with a CIDOC-CRM E22 Man-Made Object identity layer. Profiles ogc.sosa.properties.sensor.

### `ogc.heritage.observation` — Observation

**Type:** schema

A scientific observation or measurement of a heritage object or place — environmental monitoring, condition assessment, conservation science — profiling the SOSA/SSN Observation. `hasFeatureOfInterest` ties the reading back to the heritage object or place being measured (CIDOC-CRM/CRMsci's scientific observation context); `madeBySensor` is expected to reference an OGC SensorThings API `Sensor`.

### `ogc.heritage.place` — Place

**Type:** schema

A spatial location relevant to cultural heritage — a site, building, room or landscape feature — modelled as a CIDOC-CRM E53 Place and encoded as an OGC JSON-FG Feature.

### `ogc.heritage.event` — Event

**Type:** schema

An event in the history of a heritage object — production, restoration, modification, acquisition — modelled as a CIDOC-CRM E5 Event (generalising E12 Production and E11 Modification) and profiling the PROV-O Activity.

### `ogc.heritage.heritage-site` — Heritage Site

**Type:** schema

A designated heritage site or pilot area, typed as CIDOC-CRM E27 Site. Extends 'place' with site-level designation, responsible organisation and rights fields.

### `ogc.heritage.historical-place` — Historical Place

**Type:** schema

A named historical spatial feature — room, area, route or landscape designation — that has since been renamed, restructured or disappeared, mapped to its current spatial equivalent with a confidence rating (CIDOC-CRM E53 Place + P89, CRRS-015).

### `ogc.heritage.route` — Route

**Type:** schema

A road, visitor path, internal track or circulation route within or between heritage zones, typed as CIDOC-CRM E26 Physical Feature. Profiles 'place', inheriting JSON-FG geometry and core CRM place attributes, and adding route type, surface, width, accessibility and permitted-use fields.

### `ogc.heritage.actor` — Actor

**Type:** schema

A person or organization associated with cultural heritage objects, events or activities — creators, custodians, restorers — modelled as a CIDOC-CRM E39 Actor and profiling the PROV-O Agent.

### `ogc.heritage.digital-representation` — Digital Representation

**Type:** schema

A digital asset (image, 3D model, document...) representing or documenting a heritage object, modelled as a CIDOC-CRM E73 Information Object and profiling the PROV-O Entity.

### `ogc.heritage.archival-document` — Archival Document

**Type:** schema

A digital-representation profile constrained to archival document formats (PDF/A, LIDO/XML), with a mandatory persistent identifier for long-term archival reference.

### `ogc.heritage.derived-survey-product` — Derived Survey Product

**Type:** schema

A processed output derived from a geometric survey dataset — section drawing, triangulated mesh, deviation map or registration report — modelled as a CRMdig D9 Data Object. Profiles ogc.heritage.digital-representation, adding a mandatory PROV derivation link to the parent survey dataset and a structured CRMdig D10 Software Execution sub-object recording the processing parameters.

### `ogc.heritage.digital-surrogate` — Digital Surrogate

**Type:** schema

A digital file produced by digitising a physical source carrier (e.g. a photograph, drawing or bound volume), modelled as a CRMdig D1 Digital Object with a mandatory PROV derivation chain linking to the source-carrier and a D7 digitisation event sub-object. Profiles ogc.heritage.digital-representation.

### `ogc.heritage.fabrication-output` — Fabrication Output

**Type:** schema

A digital-representation profile constrained to physical-fabrication formats (STL, 3MF), with a mandatory PROV derivation chain back to its source 3D model.

### `ogc.heritage.oral-history` — Oral History

**Type:** schema

An audio or video oral history recording or community narrative, modelled as a CIDOC-CRM E73 Information Object with a mandatory IIIF Presentation API manifest, interviewee reference, language, and consent status. Profiles ogc.heritage.digital-representation and adds schema.org AudioObject/VideoObject alignment for web visibility.

### `ogc.heritage.three-d-model` — 3D Model

**Type:** schema

A digital-representation profile constrained to real-time 3D model formats (glTF, 3D Tiles, OBJ), for 3D documentation and web rendering of heritage objects.

### `ogc.heritage.condition-assessment` — Condition Assessment

**Type:** schema

A qualitative conservation inspection event (CRMsci E14 Condition Assessment) recording the condition state of a heritage object or place at a point in time, with assessor, method, severity and optional spatial localisation.

### `ogc.heritage.digital-representation-feature` — Digital Representation Feature

**Type:** schema

Feature-envelope wrapper for Digital Representation: a spatially located digital asset encoded as a GeoJSON/JSON-FG Feature, or as a topo-feature when geometry is defined by reference/topology.

### `ogc.heritage.heritage-object-feature` — Heritage Object Feature

**Type:** schema

Feature-envelope wrapper for Heritage Object: a spatially located CIDOC-CRM E22 Man-Made Object encoded as a GeoJSON/JSON-FG Feature, or as a topo-feature when geometry is defined by reference/topology.

### `ogc.heritage.surface` — Surface

**Type:** schema

A material finish layer or physical surface feature recognised on an architectural component or heritage object (CIDOC-CRM E25 Man-Made Feature), documenting material, application technique, historical phase and spatial footprint (CRRS-005).

### `ogc.heritage.survey-dataset` — Survey Dataset

**Type:** schema

A geometric survey dataset (point cloud, photogrammetric model or similar) acquired from a heritage asset, modelled as a CRMdig D1 Digital Object with a CRMdig D7 acquisition event and a spatial coverage geometry. Profiles ogc.heritage.digital-representation, adding survey acquisition paradata (method, equipment, CRS, density, accuracy) and a mandatory coverage polygon.

### `ogc.heritage.architectural-space` — Architectural Space

**Type:** schema

An interior room, bay, hall, zone or level within a building, modelled as CIDOC-CRM E22 Man-Made Object. Profile of heritage-object adding containment hierarchy, historical name crosswalk, floor/bay identifiers and optional IFC space reference.

### `ogc.heritage.building` — Building

**Type:** schema

A principal building or architectural unit as a CIDOC-CRM E22 Man-Made Object with spatial footprint and optional IFC/BIM reference. Profile of heritage-object.

