# WordSV

**Open, independently verifiable document proofs.** Anyone should be able to
check that a file has not changed since a given point in time — without an
account, without fees, and without having to trust Kallubi or any other
vendor.

> **Status: planning scaffold.** This repository holds a draft of the open
> proof format, a threat-model outline, and project documentation. There is
> **no application code yet.** The rebuild is future work (see
> [ROADMAP.md](ROADMAP.md)).

[Disclaimer](DISCLAIMER.md) · [Proof format draft v0.1](docs/proof-format-v0.1.md) ·
[Threat model](docs/threat-model.md) · [Security policy](SECURITY.md) ·
[Trademarks](TRADEMARKS.md)

## Why this exists

A club may need to show that meeting minutes have not been altered after
they were agreed. A craftsperson may need to show that a quote or invoice
has not been changed after it was sent. Today that usually means paid
timestamping services and vendor accounts, or simply trusting someone.

WordSV is meant to make the same check simple and free: the tool
computes a SHA-256 fingerprint of the file and records that fingerprint,
with a timestamp, in a public ledger. **The document itself never leaves
the computer.** Later, anyone can check the file against the proof with a
free browser page or command-line tool.

## What “independent” means

Proofs must remain verifiable **even if Kallubi disappears**:

- the proof format is open and versioned, with test vectors
- verification runs in a free browser page (fully offline) and on the
  command line, with no Kallubi server in the path
- the fingerprint is recorded on a public chain; block headers can come
  from a source the user chooses, or from the user's own node
- there are no accounts and no usage data

Bitcoin SV is the technical ledger used for the timestamp. Only the
fingerprint is written there, never the document. An optional salt is
planned so that a third party who already has a copy of the file cannot
quietly check whether that copy was anchored.

## What will be built

All new code will be **Apache-2.0**, in TypeScript, around one shared core
library:

1. An open proof-format specification
2. A core library (hash, create proof, verify proof)
3. A browser verifier that runs locally, and a command-line verifier
4. A new Microsoft Word add-in (Office.js), installable without an app store
5. A LibreOffice extension (`.oxt`)
6. A threat model, a security concept, and a privacy concept
7. Accessibility (WCAG 2.2 AA), user tests, and documentation in German
   and English

The add-in will **hold no keys**. Paying for the anchoring transaction
will use a wallet of the user's choice; the request and the signed result
can be exchanged as a file or a QR code. An optional Kallubi bundling
helper, if offered, will stay separate from verification.

Verification will **always be free** and will not require an account.
Optional convenience features must not restrict that.

## What exists today

A Word-only **beta** of WordSV already exists as a private demo. It can
compute a SHA-256 hash, anchor it, and write hash, timestamp and
transaction id into the footer. It has **no** independent verifier, **no**
documented format, and **no** payment function. It will be **rebuilt from
scratch**. Existing proofs from the beta should remain checkable where
possible; the exact compatibility rules are part of the specification work.

The name WordSV is a filed (not registered) word mark held by the
maintainer. The mark protects only the name. The code will be Apache-2.0.
See [TRADEMARKS.md](TRADEMARKS.md).

Related public work by the same maintainer, not part of this codebase:

- Explorer: https://kallubi-bsv-explorer.de
- Desktop wallet: https://github.com/Kallubi/kallubi-bsv-wallet
- Pay Button (live demo): https://kallubi-bsv-explorer.de/?tool=paybutton

## Licence and warranty

Copyright 2026 The Kallubi Authors. Licensed under the
[Apache License 2.0](LICENSE). See [NOTICE](NOTICE) and
[DISCLAIMER.md](DISCLAIMER.md). This is not a qualified eIDAS timestamp
and not a statement about evidential value in court.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md). Security reports go through
GitHub's private vulnerability reporting, as described in
[SECURITY.md](SECURITY.md).
