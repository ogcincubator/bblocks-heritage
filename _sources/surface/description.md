# Surface

A **Surface** records a material finish layer or physical surface feature recognised on an
architectural component or heritage object — plaster, stucco, fresco, stone, gilding or any
other applied or inherent surface finish. The block documents the host object, material and
technique using Getty AAT controlled terms, the historical phase of application and a mandatory
polygon footprint delineating the spatial extent of the surface.

## Semantic anchor

The block is modelled as **CIDOC-CRM E25 Man-Made Feature** (`crm:E25_Man-Made_Feature`), a
subclass of E26 Physical Feature, representing a surface as a feature *recognised on* a physical
object rather than an independently manufactured entity. The mandatory link `crm:P56i_is_found_on`
connects the surface to its host heritage object or space.

## Property table

| Property | Required | CRM mapping | Description |
|---|---|---|---|
| `id` | no | `@id` | Persistent URI for this surface record |
| `type` | yes | `@type` → `crm:E25_Man-Made_Feature` | Fixed token `Surface` |
| `hostObject` | yes | `crm:P56i_is_found_on` (@id) | URI of the host heritage-object, architectural-space or building |
| `material` | yes | `crm:P45_consists_of` (@id) | Getty AAT URI for the primary surface material |
| `technique` | no | `crm:P33_used_specific_technique` (@id) | Getty AAT URI for the application technique |
| `historicalPhase` | no | `crm:P4_has_time-span` | Date or period label of application/last significant alteration |
| `exposure` | no | `crm:P3_has_note` | Positional context: "vault intrados", "north wall", "floor", etc. |
| `conditionSummary` | no | `crm:P44_has_condition` | Brief conservation condition description |
| `conditionAssessment` | no | *(unmapped — JSON payload)* | URI of a linked condition-assessment record |
| `sourceEvidence` | no | `crm:P70i_is_documented_in` (@id) | Source carrier or surrogate URI documenting the surface |
| `reviewStatus` | no | *(unmapped — JSON payload)* | Workflow status: "draft", "reviewed", "approved" |
| `footprint` | yes | `geojson:geometry` (`@type: @json`) | GeoJSON Polygon or MultiPolygon of the surface extent |

## Design notes

- `footprint` follows the embedded-geometry pattern used by `condition-assessment`: a plain
  GeoJSON Geometry sub-object mapped as `@type: "@json"` to prevent coordinate arrays being
  interpreted as RDF lists. The schema restricts geometry type to Polygon/MultiPolygon, since
  a surface always has area extent (not a point or line).
- `conditionAssessment` and `reviewStatus` are left unmapped in context — they carry workflow
  or cross-reference payload with no suitable CRM predicate and are not validated by SHACL.
- `sh:targetClass crm:E25_Man-Made_Feature` is safe: no other block in the register instantiates
  that class.
- `crm:P56i_is_found_on` is the inverse of `P56_bears_feature`; it records that this surface
  feature is found on (borne by) the host object.

## Standard alignments

- **CIDOC-CRM** (● mandatory): E25 Man-Made Feature; P56i found on (host linkage); P45 consists
  of (material); P33 technique; P4 time-span (phase); P3 note (exposure); P44 condition.
- **Getty AAT** (● mandatory): material and technique URIs.
- **GeoJSON / JSON-FG** (● mandatory): footprint geometry via `geojson:geometry`.
- **JSON-LD** (● mandatory): full context mapping.
- **CRMsci E14 Condition Assessment** (○ optional): linked via `conditionAssessment` URI.

## Use cases

- Reggia di Venaria (CRRS-005): Baroque fresco plaster on the vault of the Galleria Grande,
  with a polygon footprint covering an individual bay and links to photogrammetric survey
  products and condition-assessment records.
- Villa Portelli Malta: Polychrome stone floor surface in the Grand Salon, with material
  (limestone) and technique (in-situ cast mosaic) documented with Getty AAT terms.
