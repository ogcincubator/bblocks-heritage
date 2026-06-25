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
