# Digital Surrogate

A digital file produced by digitising a physical source carrier — a photograph, architectural
drawing, negative, manuscript or bound volume — and made available for research, preservation or
publication. Modelled as a **CRMdig D1 Digital Object** (subtype of CIDOC-CRM E73 Information
Object) with a mandatory PROV derivation link to the originating `source-carrier` record and a
structured **CRMdig D7 Digital Machine Event** sub-object recording the digitisation process.

This block profiles `ogc.heritage.digital-representation` and inherits its `isAbout` link (the
heritage asset the surrogate documents), `mediaType` (IANA format), PROV-O provenance properties,
and persistent identifier field.

## CRM anchor

| JSON property | CRM / vocab predicate | Notes |
|---|---|---|
| `type` | `rdf:type crmdig:D1_Digital_Object` | Fixed const `"DigitalSurrogate"` |
| `wasDerivedFrom` | `prov:wasDerivedFrom` | URI → source-carrier; mandatory |
| `digitisationEvent` | `crmdig:L11i_was_output_of` | D7 event; `@type: @json` opaque object |
| `mediaType` | `dct:format` | Inherited from parent |
| `rights` | `dct:rights` | Licence or access label; mandatory |
| `isAbout` | `crm:P129_is_about` | Inherited; URI of the documented heritage asset |
| `iiifManifest` | `crm:P70i_is_documented_in` | Optional IIIF manifest URI |
| `completeness` | `crm:P3_has_note` | Optional free-text coverage statement |

## Digitisation event sub-object

The `digitisationEvent` object records the CRMdig D7 Digital Machine Event. It is stored as an
opaque JSON blob (`@type: "@json"`) linked via `crmdig:L11i_was_output_of` (the inverse of
`L11 had output`, meaning "this D1 object was the output of this D7 event"). At minimum, `method`
and `date` are required.

| Field | Description |
|---|---|
| `method` (required) | Technique: "flatbed scan", "photogrammetry", "RTI", "digital photography" |
| `date` (required) | ISO 8601 date of the digitisation event |
| `equipment` | Scanner / camera make and model |
| `software` | Capture or processing software |
| `operator` | Person or institution responsible |
| `resolution` | Pixel density or scan resolution |
| `colourMode` | "RGB", "grayscale", "bitonal" |
| `quality` | Derivative tier: "master", "access copy", "thumbnail" |
| `processingHistory` | Free-text log of post-processing steps |

## PROV chain

The full provenance chain for a digitised heritage resource is:

```
source-carrier (E31 Document)
  ← prov:wasDerivedFrom —
    digital-surrogate (D1 Digital Object)
```

For derived products (an OCR transcript, a vector tracing, a colour-corrected reprint), the
chain extends further using additional `prov:wasDerivedFrom` links within those records.

## SHACL targeting

SHACL shapes target nodes via `sh:targetSubjectsOf prov:wasDerivedFrom` rather than
`sh:targetClass crmdig:D1_Digital_Object`. This avoids false positives against
`survey-dataset` instances, which share the same RDF class but lack the derivation link.

## Pilot use cases

| Pilot | Requirement | Typical use |
|---|---|---|
| Reggia di Venaria (CRRS) | CRRS-013 | Digitised archival plans, engravings, photographs from ASTo and Archivio Fotografico |
| Villa Portelli, Malta (HM) | HM-05 | Digitised historical photographs from Heritage Malta collections |
