# Derived Survey Product

A processed output produced by applying computational methods to a raw geometric survey dataset
(point cloud, photogrammetric model, etc.). Typical products include triangulated meshes, section
drawings, deviation maps, orthophotos and registration reports.

Modelled as a **CRMdig D9 Data Object** (a derived digital object). Profiles
`ogc.heritage.digital-representation`, adding:

- a mandatory **PROV derivation link** (`prov:wasDerivedFrom`) to the source
  `ogc.heritage.survey-dataset` record
- a structured **CRMdig D10 Software Execution** sub-object (`processingEvent`) capturing the
  processing method, software, operator, parameters and quality result

The spatial extent of the derived product is inherited from the parent survey dataset; no
separate coverage geometry is required.

## CRM anchor

| Term | URI |
|------|-----|
| D9 Data Object | `crmdig:D9_Data_Object` |
| D10 Software Execution | `crmdig:D10_Software_Execution` |
| L11i was_output_of | `crmdig:L11i_was_output_of` |

## Properties

| Property | Mapping | Required | Notes |
|----------|---------|----------|-------|
| `type` | `@type` → `crmdig:D9_Data_Object` | ✔ | Fixed const `"DerivedSurveyProduct"` |
| `title` | `dct:title` | ✔ | Human-readable label for the product |
| `isAbout` | `crm:P129_is_about` | ✔ | URI of the heritage asset (inherited from `digital-representation`) |
| `parentDataset` | `prov:wasDerivedFrom` | ✔ | URI of the source `survey-dataset` record |
| `processingEvent` | `crmdig:L11i_was_output_of` (@json) | ✔ | D10 Software Execution sub-object (see below) |
| `url` | `dcat:accessURL` | — | Download URL for the derived file (inherited) |
| `mediaType` | `dct:format` | — | IANA media type of the output file (inherited) |
| `limitations` | `dct:description` | — | Known artefacts or coverage caveats |
| `reviewStatus` | — | — | Workflow status (draft / reviewed / approved) |

### `processingEvent` sub-object (D10 Software Execution)

Treated as an opaque JSON value at the RDF level (`@type: "@json"`). Fields within the sub-object
are not individually mapped to RDF predicates — they are captured as processing paradata.

| Field | Required | Notes |
|-------|----------|-------|
| `processingMethod` | ✔ | e.g. `"mesh generation"`, `"section extraction"`, `"deviation mapping"` |
| `processingDate` | ✔ | ISO 8601 date |
| `software` | — | Name of the processing software |
| `softwareVersion` | — | Version string |
| `operator` | — | URI of the responsible person/organisation |
| `parameters` | — | Key settings used (free-text) |
| `accuracy` | — | Quality metric, e.g. `"mean deviation ±2 mm"` |
| `qaResult` | — | QA outcome label |

## Design notes

- **Profile choice:** `digital-representation` (not `survey-dataset`) is the correct JSON Schema
  parent because `survey-dataset` requires `acquisitionEvent` and `coverage`, which derived
  products do not have. The semantic relationship to the source dataset is captured via
  `prov:wasDerivedFrom`. CRMdig D9 IS-A D1 Digital Object, consistent with profiling the
  D1-anchored `digital-representation` block.
- **SHACL targeting:** `sh:targetClass crmdig:D9_Data_Object` is safe because no other block in
  this register maps any type token to that class.
- **processingEvent as @json:** Consistent with the `acquisitionEvent` pattern in `survey-dataset`.
  Inner fields are not SHACL-validated; the shape only checks that the sub-object is present.

## Use cases

- CRRS-008 (Reggia di Venaria): mesh, section, deviation map and registration report derived from
  TLS or UAV photogrammetric surveys of palace interiors.
- HM-05 (Villa Portelli, Malta): processed photogrammetric outputs (dense point cloud mesh,
  orthophotos) submitted as deliverables from a recording campaign.
