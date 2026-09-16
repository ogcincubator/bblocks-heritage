## Event

An `Event` is an occurrence in the history of a heritage object — its production, a restoration
or modification campaign, an acquisition — modelled as a CIDOC-CRM `E5_Event` (which generalises
the more specific `E12_Production` and `E11_Modification`).

Like [`actor`](../actor), this block profiles a PROV-O class rather than reinventing event
structure: an `Event` *is* a PROV-O `Activity`. `startedAtTime`/`endedAtTime` carry the
time-span, `wasAssociatedWith` the actor(s) who carried it out, `used`/`generated` the objects
consumed or produced, and `atLocation` where it took place. This block adds only `identifier`
(a local reference, e.g. to a conservation log) and `eventType` (a Getty AAT concept identifying
the kind of event — production, restoration, acquisition...).

As with `place` and `actor`, the uplifted `rdf:type` comes from PROV-O (`prov:Activity`) rather
than `crm:E5_Event` — the SHACL shape here targets by predicate rather than by class.

A `heritage-object`'s production or restoration history is expressed as a set of `Event`s that
reference it via `used` (object worked on) or `generated` (object produced), rather than as an
embedded list on the object itself — so the object record stays stable as new events accumulate.

## HDTO alignment: reused for HC11 Digital Twin Maintenance / HC13 Project

Rather than authoring two more new blocks, this block is reused for HDTO's **HC11 Digital Twin
Maintenance** and **HC13 Project** classes (both reduce to `crm:E5_Event` in the CRM hierarchy —
HC11 via `crm:E65_Creation` ⊑ `E7_Activity` ⊑ `E5_Event`, HC13 via `crm:E7_Activity` ⊑ `E5_Event`;
see `eccch-integration/hdto/03-digital-twin-infrastructure.md`). Unlike the HC3/HC5-8 co-typing
elsewhere in this register, this is **not** derivable from any class an `Event` already asserts —
nothing distinguishes "this is a digital-twin-maintenance activity" from an ordinary
production/restoration event by CRM type alone, so `hdtoType` isn't unconditionally required like
it is on `heritage-object`/`digital-representation`. Instead: set `eventType` to the HDTO class URI
itself (`http://isl.ics.forth.gr/ontology/echoes/HC11_Digital_Twin_Maintenance` or
`.../HC13_Project`) to opt an instance in, and the schema's own `if`/`then` blocks then *require*
`hdtoType` to be set to the matching value — both map via context directly to their respective
predicates (`eventType` → `crm:P2_has_type`, `hdtoType` → `rdf:type`) on a plain JSON-LD parse, no
post-processing step needed. See
[`heritage-digital-twin`](../heritage-digital-twin)/[`heritage-proposition-set`](../heritage-proposition-set)
for how these co-typed `event` instances get referenced (HP19 has composed, HP30 added content).
