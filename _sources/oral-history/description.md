# Oral History

An audio or video oral history recording, community narrative, or witness testimony associated
with a heritage site. Modelled as a **CIDOC-CRM E73 Information Object** (the recording as
an information-bearing object) and profiling `ogc.heritage.digital-representation` for its
PROV-O provenance chain, `isAbout` link, and media type field.

This block adds mandatory fields for the **interviewee**, **language**, **consent status**
(required for legal compliance), and an **IIIF Presentation API manifest** enabling playback
and timed annotation in IIIF viewers. It aligns with **schema.org AudioObject/VideoObject**
for web-visible metadata (`schema:interviewee`, `schema:duration`).

## CRM / vocab anchor

| JSON property | Predicate | Notes |
|---|---|---|
| `type` | `rdf:type crm:E73_Information_Object` | Fixed const `"OralHistory"` |
| `title` | `dct:title` | Human-readable recording title |
| `interviewee` | `schema:interviewee` | URI → actor; mandatory |
| `language` | `crm:P72_has_language` | BCP 47 code; mandatory |
| `consentStatus` | `dct:accessRights` | Consent/access label; mandatory |
| `iiifManifest` | `crm:P70i_is_documented_in` | IIIF manifest URI; mandatory |
| `mediaType` | `dct:format` | Inherited; must be audio/* or video/* |
| `rights` | `dct:rights` | Licence; inherited and required here |
| `isAbout` | `crm:P129_is_about` | Inherited; URI of heritage entity; required |
| `transcriptUri` | `crm:P70i_is_documented_in` | Optional transcript URI (same predicate as iiifManifest) |
| `duration` | `schema:duration` | ISO 8601 duration, e.g. "PT1H23M" |
| `contributor` | `dct:contributor` | Interviewer, technician, etc. |
| `theme` | `dct:subject` | Thematic keywords or controlled terms |
| `historicalPeriod` | `crm:P4_has_time-span` | Period addressed in the narrative |

## IIIF integration

The `iiifManifest` URI should point to a IIIF Presentation API 3 manifest. Timed annotations
(interview segments, thematic tags) can be added as IIIF Annotation Pages on the manifest's
Canvas, enabling rich viewer integration without modifying this block's schema.

## Consent handling

`consentStatus` is a required free-text or controlled-vocabulary field. Recommended values:

| Value | Meaning |
|---|---|
| `"consented — public access"` | Interviewee consented to unrestricted publication |
| `"consented — restricted"` | Consented but limited to specific audiences (e.g. researchers) |
| `"anonymised"` | Personal identifiers removed; public access permitted |
| `"pending consent review"` | Under review; restrict access until resolved |

`rights` (inherited from parent) carries the formal licence or copyright statement.

## SHACL targeting

`sh:targetClass crm:E73_Information_Object` is unambiguous: no other block in this register
uses E73 as a type const (the parent `digital-representation` has it in its bblock.json
metadata but no `type: const` in schema, so instances do not emit that RDF triple).

## Pilot use case

**HM-07** (Villa Portelli, Malta): audio and video testimonies from local residents and
historians recording lived memories of the villa, its garden, and surrounding community.
