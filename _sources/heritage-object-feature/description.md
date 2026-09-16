## Heritage Object Feature

Feature-envelope wrapper for [`heritage-object`](../heritage-object) — see `schema.yaml` for the
GeoJSON/JSON-FG and topology-reference encoding options this block adds.

## HDTO alignment

Required `properties.hdtoType` (fixed `const` `hdto:HC3_Tangible_Heritage_Entity`), inherited from
[`heritage-object`](../heritage-object)'s `$defs/properties` — see that block's description for
the full rationale. Maps via this block's own `context.jsonld` directly to `rdf:type`, so it's
asserted on a plain JSON-LD parse with no post-processing step. The companion validation shape is
inherited from `heritage-object`'s `shapes.shacl` (both blocks emit the same
`crm:E22_Man-Made_Object` class via `choType`), so this block does not duplicate it.
