# The EGS Correspondence Map

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary)**

---

This document owns the relationships between the EGS protocols. Every other
document in the corpus states only its own edge and links here; if a
relationship stated elsewhere disagrees with this map, this map wins.

## The Stack

```
MAPS notation        →  Design blueprint (readable game mechanics)
    ↓
RUNS source          →  Portable game logic (Records + Processors + Networks)
    ↓
DIGS expressions     →  Deterministic Processor bodies (bit-exact cross-platform when declared)
    ↓
Platform runtime     →  Native binary (compiled for target hardware)
    ↕
AEMS entities        →  Persistent content on the commons
    ↕
WOCS coordination    →  Infrastructure market (hosting, services, settlement)
```

Each layer is independent. Replacing any component does not require changes to
the others.

## MAPS ↔ RUNS: notation to source

The relationship is design-to-implementation: MAPS is the written score, RUNS
is the composition built from that score, and the compiled binary is the
performance. Term for term:

| MAPS (the rules, notation) | RUNS (the game, source) |
|----------------------------|--------------------------|
| State                      | Record                   |
| Verb                       | Processor                |
| Arc (with guard expressions) | Network wiring (guarded transitions) |
| Mark                       | Field                    |

This term-for-term correspondence is **aspirational, not asserted**: no one
has yet earned it by translating a real MAPS Score into RUNS source (or back).
What the notation already delivers is making design decisions visible,
studyable, and forkable; the mapping above is the design intent the first real
translation will test.

## AEMS ↔ RUNS: things to source

AEMS touches RUNS at exactly two lifecycle points — never per-frame:

- **Build time** — Manifestations are compiled into the type definitions and
  lookup tables that Processors reference.
- **Load/save boundaries** — Asset and State events persist player data to the
  commons between sessions.

## AEMS ↔ MAPS: things to rules

AEMS Entities define named roles (a sword, a key, a monster); MAPS Patterns
describe the structural relationships those roles participate in (a
resource-acquire loop, a locked transition). A designer notates the mechanical
grammar in MAPS and instantiates the things in AEMS.

## WOCS ↔ everything: the coordination edge

WOCS coordinates the infrastructure around all of the above — hosting, audits,
asset commissioning, tournaments — through Offer/Fulfill/Ack with Lightning
settlement. It defines no game semantics; it is how work on any other layer
gets funded and settled.

## Who states what

| Document | States |
|----------|--------|
| This map | All cross-protocol relationships, including the MAPS↔RUNS table above |
| Each protocol README | Its own edge only, with a link here |

**MIT License**
