# Roadmap

This roadmap matches the public description of WordSV. Dates assume
work in the period **1 June 2027 to 30 November 2027**, with an optional
second stage from **1 December 2027 to 31 March 2028**. If that funded
period does not take place, the same sequence can be followed
independently; the dates would then shift.

Status of this repository: **planning documents only**. No application
code yet. The rebuild itself is future work.

## First phase

### June 2027 — Foundations

- Publish a public draft of the proof format and the threat model
- Publish a privacy concept (hash only on the chain, never document
  content; optional salt; no accounts; no usage tracking)
- Start the shared TypeScript core library and test vectors

**Milestone:** proof-format draft and threat model are public; core
library in progress.

### July 2027 — Independent verifier

- Finish the core library (hash, create proof, verify proof, Merkle path)
- Command-line verifier
- Browser verifier that runs fully offline: no upload, no account, no
  Kallubi server. Block headers from a source the user chooses, or from
  the user's own node

**Milestone:** the browser verifier works fully offline. A stranger can
check a file and a proof without any Kallubi service.

### August 2027 — Word add-in

- New Microsoft Word add-in (Office.js)
- Computes the hash, writes a proof block into the footer without
  changing the hashed document, and can verify a proof
- The add-in holds **no keys**. Payment for the anchoring transaction
  comes from a wallet of the user's choice; the unsigned request and
  signed result are exchanged as a file or QR code. An optional Kallubi
  bundling helper, if offered, stays separate from verification
- Installable without an app store, via a manifest file

**Milestone:** a usable Word add-in; a document anchored in Word can be
checked with the independent verifier.

### September 2027 — LibreOffice and accessibility

- LibreOffice extension (`.oxt`) using the same core
- Accessibility review against WCAG 2.2 AA (keyboard, screen reader)
- User-facing text in plain language, German and English

**Milestone:** LibreOffice extension published; same result in Word and
LibreOffice; browser verifier checked against WCAG 2.2 AA.

### October 2027 — User tests and security review

- User tests with **at least 10 people** from the intended audience
  (associations, trades, small offices, private individuals)
- Internal security review against the threat model
- Reproducible, signed builds and checksums
- Practise the vulnerability-reporting path

**Milestone:** at least 10 user tests evaluated; open security report
(without exploitable detail); reproducible builds.

### November 2027 — Release, documentation, demo

- Fixes from tests and review
- Documentation in German and English
- **Proof format v1.0 published**
- Public demo

**Milestone:** proof format v1.0 is published; public demo of the first
phase.

## Second stage (December 2027 – March 2028)

Focus: use, clarity, contributors, and a way for the project to continue
— not large new features. Technical work stays secondary (about one
fifth of the time).

- December–January: a second round of user tests with at least 10
  further people; improve the interface and guides
- January–February: explainer videos, examples, project site, a path
  for contributors, talks or workshops
- February–March: conversations with possible partner organisations;
  a sustainability model (see below); only technical fixes that follow
  from feedback
- March: closing workshop, hand-over documentation

### Sustainability

**Verification stays free of charge and does not require an account**,
even if the maintainer or Kallubi disappears (open format, independent
verifier, public chain). Long-term support may come from donations,
grants, supporting organisations, and optional convenience features
that never restrict free verification.
