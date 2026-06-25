## Fabrication Output

A `FabricationOutput` is a [`digital-representation`](../digital-representation) constrained to
physical-fabrication formats — STL or 3MF — covering D8.2 profile F (physical replicas and
tactile reproductions for accessibility).

Unlike the other digital-representation profiles, `wasDerivedFrom` is **mandatory**: a
fabrication output must record the PROV derivation chain back to the source
[`three-d-model`](../three-d-model) it was generated from, so the replica stays traceable to its
digital original. This is enforced both in the JSON Schema (presence) and in SHACL (the value
must actually uplift to at least one IRI).
