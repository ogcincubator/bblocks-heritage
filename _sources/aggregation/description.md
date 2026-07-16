## Aggregation

An `Aggregation` is the EDM/ORE publishing wrapper that Europeana and ECCCH actually ingest:
it pairs a [`heritage-object`](../heritage-object) (the *Cultural Heritage Object*) with the
[`digital-representation`](../digital-representation)(s) that depict it and the provider and
rights metadata required for publication.

CIDOC-CRM and PROV-O do not model this aggregation layer — EDM (Europeana Data Model) and
OAI-ORE do. This block captures that layer without reimplementing the full EDM/ORE ontology,
using just the properties actually mandatory or commonly used in ECCCH submission flows.

The `type` field is fixed to `"Aggregation"`, which the JSON-LD context maps to
`ore:Aggregation`. SHACL shapes therefore use `sh:targetClass ore:Aggregation`.

Properties:

| Property | EDM / ORE mapping | Notes |
|---|---|---|
| `aggregatedCHO` (required) | `edm:aggregatedCHO` (@id) | URI of the `heritage-object` |
| `isShownBy` | `edm:isShownBy` (@id) | URI of the primary `digital-representation` |
| `isShownAt` | `edm:isShownAt` (@id) | URI of the object's landing page at the provider |
| `hasView` | `edm:hasView` (@id, array) | Additional `digital-representation` URIs |
| `object` | `edm:object` (@id) | URI of a thumbnail image |
| `dataProvider` | `edm:dataProvider` | Holding institution name |
| `provider` | `edm:provider` | ECCCH national aggregator name |
| `rights` (required) | `edm:rights` (@id) | Rights URI (rightsstatements.org or Creative Commons) |

### Use cases

- D8.2 profiles B, C, D — EDM aggregation is **mandatory** for Europeana / ECCCH publishing
  in all three digital asset profiles (images, archival documents, 3D models)
