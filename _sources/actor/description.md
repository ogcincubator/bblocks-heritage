## Actor

An `Actor` is a person or organization associated with cultural heritage objects, events or
activities — a creator, custodian, restorer, donor — modelled as a CIDOC-CRM `E39_Actor`.

Rather than reinventing agent identity, this block profiles
[`ogc.ogc-utils.prov-agent`](https://ogcincubator.github.io/bblock-prov-schema/register/bblock/ogc.ogc-utils.prov-agent):
an `Actor` *is* a PROV-O Agent. `agentType` discriminates `Person` / `Organization` /
`SoftwareAgent` (mapped to PROV-O's corresponding RDF classes), `name` and `id` carry identity,
and `actedOnBehalfOf` expresses delegation (e.g. a conservator acting for a museum). This block
adds two CIDOC-CRM-specific properties: `identifier` (a local authority/catalogue number) and
`sameAs` (a link to an external authority record, typically Getty ULAN).

As with [`place`](../place), profiling an external vocabulary means the uplifted `rdf:type` comes
from PROV-O (`prov:Person`, `prov:Organization`...) rather than `crm:E39_Actor` — the SHACL shape
here targets by predicate (`sh:targetSubjectsOf`) rather than by class.

`prov-agent` requires *exactly one* of `name` or `id` (not both): `id` is for bare references to
an agent described elsewhere, `name` is for an inline, fully-described record like the examples
here. An `Actor` with its own resolvable URI should still set `id` — just not alongside `name` in
the same object.

Other blocks reference an `Actor` by `id`:

- an `event` (production, restoration) links to the `Actor`(s) who carried it out
- a `digital-representation`'s provenance chain can attribute creation/digitisation to an `Actor`
  via the imported PROV blocks
