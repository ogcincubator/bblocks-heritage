## Digital Representation

A `DigitalRepresentation` is a digital asset — an image, a 3D model, a scanned document — that
represents or documents a heritage object, modelled as a CIDOC-CRM `E73_Information_Object`.

Like `actor` and `event`, this block profiles a PROV-O class: a `DigitalRepresentation` *is* a
PROV-O `Entity`, so its provenance chain (`wasAttributedTo`, `wasGeneratedBy`, `wasDerivedFrom`)
comes for free from `ogc.ogc-utils.prov-entity`. This block adds `isAbout` (a required link back
to the [`heritage-object`](../heritage-object) it represents), `mediaType` and an optional
`persistentIdentifier`.

`DigitalRepresentation` is deliberately generic — it covers any digital asset type. The three
format-specific profiles constrain it for a particular kind of asset:

- [`three-d-model`](../three-d-model) — glTF / 3D Tiles / OBJ 3D models (profile B)
- [`archival-document`](../archival-document) — PDF/A and LIDO/XML documents with a mandatory
  persistent identifier (profile C)
- [`fabrication-output`](../fabrication-output) — STL/3MF outputs with a mandatory PROV
  derivation chain back to their source 3D model (profile F)

## HDTO alignment

Every instance requires two co-types, both mapping via `context.jsonld` directly to `rdf:type` —
asserted on a plain JSON-LD parse, no post-processing step needed:

- `crmdigType`, a fixed `const` of `crmdig:D9_Data_Object`. HDTO's own OGC SensorThings crosswalk
  table (D7.1 Table 1, p.28) names `crmdig:D9_Data_Object`, not `crm:E73_Information_Object` (this
  block's `bblock.json` `rdfType` metadata), as HC5's actual CRM anchor — this block's instances
  carried no CIDOC-CRM class triple at all before this (the real uplifted type was only
  `prov:Entity`, inherited from the PROV-O Entity profile), so D9 is now asserted directly rather
  than left as documentation-only metadata.
- `hdtoType`, required with an `enum` of `hdto:HC5_Digital_Representation` /
  `hdto:HC7_Digital_Audiovisual_Object` / `hdto:HC8_3D_Model` (HC7/HC8 ⊑ HC5 per D7.1).
  `oral-history` and `three-d-model` narrow this to their own specific HC7/HC8 `const` via their
  own schema; a plain `digital-representation` or `archival-document` instance defaults to HC5.

**Cross-block D9 collision, resolved:** `derived-survey-product` also profiles this block and
previously targeted `crmdig:D9_Data_Object` exclusively via `sh:targetClass` in its own
`shapes.shacl`, on the assumption that no sibling block asserted D9. Now that every
digital-representation-family instance does, `derived-survey-product`'s shape was switched to
`sh:targetSubjectsOf crmdig:L11i_was_output_of` (a predicate unique to that block) so its
processing-specific constraints don't false-positive on a plain photo or 3D model that also
happens to carry the shared D9 co-type — see its own `shapes.shacl` and the register's established
convention for this exact situation (`feedback_bblocks_authoring` memory).

`archival-document` is co-typed HC5 too (see its own description); `fabrication-output` is
deliberately left unassigned — it produces a *physical* 3D-printed artifact from digital input, so
it may belong in the HC3 tangible family instead of HC5, and that's a decision for whoever owns
that pilot requirement, not OGC alone.
