## 3D Model

A `ThreeDModel` is a [`digital-representation`](../digital-representation) constrained to
real-time 3D model formats — glTF, OBJ, or an OGC 3D Tiles tileset — covering D8.2 profile B
(3D documentation and web rendering of heritage objects, gardens and building elements).

It inherits everything from `digital-representation` (`isAbout`, PROV provenance chain,
persistent identifier) and only adds a constrained `mediaType`.

## HDTO alignment

`hdtoType` (required, inherited from `digital-representation`) is narrowed to a fixed `const` of
`hdto:HC8_3D_Model` (⊑ `hdto:HC5_Digital_Representation`), asserted via context on a plain JSON-LD
parse. `crmdigType` (`crmdig:D9_Data_Object`) is also inherited and required. See
[`digital-representation`](../digital-representation)'s description for the shared HC5-family
rationale. This block also gained its first `shapes.shacl` in this session (it previously had
none) — see the companion `HC83DModelShape` validation there.
