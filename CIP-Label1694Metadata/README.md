---
CIP: ????
Title: Governance Metadata Reference under Label 1694
Category: Metadata
Status: Proposed
Authors:
    - Pi Lanningham <pi@sundaeswap.finance>
Implementors: []
Discussions:
    - https://github.com/cardano-foundation/CIPs/pulls/????
Created: 2026-04-25
License: CC-BY-4.0
---

## Abstract

This CIP formalises two interchangeable wire forms for governance
metadata documents published under Cardano transaction metadatum
label `1694`: an **inline** form (the full JSON-LD document encoded
directly per CIP-100's existing convention) and a **reference**
form (a small `{uri, hash}` pair pointing at the document hosted
off-chain). It closes a gap left open by [CIP-100][] for
transactions that publish governance metadata without an
associated governance certificate or action through which to
attach an on-chain `Anchor`.

## Motivation: why is this CIP necessary?

CIP-100 reserves metadatum label `1694` for "publishing governance
related metadata on-chain" and prescribes the standard
JSON-to-CBOR conversion as the encoding. The model CIP-100
sketches is:

1. A governance action, vote, DRep registration, or other
   certificate carries an on-chain `Anchor = (URL, hash)` field.
2. The `URL` resolves to a CIP-100-format JSON-LD document, hosted
   externally or inlined under label `1694` of some other (or the
   same) transaction.

This works cleanly when there is a certificate. But Cardano
governance also produces metadata documents that have no natural
certificate to ride on — published commentary on an in-flight
proposal, draft proposals circulated for review, clarifications and
addenda from authors who are not currently casting a vote or
updating their registration. For these, the only available
publication channel is label `1694` directly, with no `Anchor`
field through which to express indirection.

CIP-100 does not formally define what to put under label `1694` in
that case. The implicit expectation is that the document is
inlined. That works, but has two sharp edges that often steer
publishers to off-chain forums instead — the very outcome label
`1694` was reserved to prevent:

- **Cost.** Transaction metadata bytes are paid for in lovelace.
  A long-form draft proposal or a multi-paragraph addendum can
  run several kilobytes; inlining is wasteful when external
  content-addressed hosting (IPFS, Arweave, the publisher's own
  domain with a hash guarantee) is essentially free at this scale.
- **Friction.** Cardano transaction metadata strings are limited
  to 64 bytes per CDDL, requiring long string fields to be
  chunked into arrays. That is doable, but adds preprocessing
  overhead and makes the on-chain form less faithful to the
  canonical JSON-LD source.

This CIP addresses both by formalising a `{uri, hash}` reference
form under label `1694`, alongside the existing inline form. The
reference form is the same `(URL, hash)` shape as the on-chain
`Anchor` already used by certificates — lifted into label-1694
metadata for the case where the publisher has no certificate to
anchor *from*.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in [RFC 2119][].

A transaction MAY publish a CIP-100-conformant governance metadata
document by attaching it to the transaction's metadata under
[CIP-10][] label `1694` in either of two interchangeable wire
forms. Tooling that consumes label-`1694` metadata MUST recognise
both forms.

### Inline form

The full JSON-LD document, encoded directly under label `1694` per
the [standard convention][] used elsewhere in Cardano for
converting JSON to transaction metadata CBOR.

Because Cardano transaction-metadata strings are limited to 64
bytes per CDDL, string fields longer than 64 bytes MUST be encoded
as arrays of ≤64-byte string segments, in source order, per the
standard convention. Tooling that decodes label-`1694` metadata
MUST reassemble such arrays into their original strings before
validating against the referenced JSON-LD context.

The inline form is appropriate when the document is short enough
that the on-chain byte cost is acceptable, or when the publisher
explicitly wants the document body resolvable from the chain alone
without any external dereferencing.

### Reference form

A small object containing a URI and a content hash:

```json
{
  "1694": {
    "uri":  "<resolvable URI for the document>",
    "hash": "<blake2b-256 hex of the document's raw bytes>"
  }
}
```

Both fields are REQUIRED:

- `uri` MUST be a resolvable URI from which the document can be
  fetched (HTTPS, IPFS, Arweave, etc.). Content-addressable
  schemes are RECOMMENDED for durability, in keeping with CIP-100's
  general guidance for context and metadata hosting.
- `hash` MUST be the blake2b-256 hash of the raw bytes of the
  document at `uri`, encoded as a 64-character lowercase hex
  string. This matches the hashing convention used by
  `cardano-cli hash anchor-data --file-binary` and by the on-chain
  `Anchor` struct attached to certificates and governance actions.

The reference form MAY additionally include `@context` and `@type`
fields naming the JSON-LD context and the document class. Tooling
SHOULD include them when the document class is known: it allows
indexers to identify and route the document without first
dereferencing `uri`. Tooling MUST NOT require these optional
fields to recognise a reference as valid.

The reference form is appropriate when the document is long, when
external content-addressed hosting is already in use, or when the
same document is referenced from multiple transactions and
inlining all of them would be wasteful.

### Verification

Tooling that resolves a reference-form entry MUST:

1. Fetch the bytes at `uri`.
2. Compute their blake2b-256 hash.
3. Compare with the on-chain `hash`. On mismatch, treat the
   reference as invalid and surface the mismatch to the user; the
   referenced document MUST NOT be considered the authoritative
   document the on-chain reference claimed to point at.

This is the same verification pattern as for any CIP-100 anchor
hash check.

### Coexistence with CIP-100 anchors

This CIP does not replace, modify, or constrain the existing
CIP-100 / CIP-1694 `Anchor` mechanism on certificates and
governance actions. Where a certificate is being submitted, its
on-chain `Anchor` field remains the canonical way to reference
metadata. The wire forms defined here apply specifically to
metadata published *under label `1694`*, whether or not any
certificate in the same transaction also carries an `Anchor`.

A transaction MAY use both: a certificate-level `Anchor` pointing
at off-chain metadata, *and* a label-`1694` metadata entry in
either form on the same transaction. Tooling MUST treat the two as
independent surfaces — the `Anchor` is consumed by the ledger's
governance subsystem; the label-`1694` entry is consumed by
metadata indexers.

## Rationale

### Why a separate CIP rather than amending CIP-100?

CIP-100 anticipates evolution through extension CIPs rather than
in-place revision. The label-`1694` wire format is a small,
focused, infrastructural concern that is independently useful to
any metadata-publishing CIP — it does not belong inside the spec
of any one extension that happens to need it first. Factoring it
out lets future extensions reference it directly.

### Why mirror the on-chain Anchor field shape?

Wallets, indexers, and CLI tooling already know how to parse the
on-chain `Anchor = (URL, hash)` structure from certificate fields.
Using the same `(URL, hash)` shape — and field names close to the
JSON convention already in use for anchors (`uri`, `hash`) — for
the label-`1694` reference form lets that tooling reuse existing
parse and verification paths instead of growing a parallel one.

### Why not require inline-only?

Forcing inline-only would impose a hard cap on document length
above which the only way to publish is to host externally and skip
the on-chain town square entirely — defeating the entire reason
label `1694` was reserved. Allowing the reference form keeps the
discoverability property (the chain still contains a verifiable
pointer) while removing the byte-cost and chunking friction.

### Why not require reference-only?

The inline form is necessary for publishers who lack durable
external hosting, or who explicitly want the document body
resolvable from chain history alone. CIP-100's reservation of
label `1694` was motivated precisely by the case where "external
hosting may be unavailable to some users". Removing inline support
would leave those users without an on-chain option.

### Why hash raw bytes rather than canonicalised RDF?

The `cardano-cli hash anchor-data --file-binary` command, and most
deployed wallet tooling, hash the raw bytes of the metadata file
as received. Strict CIP-100 §Hashing prescribes RDF-canonicalised
hashing, but practice has settled on raw-byte hashing for
operational reasons: it is parser-agnostic, deterministic across
implementations, and trivially reproducible from any HTTP `GET` of
the document. This CIP codifies what is already deployed.
Implementations that wish to additionally produce or verify the
canonicalised-RDF hash MAY do so out of band, but the hash
referenced under label `1694` is the raw-bytes hash.

## Path to Active

### Acceptance Criteria

- At least two client libraries parse and verify both forms
  transparently.
- At least one wallet or governance-explorer tool surfaces
  reference-form entries with the same UI affordances as inline
  ones, including hash verification on dereference and a clear
  warning on mismatch.
- At least one published governance-metadata document exists in
  each form on mainnet, with verifiable hashes and resolvable
  URIs, suitable as a test vector.

### Implementation Plan

- Reference parsers in Rust and TypeScript covering both forms,
  alongside the CIP-100 reference implementations.
- Coordination with `cardano-cli` to document the chunking
  expectations for inline form and the recommended pattern for
  reference form (a one-shot helper for producing
  `{ "1694": { "uri": ..., "hash": ... } }` from a file path and
  URI is the obvious ergonomics win).
- A small published test vector — one inline document, one
  reference document — committed alongside this CIP for parser
  conformance testing.

## Copyright

This CIP is licensed under [CC-BY-4.0][].

[CIP-100]: ../CIP-0100/README.md
[CIP-10]: ../CIP-0010/README.md
[RFC 2119]: https://www.rfc-editor.org/rfc/rfc2119
[standard convention]: https://developers.cardano.org/docs/get-started/cardano-serialization-lib/transaction-metadata/#json-conversion
[CC-BY-4.0]: https://creativecommons.org/licenses/by/4.0/legalcode
