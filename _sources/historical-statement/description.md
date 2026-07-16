# Historical Statement

A piece of historical knowledge extracted from a document or legacy record — an authorship
attribution, date assignment, iconographic identification, material note, or condition
observation — modelled as a **CIDOC-CRM E13 Attribute Assignment**. The act of extraction
and interpretation is the E13 event; the extracted text itself is an inner **E33 Linguistic
Object** carried in the `statement` sub-object.

A W3C Web Annotation **evidence locator** (`evidenceLocator`) pinpoints the precise passage
within the source record — page number, image region (IIIF fragment), or XML row — using the
OA `oa:hasBody` / `oa:SpecificResource` pattern.

## CRM anchor

| JSON property | CRM / vocab predicate | Notes |
|---|---|---|
| `type` | `rdf:type crm:E13_Attribute_Assignment` | Fixed const `"HistoricalStatement"` |
| `statementType` | `crm:P2_has_type` | Category label or Getty AAT URI |
| `statement` | `crm:P141_assigned` | Inner E33; `@type: @json` opaque object |
| `describedObject` | `crm:P140_assigned_attribute_to` | URI of the heritage entity described |
| `sourceRecord` | `crm:P16_used_specific_object` | URI → source-carrier or digital-surrogate |
| `evidenceLocator` | `oa:hasBody` | W3C OA SpecificResource; `@type: @json` |
| `reviewer` | `prov:wasAttributedTo` | URI of the person/org who extracted the statement |
| `confidence` | `crm:P3_has_note` | Qualitative confidence label |
| `datePeriod` | `crm:P4_has_time-span` | Period the statement pertains to |

## Inner `statement` sub-object (E33 Linguistic Object)

Stored as an opaque JSON value (`@type: "@json"`) linked via `crm:P141_assigned`. Contains:

| Field | Description |
|---|---|
| `text` (required) | The verbatim or summarised statement text |
| `language` | BCP 47 language code, e.g. `"it"`, `"en"`, `"mt"` |
| `datePeriod` | The date or period the text addresses |
| `keywords` | Controlled-vocabulary or free keywords |

## Evidence locator (W3C OA)

The `evidenceLocator` object follows the W3C Web Annotation SpecificResource pattern. The
`source` URI points to the document or digital surrogate containing the evidence; the
`selector` narrows down to the fragment:

```json
{
  "source": "https://…/digital-surrogate/ASTo-VR-lett-1699-dig",
  "selector": {
    "type": "FragmentSelector",
    "conformsTo": "http://www.w3.org/TR/media-frags/",
    "value": "page=3"
  }
}
```

For IIIF-hosted images, use `"type": "FragmentSelector"` with an `xywh=` value conforming to
the IIIF Image API 3 region syntax.

## Relationship to other blocks

- `sourceRecord` must reference a `source-carrier` or `digital-surrogate` URI.
- `describedObject` should reference a `heritage-object`, `place`, `architectural-space`,
  or similar block URI.
- Multiple `historical-statement` records can describe the same heritage entity from different
  sources — they are linked via `describedObject`, not embedded.

## Pilot use case

Primarily addresses **CRRS-014** (Reggia di Venaria): art-historical attributions, dated
inscriptions and material identifications extracted from 17th–18th century archival sources
(letters, inventories, engravings) held in ASTo and the palace archive.
