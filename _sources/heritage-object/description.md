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
