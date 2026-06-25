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
