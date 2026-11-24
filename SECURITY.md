# Security policy

## Status

WordSV is in an **early planning stage**. This repository currently
contains a draft specification and a threat-model outline, not production
software. Nothing here should be read as a claim that an implementation
is complete or has been audited.

## Reporting a vulnerability

Please report security problems **privately** and give the maintainer a
reasonable time to fix them before you publish details.

**Preferred and only contact path:** GitHub's private vulnerability
reporting (the repository's **Security** tab → **Report a vulnerability**).
This will be enabled when the repository is published on GitHub; please
use it as soon as it is available.

Please include what you found, how to reproduce it, and which commit or
document version you reviewed. **Do not send private keys, wallet seeds,
or copies of other people's documents.** A synthetic example is enough.

What to expect: an acknowledgement as soon as the maintainer can, a fix
or a clear statement of the limits, and credit if you want it. There is
no bug bounty at this stage.

## In scope (once code exists)

- Anything that could make verification report success for a changed file
- Ambiguity or bypasses in the proof format or its parsers
- Hash-scope mistakes (for example a footer that changes the hashed bytes)
- Key handling, if any payment-helper code is added (the Word and
  LibreOffice add-ins are designed to hold **no keys**)
- Supply-chain issues in this project's own build and dependencies

## Out of scope

- Findings that require a compromised operating system, root access, or
  physical access to an unlocked device (see `docs/threat-model.md`)
- Vulnerabilities in third-party dependencies that are already public
  (please report those upstream; tell us if we should update)
- Claims about legal evidential value (see [DISCLAIMER.md](DISCLAIMER.md))
- Social engineering of the maintainer
