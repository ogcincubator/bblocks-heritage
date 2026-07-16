# Historical Place

A **Historical Place** records a named spatial feature — room designation, area code, route name
or landscape element — that existed at some point in the past and has since been renamed,
restructured or disappeared. The record bridges archival nomenclature to the contemporary
spatial inventory by linking each historical designation to its best current equivalent and
recording the confidence of that crosswalk.

## Semantic anchor

The block profiles **`ogc.heritage.place`**, inheriting JSON-FG Feature geometry and core
CIDOC-CRM E53 Place attributes (`identifier`, `name`, `placeType`, `partOf`, `sameAs`).
Added properties express the temporal scope of the historical designation, its archival
source, its crosswalk to a current space or place, and the confidence of that mapping.

## Property table

| Property | Required | CRM mapping | Description |
|---|---|---|---|
| *(inherited)* `identifier` | yes | `crm:P1_is_identified_by` | Inventory or gazetteer identifier |
| *(inherited)* `name` | yes | `crm:P87_is_identified_by` | Historical place name or room code |
| *(inherited)* `placeType` | no | `crm:P2_has_type` (@id) | Getty AAT URI for the type of feature |
| *(inherited)* `partOf` | no | `crm:P89_falls_within` (@id) | Broader spatial container (spatial hierarchy) |
| *(inherited)* `sameAs` | no | `owl:sameAs` (@id) | Authority record URI (Getty TGN, Wikidata) |
| `datePeriod` | no* | `crm:P4_has_time-span` | Period or date range when this name was in use |
| `historicalSource` | no* | `crm:P70i_is_documented_in` (@id) | Source carrier or surrogate URI |
| `currentEquivalent` | no | `crm:P89_falls_within` (@id) | Contemporary space or place URI |
| `mappingConfidence` | yes | `crm:P3_has_note` | `confirmed`, `probable`, `approximate`, `uncertain` |

\* `datePeriod` or `historicalSource` (or both) should be provided to support archival crosswalk traceability.

## Design notes

- `currentEquivalent` maps to `crm:P89_falls_within`, the same predicate as the inherited
  `partOf`. Use `currentEquivalent` for historical crosswalk relationships; use `partOf` for
  contemporary spatial containment. Both produce the same RDF predicate — the distinction is
  in the authoring intent.
- `mappingConfidence` maps to `crm:P3_has_note` with a literal value. Enum values follow a
  scale from archivally confirmed identity to speculative association.
- SHACL targets `sh:targetSubjectsOf crm:P4_has_time-span` to avoid a class-based target
  (JSON-FG Feature produces `geojson:Feature`, not `crm:E53_Place`, after uplift). The shape
  only checks IRI validity on optional links; required fields are validated by JSON Schema.

## Standard alignments

- **CIDOC-CRM** (● mandatory): E53 Place (via `place` profile); P89 `falls_within` for both
  spatial containment and historical-to-current mapping; P4 time-span; P70i documented in.
- **JSON-FG** (● mandatory): inherited geometry encoding as a JSON-FG Feature.
- **PROV-O** (● mandatory): inherited from `place` via `digital-representation` lineage where
  a source carrier is cited.
- **JSON-LD** (● mandatory): context maps all properties to CRM URIs.

## Use cases

- Reggia di Venaria (CRRS-015): historical room names from 18th-century inventories mapped
  to current gallery numbers, with source references to archival plans held in the Archivio
  di Stato di Torino.
- Villa Portelli Malta: historical garden features (pergola, terrace names) from pre-war
  survey plans crosswalked to current heritage-site zone records.
