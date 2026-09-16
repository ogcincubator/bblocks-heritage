## Heritage Proposition Set

`HeritagePropositionSet` is HDTO's **HC16 Heritage Proposition Set** (⊑ `crminf:I4 Proposition
Set` + `HC15 Persistent Digital Object`) — the named-graph content unit a
[`heritage-digital-twin`](../heritage-digital-twin) (HC2) actually *contains*, via HP33 (current
content) / HP34 (superseded content, for versioning).

### Deliberately flat: what's modelled here vs. D7.1's full worked example

D7.1's own worked example (the Deryneia icon, §3 Figures 16-17) shows a real HC16 instance is
**not a flat metadata bag** — it decomposes into a provenance-linked process chain per
digitization/study method: a `D2 Digitization Process` → `D14 Software`/`D6 Digital Device` →
`D9 Data Object` (or `HC7 Digital Audiovisual Object`) output, sometimes continuing into an
`HC17 Observation with Inference` node for interpretive conclusions.

Modelling that full chain is **out of scope for this pass** — it would need its own set of
CRMdig-anchored properties (L1/L23/L12/L20/L10 and others, several of which are still flagged
unconfirmed in `eccch-integration/hdto/09-worked-example-deryneia.md` §7) and is exactly the kind
of speculative authoring this register's own conventions avoid without a concrete pilot
requirement behind it (the same reasoning already applied to deferring HC9/HC10/HC12/HC17-HC20).
This schema instead gives HC16 a flat `content` property (`crm:P148_has_component`) referencing
the register entities the set's assertions are about or derived from — enough to make HC2/HC16
usable end-to-end now. **Extension point:** when a concrete pilot requirement needs the deeper
process chain, model it as a `$ref`/`allOf` composition adding a `process` property (or similar)
alongside `content`, rather than redesigning this block — `heritage-proposition-set`'s own worked
example in `09-worked-example-deryneia.md` §5 is the reference to build that against.

### Property reference

| Property | HDTO property | Notes |
|---|---|---|
| `content` | — (modelled as crm:P148_has_component, a flat simplification — see above) | Required, ≥1. What this proposition set's assertions are about/derived from. |
| `composedBy` | HP30i content was added by (inverse of HP30 added content) | Required. The `event` (co-typed HC11) that produced this set. |
| `replaces` | HP32 replaced | The prior `heritage-proposition-set` this one supersedes, within the same digital twin's version chain. |

### `crmpem:` lineage (inherited from HC15, not asserted directly here)

HC16's own parent, HC15 Persistent Digital Object, is declared by D7.1 as ≡ `crmpem:PE19`. This
block doesn't assert that equivalence directly (HC16 itself has no `crmpem:` equivalence in
D7.1 — only HC15/HC14 do), but see
[`heritage-digital-twin`](../heritage-digital-twin)'s own `ontology.ttl` and description for the
`crmpem:` namespace's provisional status and
[ogcincubator/bblocks-heritage#1](https://github.com/ogcincubator/bblocks-heritage/issues/1).
