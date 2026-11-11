# The EGS Correspondence Map

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary)**

---

This document owns the relationships between the EGS protocols. Every other
document in the corpus states only its own edge and links here; if a
relationship stated elsewhere disagrees with this map, this map wins.

## The Stack

```
RUNS source          →  Portable game logic (Records + Processors + Networks)
    ↓
DIGS expressions     →  Deterministic Processor bodies (bit-exact cross-platform when declared)
    ↓
Platform runtime     →  Native binary (compiled for target hardware)
    ↕
AEMS events          →  Signed game objects on the commons (optional seams only)
    ↕
WOCS coordination    →  Infrastructure market (hosting, services, settlement)

MAPS notation        →  Readable game mechanics
```

Each layer is independent. Replacing any component does not require changes to
the others.

## AEMS ↔ RUNS: things to source

AEMS and RUNS can meet at two optional seams, never per-frame. Neither
depends on the other, and a RUNS game can use no AEMS at all.

- **Build time** — a build can compile Manifestation data into the type
  definitions and lookup tables that Processors reference.
- **Load/save boundaries** — a game can read and publish Asset and State events
  between sessions.

## AEMS ↔ MAPS: things to rules

AEMS Entities name objects (a sword, a key, a monster). AEMS Manifestations
carry the roles those objects play in a game as tags by convention, and MAPS Patterns
describe the structural relationships those roles participate in (a
resource-acquire loop, a locked transition). The seam is optional. A designer
can notate the mechanical grammar in MAPS and publish the things in AEMS.

## WOCS ↔ everything: the coordination edge

WOCS coordinates the infrastructure around all of the above — hosting, audits,
asset commissioning, tournaments — through Offer/Fulfill/Ack with Lightning
settlement. It defines no game semantics; it is how work on any other layer
gets funded and settled.

## Who states what

| Document | States |
|----------|--------|
| This map | All cross-protocol relationships |
| Each protocol README | Its own edge only, with a link here |

**MIT License**
