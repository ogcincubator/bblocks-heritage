# Source Carrier

A physical original document, photograph, drawing, negative or bound volume that carries heritage
information, modelled as **CIDOC-CRM E31 Document**. This block records the *physical carrier* —
the original artefact held in an archive, library or collection — as distinct from any digital
copy derived from it.

The intended workflow is:
1. A `source-carrier` record describes the physical original.
2. An [`ogc.heritage.digital-surrogate`](../digital-surrogate/) record describes the digital copy
   and includes a mandatory `prov:wasDerivedFrom` link back to this source carrier.
3. An [`ogc.heritage.historical-statement`](../historical-statement/) can reference this carrier
   as its `sourceRecord` when extracting historical knowledge.

## CRM anchor

| Class | Role |
|-------|------|
| `crm:E31_Document` | The physical carrier as both a physical object and an information-bearing entity |

The JSON `type: "SourceCarrier"` token maps to `crm:E31_Document` via the JSON-LD context.

## Properties

| Property | JSON key | CRM / DCT mapping | Required |
|----------|----------|--------------------|----------|
| Type token | `type` | `@type` → `crm:E31_Document` | yes |
| Carrier type | `carrierType` | `crm:P2_has_type` (Getty AAT URI) | yes |
| Title | `title` | `crm:P102_has_title` | yes |
| Creator(s) | `creator` | `dct:creator` | no |
| Date / period | `datePeriod` | `crm:P4_has_time-span` | no |
| Language | `language` | `crm:P72_has_language` (BCP 47) | no |
| Repository | `repository` | `crm:P50_has_current_keeper` | no |
| Archival code | `archivalCode` | `crm:P1_is_identified_by` | no |
| Collection / series | `collection` | `dct:isPartOf` | no |
| Physical format | `physicalFormat` | `crm:P3_has_note` | no |
| Condition | `condition` | `crm:P44_has_condition` | no |
| Rights | `rights` | `dct:rights` | no |
| Documents object | `documentsObject` | `crm:P70_documents` | no |

## Design notes

- **E31 Document as dual nature** — In CIDOC-CRM, E31 Document is a subclass of both E22 Man-Made
  Object (it has physical existence) and E73 Information Object (it carries propositional content).
  This block exploits both: the physical attributes (format, condition, repository) and the
  documentary function (`P70_documents` the heritage subject).
- **`carrierType` as Getty AAT URI** — Use Getty AAT to distinguish carrier subtypes rather than a
  free text `subtype` field. Example URIs: manuscript (300265483), photograph (300046300),
  architectural drawing (300034787), photographic negative (300127173), bound volume (300028051).
- **`repository` as label or URI** — When the holding institution has a stable URI (ISNI, VIAF,
  Wikidata), prefer that. Literals are accepted for institutions without one.
- **`documentsObject` is optional** — The link from carrier to its heritage subject is provided
  where known, but archives often lack this attribution in their finding aids. The reverse direction
  (heritage object documented by this carrier) can also be expressed via the heritage object's own
  `prov:wasInfluencedBy` or through the `historical-statement` block.
- **Digital surrogates link back** — The forward link from source carrier to digital surrogate is
  not modelled here (to avoid circular dependency in the authoring order). The `digital-surrogate`
  block carries a mandatory `prov:wasDerivedFrom` pointing to the source carrier URI.

## Use cases

- **Venaria REQ-012** — Physical originals (drawings, plans, manuscripts) held in the Archivio di
  Stato di Torino and other repositories, from which digital surrogates were produced.
- **Malta MT-05** — Historical photographs of Villa Portelli (glass plates, prints) held in the
  Malta National Archives or private collections.
- **Malta MT-06** — Books, ledgers and archival documents from the villa's estate records.
