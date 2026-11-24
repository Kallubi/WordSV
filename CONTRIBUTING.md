# Contributing

Thank you for your interest in WordSV.

This repository is currently a **planning scaffold**: a draft of the open
proof format, a threat-model outline, and project documentation. There is
no application code to patch yet. Implementation is planned for a later
phase (see [ROADMAP.md](ROADMAP.md)).

## How to help now

- Read the [proof-format draft](docs/proof-format-v0.1.md) and the
  [threat model](docs/threat-model.md).
- Open an issue (once this repository is on GitHub) for ambiguities,
  missing test vectors, or threats we have not named.
- Security issues: please follow [SECURITY.md](SECURITY.md) and use
  GitHub's private vulnerability reporting, not a public issue.

## Later, when code exists

- The code will be TypeScript, licensed under Apache-2.0.
- Keep changes focused, with tests for parsers and verification.
- Do not add tracking, accounts, or any requirement to use a Kallubi
  server for verification.
- Do not add named third-party block-data providers as hard dependencies;
  verification must work against user-chosen sources or a user's own node.
- All contributions are under Apache-2.0. By opening a pull request you
  confirm that you have the right to submit the work under that license.

## Language and conduct

- Project documents and code comments are in English.
- Be precise and civil. This is a public-interest tool; hostile or
  exclusionary behaviour is not welcome.

Questions about the name and logo are covered in [TRADEMARKS.md](TRADEMARKS.md).
