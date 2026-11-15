# Status

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 🧭 **[Why](./WHY.md)** · ⚖️ **[Governance](./GOVERNANCE.md)** · ❓ **[FAQ](./FAQ.md)**

**Last updated: 2026-11-15.** The state of every layer, one page. Two words recur: a claim is **asserted** when a person or AI judged it correct by reading, **verified** when it was checked mechanically against an oracle. The Spacewar! conversion is verified against its oracle: it compiles to the original binary. Every other fidelity claim is asserted.

## State by Layer

| Layer | What exists today | What does not exist |
|-------|-------------------|---------------------|
| **RUNS** (spec) | Five specifications in [runs-spec](https://github.com/enduring-game-standard/runs-spec): DIGS Expression Language, Network Topology, Machine Description, Declarations, and Conversion Process; one compiler, for the PDP-1, in [runs-spacewar](https://github.com/wScottSh/runs-spacewar) | A compiler for any machine other than the PDP-1; a second converted game |
| **RUNS Library** | Conceptual examples of the `runs:` palette and architecture patterns — illustrations with no implementation behind them, possibly wrong | Reference implementations; a non-empty commons; anything blessed by ID |
| **AEMS** (schema) | The four event kinds and their parent references, specified; conventions with worked examples | Pinned Nostr kind numbers (deferred until formalized against real relay usage); conventions for transfer, State history, and relay-side discovery ([ADR-0010](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0010-what-the-substrate-guarantees.md)); published ecosystem artifacts |
| **MAPS** (notation) | A draft notation (four primitives, a composition hierarchy, a small pattern library). The notation itself is open | Any real game notated end-to-end |
| **WOCS** | The three-event protocol shape, specified | Pinned kind numbers; any live offer/fulfill/ack cycle; any deployed coordination. The most speculative layer, and the only one with a Lightning dependency |
| **Spacewar! conversion** | Spacewar! 3.1 rewritten as RUNS source in [runs-spacewar](https://github.com/wScottSh/runs-spacewar). The PDP-1 compiler builds it back into the original binary, bit for bit; that identical binary is the oracle | Source shown pure: the purity report prints 0, but it does not yet count 83 copy transitions or one handoff Record, so purity is not shown. A documented, repeatable conversion process |

## The Oracle

A conversion is checked by compiling it for the original machine: the RUNS source must rebuild the original binary exactly. Anyone with the source can run that check, and a wrong conversion fails it in public. Spacewar! passes it today. What remains is making every Record and Processor pure while the binary stays identical, then writing the process down so it repeats for any game.

## What Would Count as Adoption

Later milestones, in order of difficulty: a MAPS score of a real, existing game written by someone other than the author; a second RUNS game; a machine-compiled build on a second platform; a published AEMS Entity referenced by a Manifestation the publisher didn't write; one WOCS cycle settling real work. None of these exist today.

**MIT License**
