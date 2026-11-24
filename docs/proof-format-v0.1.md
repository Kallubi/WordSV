# WordSV proof format — draft v0.1

> **Draft, not a frozen specification.** Field names, encodings and the
> exact hash scope are open points for the specification work described
> in [ROADMAP.md](../ROADMAP.md). A frozen **v1.0** is planned after
> implementation, test vectors and user tests. This document records the
> intended shape so that others can comment early.

## 1. Goal

A WordSV proof should let anyone, without an account and without a
Kallubi service, check two things:

1. This file still has the fingerprint recorded in the proof.
2. That fingerprint was committed in a public-ledger transaction at a
   particular time.

The proof must not contain the document itself. It is not a claim of
authorship, legal ownership, or court-ready evidence (see
[DISCLAIMER.md](../DISCLAIMER.md)).

## 2. Design rules

- **Hash first, proof second.** The bytes that are hashed must be
  defined so strictly that two honest implementations always get the
  same digest for the same document.
- **The proof must not change the hashed document.** If Word or
  LibreOffice writes a proof block into a footer, that block must be
  outside the hash scope, or the hash must be taken over a canonical
  form that excludes it. Getting this wrong would make every stamped
  file fail its own check. This is a central open question of v1.0.
- **Verification has no vendor.** A verifier may fetch block headers
  from a source the user chooses, or from the user's own node. There is
  no required Kallubi endpoint.
- **Be explicit about what was checked.** A result should say whether
  the file matches the hash, whether the Merkle path is consistent, and
  whether a block header was checked — and what was *not* checked.
- **Beta proofs should remain checkable** where possible. The Word-only
  beta is undocumented; compatibility rules will be written once that
  behaviour is specified in the rebuild.

## 3. Hash

- Algorithm: SHA-256.
- Digest encoding in the proof: lowercase hexadecimal, 64 characters.
- **Hash scope (open):** two candidate rules, to be decided with test
  vectors:
  - `raw`: SHA-256 of the exact file bytes on disk.
  - `canonical-excluding-proof`: SHA-256 of a defined canonical form of
    the document that omits the proof footer (needed for in-document
    stamping).
- An **optional salt** is planned so that a third party who already has
  a copy of the file cannot quietly confirm that the copy was anchored
  (a confirmation attack). The exact combination (for example a
  salted commitment instead of a bare digest on the chain) will be
  fixed in v1.0. The salt, if used, lives in the proof, not on the
  chain.

## 4. Proof object (draft JSON)

A proof is a UTF-8 JSON object. A suggested media type is
`application/vnd.wordsv.proof+json`; a suggested file suffix is
`.wordsv.json`. Both are provisional.

```json
{
  "format": "wordsv-proof",
  "version": "0.1",
  "alg": "sha256",
  "hashScope": "raw",
  "hash": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "salt": null,
  "chain": "bsv",
  "txid": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "vout": 0,
  "merklePath": [],
  "blockHash": null,
  "blockHeight": null,
  "claimedTime": null,
  "betaCompatible": false
}
```

| Field | Meaning |
|---|---|
| `format` | Constant identifying this family of proofs. |
| `version` | Format version. Readers must reject versions they do not support, or implement a documented compatibility rule. |
| `alg` | Hash algorithm. v0.1 allows only `sha256`. |
| `hashScope` | Which bytes were hashed. See section 3. |
| `hash` | The SHA-256 digest, hex. |
| `salt` | Optional salt, or `null`. Encoding to be fixed in v1.0. |
| `chain` | Ledger identifier. v0.1 allows only `bsv`. |
| `txid` | Transaction that commits to the (possibly salted) digest. |
| `vout` | Output index of the commitment, if applicable. |
| `merklePath` | Nodes needed to connect the commitment to a block header. Structure to be fixed in v1.0, with test vectors. |
| `blockHash`, `blockHeight` | Optional hints. A verifier must still check them against a chosen header source. |
| `claimedTime` | Optional wall-clock time recorded by the tool. This is **not** the authoritative timestamp. The authoritative time is the time of the committing block. |
| `betaCompatible` | Whether this proof follows a compatibility profile for the undocumented Word beta. Details later. |

The proof **must not** include document content, paths that identify a
person's machine, or private keys.

## 5. Anchoring (outline)

The digest (or the salted commitment) is written into a Bitcoin SV
transaction in a way that a verifier can extract unambiguously. The
exact output script and envelope will be defined in v1.0, together with
how several proofs may be batched in one transaction (Merkle tree).

Payment for that transaction is **outside** the proof format. The Word
and LibreOffice add-ins are designed to hold no keys; a wallet of the
user's choice signs the transaction. Optional bundling by a helper
service does not change the proof and is not required for verification.

## 6. Verification algorithm (outline)

A verifier should:

1. Parse the proof strictly (unknown required fields → fail; trailing
   or malformed data → fail).
2. Hash the file according to `hashScope` and compare it to `hash`.
   Mismatch → fail, and say so clearly.
3. If a salt is present, recompute the on-chain commitment from hash
   and salt.
4. Extract the commitment from the transaction referenced by `txid`
   (the transaction body may be supplied by the user or fetched from a
   source the user chooses).
5. Check the Merkle path against a block header from a source the user
   chooses, or from the user's own node.
6. Report each step separately: file match, commitment match, chain
   check. Never report a single "OK" if only the file-to-proof match
   was checked.

A browser verifier must perform these steps locally. Uploading the
document to a server is not part of the design.

## 7. Test vectors

v0.1 ships no vectors yet. v1.0 will include at least:

- an unchanged document that must verify
- a one-byte change that must fail
- a footer-in-document case that must not change the hash scope
- a salted and an unsalted proof
- a truncated or mutated proof that must fail closed
- a beta-compatibility example, if a profile is defined

## 8. What this draft deliberately does not decide

- Canonicalisation of `.docx` / `.odt` when the proof lives in the file
- The exact on-chain envelope and Merkle-path encoding
- Multi-document batches
- Signature of the proof object itself (not required for the first
  version; integrity comes from the chain commitment)

Comments should go to the issue tracker once this repository is public,
or as a private security report if a comment describes a weakness
(see [SECURITY.md](../SECURITY.md)).
