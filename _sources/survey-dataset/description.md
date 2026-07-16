# Survey Dataset

A geometric survey dataset — point cloud, photogrammetric model, structured-light scan or similar
product — acquired from a heritage asset. Models the dataset as a **CRMdig D1 Digital Object**
(itself a subclass of CIDOC-CRM E73 Information Object) and captures the acquisition paradata as a
**CRMdig D7 Digital Machine Event** sub-object.

Profiles [`ogc.heritage.digital-representation`](../digital-representation/) and inherits its PROV-O
provenance fields (`wasAttributedTo`, `wasGeneratedBy`, `wasDerivedFrom`) and its `isAbout` link to
the surveyed heritage asset.

## CRM anchor

| Class | Namespace | Role |
|-------|-----------|------|
| `crmdig:D1_Digital_Object` | `http://www.ics.forth.gr/isl/CRMdig/` | The survey dataset itself |
| `crmdig:D7_Digital_Machine_Event` | `http://www.ics.forth.gr/isl/CRMdig/` | The acquisition event (embedded as `acquisitionEvent`) |

The JSON `type: "SurveyDataset"` token maps to `crmdig:D1_Digital_Object` via the JSON-LD context.
The `acquisitionEvent` sub-object maps to `crmdig:L11i_was_output_of` (D1 was output of D7), treated
as an opaque JSON value to avoid deep blank-node expansion.

## Properties

| Property | JSON key | Mapping | Required |
|----------|----------|---------|----------|
| Type token | `type` | `@type` → `crmdig:D1_Digital_Object` | yes |
| Dataset title | `title` | `dct:title` | yes |
| Surveyed asset | `isAbout` | `crm:P129_is_about` (inherited) | yes |
| Acquisition event | `acquisitionEvent` | `crmdig:L11i_was_output_of` | yes |
| — method | `acquisitionEvent.acquisitionMethod` | (opaque JSON) | yes |
| — date | `acquisitionEvent.acquisitionDate` | (opaque JSON) | yes |
| — operator | `acquisitionEvent.operator` | (opaque JSON) | no |
| — equipment | `acquisitionEvent.equipment` | (opaque JSON) | no |
| — CRS | `acquisitionEvent.crs` | (opaque JSON) | no |
| — density | `acquisitionEvent.density` | (opaque JSON) | no |
| — accuracy | `acquisitionEvent.accuracy` | (opaque JSON) | no |
| — processing | `acquisitionEvent.processingHistory` | (opaque JSON) | no |
| Coverage geometry | `coverage` | `geojson:geometry` | yes |
| Media type | `mediaType` | `dct:format` (inherited) | no |
| File location | `url` | `dcat:accessURL` | no |
| Limitations | `limitations` | `dct:description` | no |
| Workflow status | `reviewStatus` | (unmapped) | no |
| Persistent ID | `persistentIdentifier` | `crm:P1_is_identified_by` (inherited) | no |
| PROV attribution | `wasAttributedTo` | `prov:wasAttributedTo` (inherited) | no |
| PROV derivation | `wasDerivedFrom` | `prov:wasDerivedFrom` (inherited) | no |

## Design notes

- **`acquisitionEvent` as opaque JSON** — the D7 sub-object is mapped with `@type: "@json"` to
  prevent coordinate arrays and string fields from being misinterpreted as RDF lists. The
  acquisition paradata is thus preserved in the JSON payload without deep uplift.
- **`isAbout` is required** (inherited from `digital-representation`). It should reference the URI
  of the heritage-site, building, architectural-space or heritage-object that was surveyed.
- **Coverage geometry** uses the `geojson:geometry` predicate (WGS 84 coordinates by default). For
  interior surveys where a 2D WGS 84 polygon is not meaningful, a bounding rectangle or a GeoJSON
  Point at the centroid of the asset is acceptable.
- **Derived products** (processed meshes, deviation maps, registration reports) are modelled as
  `ogc.heritage.derived-survey-product`, which profiles this block and adds a mandatory
  `prov:wasDerivedFrom` link back to the parent survey dataset.

## Use cases

- **Venaria REQ-007** — Point-cloud and photogrammetric survey datasets of buildings and
  decorative systems at Reggia di Venaria Reale.
- **Malta MT-01 / MT-03** — UAV photogrammetric models and 3D scans of Villa Portelli and its
  gardens.
