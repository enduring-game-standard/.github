# Status

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 🧭 **[Why](./WHY.md)** · ⚖️ **[Governance](./GOVERNANCE.md)** · ❓ **[FAQ](./FAQ.md)**

**Last updated: 2026-07-06.** The state of every layer, one page. Two words recur: a claim is **asserted** when a person or AI judged it correct by reading, **verified** when it was checked mechanically against an oracle. Today every fidelity claim in the ecosystem is asserted; none is verified. The roadmap below names the event that moves each one.

## State by Layer

| Layer | What exists today | What does not exist |
|-------|-------------------|---------------------|
| **RUNS** (spec) | Three specifications — [Record Schema](https://github.com/enduring-game-standard/runs-spec/blob/main/RECORD_SCHEMA.md), [DIGS Expression Language](https://github.com/enduring-game-standard/runs-spec/blob/main/DIGS_EXPRESSION_LANGUAGE.md), [Network Topology](https://github.com/enduring-game-standard/runs-spec/blob/main/NETWORK_TOPOLOGY.md) — written to be implementable in isolation | A reference parser, evaluator, or compiler; a second game; machine compilation to any platform |
| **RUNS Library** | Conceptual examples of the `runs:` palette and architecture patterns — illustrations with no implementation behind them, possibly wrong | Reference implementations; a non-empty commons; anything blessed by ID |
| **AEMS** (schema) | The four event kinds and provenance chain, specified; conventions with worked examples | Pinned Nostr kind numbers (deferred until formalized against real relay usage); published ecosystem artifacts |
| **MAPS** (notation) | The four primitives, composition hierarchy, and a small pattern library, specified | Any real game notated end-to-end; a demonstrated MAPS→RUNS translation (the correspondence is **aspirational**, per the [correspondence map](./CORRESPONDENCE.md)) |
| **WOCS** | The three-event protocol shape, specified | Pinned kind numbers; any live offer/fulfill/ack cycle; any deployed coordination. The most speculative layer, and the only one with a Lightning dependency |
| **Spacewar! conversion** | The 1962 PDP-1 game expressed as RUNS source — 26 DIGS Processors — **asserted** faithful by line-level source tracing; an AI hand-compiled it into a playable PICO-8 cartridge (a hand-compilation demonstrates playability; it verifies nothing). Private repo, not yet published | Publication; mechanical verification of any Processor |

## The Falsifiable Milestone

The current work is the oracle that turns *asserted* into *verified*:

1. **Test vectors from the original platform.** Run the original Spacewar! 3.1 on a PDP-1 emulator and extract input→state vectors from the machine itself — ground truth, not interpretation.
2. **A reference DIGS evaluator.** A program that mechanically parses and evaluates the RUNS source — no human in the loop.
3. **The check.** Evaluate the 26 Processors against the emulator-derived vectors. Bit-exact agreement moves the Spacewar fidelity claim from *asserted* to *verified*; disagreement falsifies it publicly.

When this ships, anyone with the emulator and the source can run the check.

## What Would Count as Adoption

Later milestones, in order of difficulty: a MAPS score of a real, existing game written by someone other than the author; a second RUNS game; a machine-compiled build on a second platform; a published AEMS entity referenced by a manifestation the publisher didn't write; one WOCS cycle settling real work. None of these exist today.

**MIT License**
