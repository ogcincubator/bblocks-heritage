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

### `ogc.heritage.event` — Event

**Type:** schema

An event in the history of a heritage object — production, restoration, modification, acquisition — modelled as a CIDOC-CRM E5 Event (generalising E12 Production and E11 Modification) and profiling the PROV-O Activity.

### `ogc.heritage.observation` — Observation

**Type:** schema

A scientific observation or measurement of a heritage object or place — environmental monitoring, condition assessment, conservation science — modelled as a CIDOC-CRM E16 Measurement (the entry point CRMsci specialises for scientific observation) and profiling the PROV-O Activity. Bridges sensor data recorded via the OGC SensorThings API back into the CIDOC-CRM graph.

### `ogc.heritage.digital-representation` — Digital Representation

**Type:** schema

A digital asset (image, 3D model, document...) representing or documenting a heritage object, modelled as a CIDOC-CRM E73 Information Object and profiling the PROV-O Entity.

### `ogc.heritage.place` — Place

**Type:** schema

A spatial location relevant to cultural heritage — a site, building, room or landscape feature — modelled as a CIDOC-CRM E53 Place and encoded as an OGC JSON-FG Feature.

### `ogc.heritage.archival-document` — Archival Document

**Type:** schema

A digital-representation profile constrained to archival document formats (PDF/A, LIDO/XML), with a mandatory persistent identifier for long-term archival reference.

### `ogc.heritage.fabrication-output` — Fabrication Output

**Type:** schema

A digital-representation profile constrained to physical-fabrication formats (STL, 3MF), with a mandatory PROV derivation chain back to its source 3D model.

### `ogc.heritage.three-d-model` — 3D Model

**Type:** schema

A digital-representation profile constrained to real-time 3D model formats (glTF, 3D Tiles, OBJ), for 3D documentation and web rendering of heritage objects.

### `ogc.heritage.actor` — Actor

**Type:** schema

A person or organization associated with cultural heritage objects, events or activities — creators, custodians, restorers — modelled as a CIDOC-CRM E39 Actor and profiling the PROV-O Agent.

