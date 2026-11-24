# Threat model — outline

> **Status: outline for a planning-stage project.** Not a security audit
> and not a complete STRIDE or LINDDUN study. A fuller model is planned
> alongside the first public specification draft.

## 1. What we try to protect

| Asset | Why it matters |
|---|---|
| Integrity of the document-to-proof match | A changed file must never be reported as matching an old proof. |
| Integrity of the chain commitment | The proof must not be accepted against a transaction or block that does not actually commit to that fingerprint. |
| Privacy of document content | The file never leaves the user's machine during hash, stamp or verify. Only a fingerprint (and never the text) is written to the ledger. |
| Privacy of the fact of anchoring | Without a salt, anyone who already has a copy of the file could check whether that copy was anchored. An optional salt is planned to reduce this confirmation attack. |
| User intent when paying for an anchor | The add-in holds no keys. A wallet of the user's choice signs the transaction; what is shown in that wallet must be what is signed. |

## 2. What we do not claim to protect

- Authorship, legal ownership, or the truth of a document's contents
- Evidential value in a court (that is decided case by case)
- Confidentiality of a file that the user already shares with others
- A user who is coerced, or who stamps the wrong file on purpose
- A compromised operating system, office installation, or firmware
- Availability of any particular third-party header source (users can
  choose another source or their own node)
- Qualified eIDAS timestamps or qualified electronic signatures

## 3. Assumptions

- The user can keep the original file and the proof together. A proof
  without the file, or a file without the proof, cannot be checked.
- At least one trustworthy source of block headers is available to the
  verifier (a source the user chooses, or a node the user runs).
- SHA-256 remains one-way and collision-resistant for this purpose.
- The user treats the office add-in and the verifier as software they
  can inspect; official builds will be reproducible.

## 4. Trust boundaries

```
[User's computer: document stays here]
        |  hash, proof JSON, maybe a payment request / QR
        v
[Wallet of choice] ---- signed transaction ----> [public chain]
        ^
        |  headers, transaction body (user-chosen source or own node)
        v
[Independent verifier: browser, fully offline, or CLI]
```

Kallubi servers, if any, are **optional conveniences** (for example
bundling several anchors). They are outside the trust boundary for
verification. If they disappear, proofs must still verify.

## 5. Threats and planned mitigations

| ID | Threat | Planned mitigation |
|---|---|---|
| T1 | A footer or other in-document proof block changes the hashed bytes, so a stamped file fails its own check — or, worse, an attacker hides data in an unhashed region. | The specification will define hash scope precisely, with test vectors. The proof block must not enter the digest, or a canonical form must exclude it. |
| T2 | Verifier says "OK" for a file that changed. | Strict SHA-256 comparison; fail closed on any mismatch or parse error; tests and fuzzing of the parser. |
| T3 | Verifier says "OK" without actually checking the chain. | Results report each step separately (file match, commitment, header check). A file-only match must be labelled as such. |
| T4 | Confirmation attack: someone who already has a copy of the file checks whether it was anchored. | Optional salt; the chain then carries a commitment, not the bare digest. Limits of this defence will be stated openly. |
| T5 | User is forced to trust a single block-data vendor. | No hard-coded required vendor. Headers come from a source the user chooses, or from the user's own node. |
| T6 | Merkle path or proof JSON is ambiguous or silently truncated. | Strict parser, canonical encodings, test vectors, fail closed. |
| T7 | Add-in or helper steals keys or swaps the payment. | The add-in holds no keys. Payment uses a wallet of the user's choice; exchange via file or QR. The wallet's own review remains the signing surface. |
| T8 | A bundling helper censors, logs documents, or becomes a single point of failure. | The helper never receives the document, only a digest or commitment. Verification does not use the helper. Users can always anchor with their own wallet. |
| T9 | Lost proofs or lost Merkle paths make old stamps uncheckable. | The proof file is the archive; documentation will say that clearly. A proof should contain the path, not a pointer to a vendor archive. |
| T10 | Supply-chain compromise of dependencies or builds. | Few, pinned dependencies; reproducible builds; public vulnerability reporting. |
| T11 | Accessibility or language failures lead users to misread a result. | Plain-language results, German and English, WCAG 2.2 AA, user tests with at least 10 people from the intended audience. |
| T12 | Beta proofs cannot be checked after the rebuild. | Compatibility is a stated goal; a profile will be specified if the beta behaviour can be described precisely. |

## 6. Residual risks (to be stated in the product)

- A timestamp proves that a fingerprint existed by a certain block
  time. It does not prove who created the file or that the contents are
  true.
- Without a salt, the fact of anchoring a known file can be observed
  on a public chain.
- Verification quality depends on the header source the user actually
  uses.
- No software add-in can protect a machine that is already compromised.

This outline will be revised when the specification draft is published
and again after the first security review.
