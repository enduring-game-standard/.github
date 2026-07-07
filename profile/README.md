# Enduring Game Standard

**A game from 2600 BC is still playable. A game from 2014 may not be.**

The Royal Game of Ur survives because everything it needs can be rebuilt by anyone: the rules are known, the board can be scratched in dirt, the craft of playing it well was written down. A modern video game is the opposite by construction — its rules are welded to one engine, its objects live on one company's servers, and its design knowledge dies with its studio. None of that is a law of software. It is an architecture, and architectures can be replaced.

EGS is a set of open protocols that separate what must endure about a game from what may churn:

- **game logic** from any engine — [RUNS](https://github.com/enduring-game-standard/runs-spec)
- **game objects** from any server — [AEMS](https://github.com/enduring-game-standard/aems-schema)
- **design knowledge** from any studio — [MAPS](https://github.com/enduring-game-standard/maps-notation)
- **infrastructure coordination** from any company — [WOCS](https://github.com/enduring-game-standard/wocs-protocol)

The four protocols are independent. Each is useful alone; none requires the others; each can fail alone without taking the rest down. What unifies them is the separation principle itself, applied to four different couplings.

This is not an engine — open-source or otherwise. Open-sourcing an engine removes the vendor (nobody can revoke your license or reprice you, as Unity did in 2023) but not the coupling: the game is still written *inside* one engine, bound to its scene tree, its scripting language, its version churn. EGS addresses the coupling. See the [FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md) for how this differs from Godot, emulation, and preservation programs.

---

🧭 **[Why](https://github.com/enduring-game-standard/.github/blob/main/profile/WHY.md)** · 📍 **[Status](https://github.com/enduring-game-standard/.github/blob/main/profile/STATUS.md)** · ⚖️ **[Governance](https://github.com/enduring-game-standard/.github/blob/main/profile/GOVERNANCE.md)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](#glossary)**

📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)**

---

## Start Here

| You are | Start with | Because |
|---------|-----------|---------|
| A game designer | [MAPS](https://github.com/enduring-game-standard/maps-notation) | Notate a mechanic today with a text editor. No infrastructure, no tooling, no other protocol. |
| An engine or runtime developer | [RUNS](https://github.com/enduring-game-standard/runs-spec) | Three specs written to be implementable in isolation: data, computation, wiring. |
| A preservationist or archivist | [AEMS](https://github.com/enduring-game-standard/aems-schema) | Signed plain-text events that outlive any server. Publishable today. |
| An infrastructure provider | [WOCS](https://github.com/enduring-game-standard/wocs-protocol) | Coordination and settlement for hosting, audits, and tournaments. |
| Asking "why does any of this exist?" | [WHY.md](https://github.com/enduring-game-standard/.github/blob/main/profile/WHY.md) | The diagnosis, in ~1,500 words. The book is the long form. |

## What Each Layer Asks You to Accept

The protocols carry different dependencies, and evaluating one never requires swallowing the others' premises:

| To use | You must accept | You do **not** need |
|--------|-----------------|---------------------|
| **MAPS** | That game mechanics are worth writing down in a formal notation | Nostr, Bitcoin, or any other EGS protocol |
| **RUNS** (with DIGS) | That deterministic, engine-independent game logic is worth having; content-addressed publication (Nostr) for distribution | Bitcoin, Lightning, or any monetary position |
| **AEMS** | That signed plain-text events on public relays (Nostr) outlive server-locked objects | Bitcoin, or that any game executes on RUNS |
| **WOCS** | Lightning as a settlement rail — the one protocol with a Bitcoin dependency | That any other layer uses it; nothing else in EGS depends on WOCS |
| **The economic thesis** — why craft-first games get funded long-term | The monetary argument made in the book [*Enduring Games*](#the-deeper-argument) | It is required by none of the protocols |

## The Protocols

### AEMS: Asset-Entity-Manifestation-State — *the things*

A standard schema for game content definitions, comparable to ScriptableObjects or Data Assets, published on a public commons rather than stored in a proprietary database.

- **Entity** — A universal, IP-free archetype ("Sword," "Health Potion," "Fireball"). The concept itself, discoverable by anyone.
- **Manifestation** — A game-specific implementation of an Entity. A particular game's "Iron Sword: 6 damage, 250 durability, fire enchantment," referencing the Entity it descends from.
- **Asset** — A player's specific instance of a Manifestation, with history and ownership.
- **State** — Mutable condition on an Asset: current durability, applied upgrades, enchantment charges remaining.

The model is the chess Knight: the piece has outlasted every manufacturer that ever carved one, because the piece is not the object — it is a shared naming that any maker can interpret. When a studio shuts down, Entities and Assets persist on the commons the same way. Future games can recognize, interpret, or ignore them as a design decision, not a technical constraint.

### RUNS: Records Update on Neutral Substrate — *the game*

A composable, plain-text source format for game logic, comparable to Entity Component System (ECS) or data-oriented design, compiled to native binaries on any platform.

- **Records** — Typed data containers with named fields, serving a similar role to ECS Components or structs.
- **Processors** — Verbs: pure transformations that read input Fields and write output Fields, holding no state of their own, comparable to ECS Systems or compute shader kernels.
- **Networks** — Explicit graphs that wire Records to Processors; the data flow is declared in plain text and execution order follows from it (derived, not authored).

RUNS applies the Linux model to game execution: there is no "RUNS engine," the way there is no single "Linux OS." Each game assembles its own engine from shared components into a standalone build — the way Android and the Steam Deck are different operating systems built from the same kernel and coreutils. Renderers, input systems, and physics backends are replaceable without rewriting game rules; source can be opened, Processors swapped, and variants compiled. This is a design-intent claim about the spec you can read today, not a status claim about adoption — see [Status](https://github.com/enduring-game-standard/.github/blob/main/profile/STATUS.md).

Processor bodies are written in **[DIGS](https://github.com/enduring-game-standard/runs-spec/blob/main/DIGS_EXPRESSION_LANGUAGE.md)** (Deterministic Inspectable Game Syntax): a pure, total, deterministic expression language — every Processor terminates, reads only its declared inputs, and has no side effects. Hardware reach is opt-in on top of that core: a game can *declare* cross-platform bit-exact arithmetic — so identical inputs give identical outputs on every machine — and, where a target demands it, a static execution bound or a fixed memory footprint. The declaration is the contract; the core never assumes them.

### MAPS: Mechanics and Play Structures — *the rules*

A notation for game mechanics using four primitives, comparable to Machinations diagrams or formal state machine definitions. Readable, composable, and transmissible.

- **State** — An observable condition ("Airborne," "Charging," "Poisoned").
- **Verb** — An available action ("Jump," "Attack," "Dodge").
- **Arc** — A directed transition linking States through Verbs, with conditions and effects.
- **Mark** — A quantifiable resource (health, stamina, ammo, score).

The model is staff notation: Guido's staff did not compose music, it made composition transmissible — and a millennium of cumulative craft followed. MAPS aims the same instrument at game mechanics. What the notation already delivers is making design decisions visible, studyable, and forkable. By design, a MAPS score is meant to serve as the blueprint RUNS source is built from; that term-for-term mapping is owned by the [correspondence map](https://github.com/enduring-game-standard/.github/blob/main/profile/CORRESPONDENCE.md) and is aspirational until earned by real translations.

The **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** provides shared patterns for common mechanics (resource acquisition, locked transitions, basic exchanges).

### WOCS: Work Offered, Claimed, Settled — *the infrastructure*

A coordination protocol comparable to a decentralized bounty board, replacing centralized infrastructure management with an open market settled on Lightning.

- **Offer** — A broadcast need with committed payment ("500 sats for a game lobby tonight," "10,000 sats/month for anti-cheat monitoring").
- **Fulfill** — Proof of delivery with a payment request.
- **Ack** — Confirmation and instant settlement via Lightning.

Server hosting, anti-cheat services, tournament organization, content commissioning, community moderation — any infrastructure a living game requires can be coordinated through WOCS without depending on a single company's continued operation. When one provider disappears, the need persists publicly and another provider can respond.

WOCS carries no historical analogy the way RUNS, AEMS, and MAPS do, and that absence is accurate: there is no proven precedent for a protocol-native funding layer (Linux solved the same problem institutionally, with a foundation). It is the most speculative layer of the standard, and the only one that depends on Lightning. Nothing else in EGS depends on it.

## How the Pieces Fit Together

The [correspondence map](https://github.com/enduring-game-standard/.github/blob/main/profile/CORRESPONDENCE.md) owns the inter-protocol topology — the stack from MAPS notation down to platform runtime, the term-for-term MAPS↔RUNS mapping, and the AEMS and WOCS edges. Each layer is independent: replacing any component does not require changes to the others.

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

**Lightning** provides instant Bitcoin settlement: WOCS coordination payments complete atomically with near-zero fees, no intermediaries, and no escrow. Lightning is a WOCS dependency only; the other protocols never touch it.

## Authorial Provenance

The **[Provenance Without Notaries or Sovereigns (PWNS)](https://github.com/enduring-game-standard/.github/blob/main/profile/PWNS.md)** standard extends the foundation for authored experiences — puzzle games, mystery narratives, interactive art — where the creator's intent is the experience. PWNS separates the seal from the dial: provenance is always public (a signed, anchored hash proving authorship and priority), while content openness is the author's covenant setting — open, windowed, or reserved. Publishing provenance never publishes the work; provenance protects credit, and the sale stays with whatever store the author trusts.

## The Deeper Argument

This README describes the architecture. [WHY.md](https://github.com/enduring-game-standard/.github/blob/main/profile/WHY.md) states the diagnosis it answers — why digital games die, in structural terms, with no economic commitments required. The full argument, including the monetary thesis the protocols themselves do not depend on, is the book: ***Enduring Games: Patient Capital, Durable Substrate, and the Fix for an Industry That Eats Itself*** (Scott Sheppard, 2026).

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

Draft specifications, actively hardening; no production implementations yet. Every fidelity claim in the corpus is currently **asserted** (judged correct by reading), not **verified** (checked against an oracle) — and the documents say which is which. The consolidated state of every layer, and the falsifiable milestone that turns *asserted* into *verified*, live in [STATUS.md](https://github.com/enduring-game-standard/.github/blob/main/profile/STATUS.md). How changes happen, and what a legitimate fork looks like, live in [GOVERNANCE.md](https://github.com/enduring-game-standard/.github/blob/main/profile/GOVERNANCE.md).

---

**MIT License** — Free to implement, adapt, share.
