## Heritage Digital Twin

`HeritageDigitalTwin` is HDTO's **HC2 Heritage Digital Twin** (ECCCH's Heritage Digital Twin
Ontology, D7.1 v1.0 §4.1) — the part of HDTO with no prior analogue anywhere in
`bblocks-heritage`. Per D7.1's own worked example (the Deryneia icon, §3), an HC2 node is
lightweight: identified by its propositional content and creating actor, not by a rich internal
structure of its own. Updating a digital twin doesn't erase its previous state — see
`containedPropositionSet` below.

The actual content lives in one or more [`heritage-proposition-set`](../heritage-proposition-set)
(HC16) instances this block points to via `containsPropositionSet`/`containedPropositionSet`.

### Not `aggregation`

This block is deliberately **not** a profile of, or a replacement for, `aggregation` (EDM/ORE
Aggregation): D7.1 gives no crosswalk between EDM/ORE and any HC class anywhere in the document
(checked explicitly). `aggregation` remains the separate Europeana/ECCCH-portal display-record
construct it already was; `heritage-digital-twin` is the KB-facing HDTO construct. If a
relationship between the two turns out to be useful later, that's an additive property to add
once both exist, not a prerequisite.

### How it's built (HC11/HC13)

An HC2 instance is composed/maintained by an **HC11 Digital Twin Maintenance** activity
(`composedBy`, HP19 has composed, inverse direction), typically running under an **HC13 Project**.
Rather than two more new blocks, HC11 and HC13 are modelled by co-typing the existing
[`event`](../event) block — see its own "HDTO alignment" section for how.

### Property reference

| Property | HDTO property | Notes |
|---|---|---|
| `ofHeritageEntity` | HP1i is digital twin of (inverse of HP1 has digital twin) | Required. Points back to the HC1/HC3 entity — in practice, any HC3-co-typed `heritage-object`/`heritage-site`/`building`/`heritage-object-feature` instance in this register. |
| `label` | — | Human-readable label, per D7.1's own worked-example convention (rdfs:label). |
| `creator` | — | The actor who created/maintains this digital twin (prov:wasAttributedTo). |
| `containsPropositionSet` | HP33 contains | Required, ≥1. Currently valid content. |
| `containedPropositionSet` | HP34 contained | Superseded content — the versioning mechanism: an update moves a `heritage-proposition-set` URI here instead of deleting it. |
| `composedBy` | HP19i was composed by (inverse of HP19 has composed) | The `event` (co-typed HC11) that produced/last updated this twin. |

### `crmpem:` namespace — provisional

D7.1 declares HC2 ⊑ HC14 Volatile Digital Object ≡ `crmpem:PE20`, but never states a resolvable
namespace URI for `crmpem:` — not in D7.1 itself, not on the live HDTO ontology page, and not in
the underlying PARTHENOS Entities v3.1 model-description document (the source D7.1 cites).
`ontology.ttl` asserts `hdto:HC2_Heritage_Digital_Twin owl:equivalentClass crmpem:PE20` using
`http://www.ics.forth.gr/isl/CRMext/CRMpe.rdfs/` as a **provisional** namespace URI (the
FORTH-ISL "CRMpe" extension, following the same URI convention this register already uses for
`crmdig:`). This is flagged, not resolved — see
[ogcincubator/bblocks-heritage#1](https://github.com/ogcincubator/bblocks-heritage/issues/1) for
the open question. Do not treat this URI as confirmed.

### Scope note

This block does not attempt to model HC14/HC15 (Volatile/Persistent Digital Object) as their own
schema — per `eccch-integration/hdto/03-digital-twin-infrastructure.md`, these are infrastructure
classes, not content-bearing blocks in their own right. HP28 (has snapshot)/HP29 (has digital
object part), which relate HC14/HC15/crmdig:D1, are likewise out of scope here — revisit only if
a concrete pilot requirement needs versioned digital-object snapshotting modelled explicitly.
