## Heritage Object

A `HeritageObject` is the physical cultural heritage item itself — a painting, a building
element, a garden feature, a museum artefact — modelled as a CIDOC-CRM `E22_Man-Made_Object`.

It is deliberately minimal: identity (`identifier`, `title`), typing and material via Getty AAT
URIs, current location, and an optional persistent identifier. Everything else attaches to it by
reference:

- digital assets (images, 3D models, documents) point back to it via `isAbout` in
  [`digital-representation`](../digital-representation)
- sensor observations point to it (or a `place` derived from it) as their feature of interest
- provenance, events (production, restoration) and actors are modelled as separate, linked
  entities rather than embedded here, so the same object can accumulate history over time without
  growing an unbounded record.

## HDTO alignment

Every instance of this block is required to carry `hdtoType`, a fixed `const` of
`hdto:HC3_Tangible_Heritage_Entity` (ECCCH's Heritage Digital Twin Ontology, D7.1 v1.0/v1.1). It
maps via `context.jsonld` directly to `rdf:type`, alongside `crm:E22_Man-Made_Object` — additive,
per D7.1 Figure 20's own worked example, which shows the same node typed as both classes at once.
This is asserted on a plain JSON-LD parse with no extra tooling: HC3 is a logical consequence of
the CIDOC-CRM type this block already declares (HC3 ⊑ crm:E18 Physical Thing, and E22 ⊑ E18), and
the value never varies, but it's still declared as a required schema property — not inferred via a
post-processing step — so that any consumer doing a basic uplift (not just one that runs this
register's `semantic-uplift.yaml` steps) reliably gets the HDTO co-type. `building` and
`heritage-object-feature` inherit `hdtoType` from this block's own `$defs/properties`, so they
don't redeclare it. `architectural-space` is deliberately **not** co-typed — whether a room/space
is independently "the" heritage entity or just a component of its containing building is a
partner (CRRS/Venaria) judgment call, not decidable from OGC's side alone.
