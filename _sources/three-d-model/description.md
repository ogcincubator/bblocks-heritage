## 3D Model

A `ThreeDModel` is a [`digital-representation`](../digital-representation) constrained to
real-time 3D model formats — glTF, OBJ, or an OGC 3D Tiles tileset — covering D8.2 profile B
(3D documentation and web rendering of heritage objects, gardens and building elements).

It inherits everything from `digital-representation` (`isAbout`, PROV provenance chain,
persistent identifier) and only adds a constrained `mediaType`.
