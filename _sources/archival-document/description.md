## Archival Document

An `ArchivalDocument` is a [`digital-representation`](../digital-representation) constrained to
archival document formats — PDF/A or LIDO/XML — covering D8.2 profile C (long-term archival and
metadata-exchange records).

Unlike the other digital-representation profiles, `persistentIdentifier` is **mandatory** here:
archival records are expected to be citable and durably resolvable, which is the whole point of
migrating a legacy paper or filesystem archive into this model (UC-V-3).

## HDTO alignment

`hdtoType` (required, inherited from `digital-representation`) is pinned to a fixed `const` of
`hdto:HC5_Digital_Representation` — a digitised archival document has no more specific HC5-family
class in D7.1. `crmdigType` (`crmdig:D9_Data_Object`) is also inherited and required. Both are
asserted via context on a plain JSON-LD parse. See
[`digital-representation`](../digital-representation)'s description for the full HC5 rationale.
