# Enduring Game Standard

**The Linux of game architecture.** Open protocols for portable game logic, persistent content, and transmissible design knowledge.

Unity and Unreal are Windows and Mac. There are bespoke engines on the fringes, but no standardized open foundation — no Linux of games. Every studio builds on infrastructure it does not control, cannot inspect, and cannot take with it when the terms change or the platform dies. When Unity restructured its pricing in 2023, every studio built on Unity discovered what it means to depend on infrastructure owned by someone else.

EGS provides shared, portable components from which each game assembles its own engine — the way Android and the Steam Deck are different operating systems built from the same Linux kernel. Game logic is plain-text source that compiles to native binaries on any platform. Content definitions live on a public commons. Design knowledge is written in formal notation that transmits across generations.

---

📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](#glossary)**

---

## The Protocols

### AEMS: Asset-Entity-Manifestation-State — *the things*

A standard schema for game content definitions, comparable to ScriptableObjects or Data Assets, published on a public commons rather than stored in a proprietary database.

- **Entity** — A universal, IP-free archetype ("Sword," "Health Potion," "Fireball"). The concept itself, discoverable by anyone.
- **Manifestation** — A game-specific implementation of an Entity. A particular game's "Iron Sword: 6 damage, 250 durability, fire enchantment," referencing the Entity it descends from.
- **Asset** — A player's specific instance of a Manifestation, with history and ownership.
- **State** — Mutable condition on an Asset: current durability, applied upgrades, enchantment charges remaining.

When a studio shuts down, Entities and Assets persist on the commons — the way a chess Knight survives the bankruptcy of a chess manufacturer. Future games can recognize, interpret, or ignore those Entities as a design decision, not a technical constraint.

### RUNS: Records Update on Neutral Substrate — *the game*

A composable, plain-text source format for game logic, comparable to Entity Component System (ECS) or data-oriented design, compiled to native binaries on any platform.

- **Records** — Typed data containers with named fields, serving a similar role to ECS Components or structs.
- **Processors** — Verbs: pure transformations that read input Fields and write output Fields, holding no state of their own, comparable to ECS Systems or compute shader kernels.
- **Networks** — Explicit graphs that wire Records to Processors; the data flow is declared in plain text and execution order follows from it (derived, not authored).

There is no "RUNS engine." Each game is its own engine, assembled from shared components into a standalone build. Renderers, input systems, and physics backends are replaceable without rewriting game rules. Source can be opened, Processors swapped, and variants compiled — portable variation at the architecture level.

Processor bodies are written in **[DIGS](https://github.com/enduring-game-standard/runs-spec/blob/main/DIGS_EXPRESSION_LANGUAGE.md)** (Deterministic Inspectable Game Syntax): a pure, total, deterministic expression language — every Processor terminates, reads only its declared inputs, and has no side effects. Hardware reach is opt-in on top of that core: a game can *declare* cross-platform bit-exact arithmetic — so identical inputs give identical outputs on every machine — and, where a target demands it, a static execution bound or a fixed memory footprint. The declaration is the contract; the core never assumes them.

### MAPS: Mechanics and Play Structures — *the rules*

A notation for game mechanics using four primitives, comparable to Machinations diagrams or formal state machine definitions. Readable, composable, and transmissible.

- **State** — An observable condition ("Airborne," "Charging," "Poisoned").
- **Verb** — An available action ("Jump," "Attack," "Dodge").
- **Arc** — A directed transition linking States through Verbs, with conditions and effects.
- **Mark** — A quantifiable resource (health, stamina, ammo, score).

By design, MAPS States are meant to correspond to RUNS Records, Verbs to Processors, and Arcs to Network wiring — so a combat system written in MAPS could serve as the blueprint RUNS source is built from. That term-for-term correspondence is an aspiration the project hasn't yet earned by translating a real MAPS score into RUNS; what the notation already delivers is making design decisions visible, studyable, and forkable.

The **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** provides shared patterns for common mechanics (resource acquisition, locked transitions, basic exchanges).

### WOCS: Work Offered, Claimed, Settled — *the infrastructure*

A coordination protocol comparable to a decentralized bounty board, replacing centralized infrastructure management with an open market settled on Lightning.

- **Offer** — A broadcast need with committed payment ("500 sats for a game lobby tonight," "10,000 sats/month for anti-cheat monitoring").
- **Fulfill** — Proof of delivery with a payment request.
- **Ack** — Confirmation and instant settlement via Lightning.

Server hosting, anti-cheat services, tournament organization, content commissioning, community moderation — any infrastructure a living game requires can be coordinated through WOCS without depending on a single company's continued operation. When one provider disappears, the need persists publicly and another provider can respond.

## How the Pieces Fit Together

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

Each layer is independent. Replacing any component does not require changes to the others.

## Design Philosophy

Long-lived protocols share a pattern: aggressive exclusion. They define minimal coordination primitives and deliberately refuse features that belong at higher layers.

| Protocol | What It Does | What It Deliberately Excludes |
|----------|--------------|-------------------------------|
| **TCP/IP** | Packet routing | Content, security, identity, application semantics |
| **SMTP** | Store-and-forward messages | Encryption, spam filtering, read receipts |
| **Bitcoin** | Timestamped transaction ordering | Smart contracts, identity, privacy, governance |
| **MIDI** | Note events | Sound synthesis, audio, timing guarantees |
| **EGS** | Game architecture primitives | Databases, marketplaces, engines, matchmaking, moderation |

AEMS defines entity structure, not databases. RUNS defines data-flow composition, not rendering. WOCS defines coordination primitives, not escrow or reputation. MAPS defines interactive grammar, not execution timing.

## Infrastructure

The protocols run on **[Nostr](https://nostr.com/)** and **[Lightning](https://lightning.network/)**.

**Nostr** provides signed persistence: every artifact — entity definitions, game logic, coordination offers, notation scores — is cryptographically signed by its creator, stored across independent relays, and discoverable without permission. No single server's failure destroys the data. No platform's policy change revokes access.

**Lightning** provides instant Bitcoin settlement: WOCS coordination payments complete atomically with near-zero fees, no intermediaries, and no escrow.

## Authorial Provenance

The **[Provenance Without Notaries or Sovereigns (PWNS)](https://github.com/enduring-game-standard/.github/blob/main/profile/PWNS.md)** standard extends the foundation for authored experiences — puzzle games, mystery narratives, interactive art — where the creator's intent is the experience. PWNS provides cryptographic attribution (unforgeable proof of authorship), voluntary use covenants (legible social signals, not enforcement), and first-experience economics (monetizing the curated journey, not the content itself).

## The Deeper Argument

This README describes the architecture. The *Enduring Games* book (unpublished) argues *why* this architecture is necessary — tracing the economic forces that make the current industry structurally hostile to the games it produces, and why portable, composable, open game infrastructure becomes economically dominant under sound money.

## Glossary

The family names are **phonetic game verbs** ("he AIMS", "he RUNS", "he DIGS"); each expansion
is a memory aid, not a precise definition. The sections above define each protocol in full.

| Name | Expands to | Role |
|------|------------|------|
| **AEMS** | Asset-Entity-Manifestation-State | *the things* — durable, signed game content on the commons |
| **RUNS** | Records Update on Neutral Substrate | *the game* — plain-text source for game logic |
| **DIGS** | Deterministic Inspectable Game Syntax | *the computation* — RUNS's pure expression language (Processor bodies + Network guards) |
| **MAPS** | Mechanics and Play Structures | *the rules* — design-time notation for game mechanics |
| **WOCS** | Work Offered, Claimed, Settled | *the coordination* — optional, Lightning-settled market for ecosystem services |
| **PWNS** | Provenance Without Notaries or Sovereigns | *extension* — provenance for authored experiences |

The four **core protocols** are AEMS, RUNS, WOCS, and MAPS. **DIGS** is a sub-language of RUNS,
not a fifth protocol. **PWNS** is an optional extension.

## Current State

The protocol specs are written and actively hardening — reasoned in depth but still provisional, not frozen. A **Spacewar! 3.1** conversion — the 1962 PDP-1 game expressed as RUNS source with DIGS Processors — is in active development (not yet published); its fidelity is *asserted* (the source has been read and judged faithful), not yet *verified* against a PDP-1 oracle. Production runtimes, compilers, and tooling — including that oracle — are next.

---

**MIT License** — Free to implement, adapt, share.