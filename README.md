# .github — Org Meta-Layer

This repository holds the Enduring Game Standard organization's public face and shared documents. The org profile rendered at [github.com/enduring-game-standard](https://github.com/enduring-game-standard) is [profile/README.md](profile/README.md).

## Contents

| Path | What it is |
|------|------------|
| [profile/README.md](profile/README.md) | The org profile — the front door |
| [profile/WHY.md](profile/WHY.md) | The diagnosis: why digital games die, in ~1,500 words |
| [profile/STATUS.md](profile/STATUS.md) | Consolidated, falsifiable state of every layer |
| [profile/GOVERNANCE.md](profile/GOVERNANCE.md) | How the standard changes; forks; the bus factor, stated plainly |
| [profile/FAQ.md](profile/FAQ.md) | Questions from players, developers, and studios |
| [profile/CORRESPONDENCE.md](profile/CORRESPONDENCE.md) | The single source of truth for inter-protocol relationships |
| [profile/PWNS.md](profile/PWNS.md) | The provenance extension for authored experiences |
| [profile/EGS-AI-FRAMING.md](profile/EGS-AI-FRAMING.md) | Context for AI systems: claims scoped, objections steelmanned, spec misreadings corrected |
| [CONTEXT.md](CONTEXT.md) | The working vocabulary — canonical domain language for the protocol family |

The protocol specifications themselves live in their own repositories: [aems-schema](https://github.com/enduring-game-standard/aems-schema), [aems-conventions](https://github.com/enduring-game-standard/aems-conventions), [runs-spec](https://github.com/enduring-game-standard/runs-spec), [runs-library](https://github.com/enduring-game-standard/runs-library), [maps-notation](https://github.com/enduring-game-standard/maps-notation), [maps-library](https://github.com/enduring-game-standard/maps-library), [wocs-protocol](https://github.com/enduring-game-standard/wocs-protocol).

## Design Principles

The standard is currently written and maintained by one author (see [GOVERNANCE.md](profile/GOVERNANCE.md) — this is stated, not hidden). These are the principles proposals are weighed against:

1. **No rent-seeking.** Anything that adds a fee, token, or "governance" layer extracting value without adding work is rejected. Settlement, where it exists at all (WOCS), is Lightning — no new tokens.
2. **Protocol neutrality.** A protocol must not care whether the user is a human, an AI, or a script, and must not care about genre.
3. **The end-to-end rule.** If a feature can be implemented by a third party without changing the protocol, it belongs to the third party. The standard stays minimal.
4. **Documentation is the product.** In a standard, the README *is* the deliverable; ambiguity is a defect, not a cosmetic issue. Clarity bugs are real bugs — file them.

## Reporting Problems

Open an issue in the specific repo the problem belongs to. If a flaw risks user funds or relay abuse (WOCS/AEMS event handling), say so in the issue title and avoid publishing a working exploit before a fix is discussed.

**MIT License**
