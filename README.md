# Reactive Research Registry

> Resolvable, versioned research-object graph for Reactive Research.

The Reactive Research Registry provides a machine-readable index
of research objects and the typed relationships among them.

It supports resolution, provenance, graph snapshots,
impact analysis, and distributed research coordination.

The registry is an implementation of the model
defined by `reactive-research-spec`.

## Purpose

Research objects frequently originate in one place and
are implemented, tested, cited, evaluated,
or relied upon somewhere else.

The registry makes those relationships resolvable.
For example:

```text
repository A
    |
    | DEFINES
    v
SE-210.Definition.4.3
    ^
    | IMPLEMENTS
    |
repository B
```

If `SE-210.Definition.4.3` changes,
the graph can identify repository B as potentially affected
without requiring a researcher to remember the relationship.

## Registry Responsibilities

The registry should support:

- globally resolvable research-object identifiers;
- authoritative definition locations;
- typed relationships among research objects;
- repository provenance;
- immutable source revisions;
- content or declaration hashes where appropriate;
- duplicate-definition detection;
- unresolved-reference detection;
- graph snapshots;
- downstream impact queries; and
- federation with other Reactive Research registries.

## Registry Records

A definition record should contain
sufficient provenance to reproduce its identity and origin.

A minimal record may include:

```text
identifier
repository
source path
source kind
commit SHA
schema version
extractor version
content or declaration hash
```

Relationships should retain their normalized semantic type.

For example:

```text
DEFINES
IMPLEMENTS
DEPENDS
INFORMS
EVIDENCES
```

Source annotations use the Reactive Research namespace,
for example:

```text
RR.DEFINES: SE-210.Definition.4.3
RR.IMPLEMENTS: SE-210.Theorem.5.2
```

The namespace identifies a source annotation
as a Reactive Research declaration.
The registry stores the normalized relationship type
rather than the annotation syntax.

Relationship-specific metadata may record
additional information required by the specification.

## First-Class Research Objects

The registry does not reduce all relationships
to repository-to-repository edges.
Research objects remain first-class graph nodes.

This permits questions such as:

- Which repositories implement this definition?
- Which theorem depends on this proposition?
- Which experiments rely on this dataset?
- Which analyses rely on this legal holding?
- Which decisions cite this evidence?
- Which claims have no implementation or evaluation?
- Which implementations reference an identifier that no longer resolves?

Repository graphs can be derived as projections
of the richer research-object graph.

## Publication Model

Participating repositories publish their own normalized declarations,
which may be extracted from source-level `RR.*` annotations or other
compatible authoritative declaration surfaces.

The **shared registry** aggregates and resolves them.

A repository should not need to clone
every other participating repository
to validate its own references.

Conceptually:

```text
participating repository
        |
        | publish declarations
        v
registry builder
        |
        | resolve + validate
        v
versioned graph snapshot
        |
        +--> local validation
        +--> impact analysis
        +--> visualization
        +--> research coordination
```

The aggregate registry should have a
clearly defined write authority.

Participating repositories publish declarations;
they do not independently mutate the aggregate graph.

## Graph Snapshots

Registry builds should produce
immutable or content-addressable snapshots.

A snapshot should identify:

- all participating objects;
- their source revisions;
- all resolved relationships;
- unresolved relationships;
- validation diagnostics;
- schema versions; and
- a stable snapshot identifier or hash.

Research artifacts can then record the graph snapshot
against which they were validated.

## Change Impact

When an indexed object changes,
the registry should support impact traversal
over selected relationship types.

Example:

```text
CHANGE
SE-210.Definition.4.3

DIRECT IMPACT
- formalization A
- verifier B

TRANSITIVE IMPACT
- experiment C
- published claim D
```

Impact means that the affected object's status relative
to the changed upstream object may require reconsideration.

## Federation

The registry supports the multi-repo
Structural Explainability research program.

The Reactive Research specification must not assume
that a single registry will permanently
own all research identifiers.

Future registries should be able to
exchange or resolve records
across organizations and disciplines.

A researcher should be able to define an object
in one organization and have it
implemented, tested, or used by researchers
elsewhere without joining one administrative platform.

## Applications

The graph model is intentionally domain-independent.

Potential graph contents include:

- scientific claims and experiments;
- statutes, cases, holdings, and legal analyses;
- policies and decisions;
- engineering requirements and verification evidence;
- datasets and derived results;
- formal definitions and proofs;
- assurance claims and evidence;
- research software and computational artifacts.

## Relationship to Existing Structural Explainability Infrastructure

The initial registry may consume declarations generated from:

- `SE_MANIFEST.toml`;
- theory-reference artifacts;
- formal-contract artifacts;
- source-level `RR.DEFINES` and `RR.IMPLEMENTS` annotations; and
- other compatible research-object declarations.

Existing tools remain authoritative for the surfaces they own.

The Reactive Research Registry
aggregates those declarations without redefining them.

## Goals

The implementation supports repositories
in the Structural Explainability ecosystem and
demonstrates:

- cross-repository identifier resolution;
- graph construction;
- change-impact traversal; and
- reproducible graph snapshots.

## License

See [LICENSE](./LICENSE).
