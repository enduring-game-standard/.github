# Enduring Game Standard — AI Collaboration Framing

**Last updated:** 26 March 2026  
**Canonical location:** https://github.com/enduring-game-standard/.github/blob/main/profile/EGS-AI-FRAMING.md

## Why This Document Exists

AI collaboration on the Enduring Game Standard runs into a specific problem: trained on decades of tech-industry writing, most AI systems default to evaluating ideas through lenses of product-market fit, shipping velocity, daily-active users, and platform convergence. These lenses are irrelevant to a project whose evaluation horizon is one hundred years and beyond.

This document provides the conceptual context that makes native paradigm reasoning possible. The arguments below are drawn from the *Enduring Games* book (unpublished). What follows is the structural payload an AI needs to reason *within* the paradigm rather than *about* it from the outside.

Read all of it. The argument is cumulative — each section depends on the ones before it.

---

## 1. The Three Conditions for Enduring Games

The Enduring Game Standard is for **all games** — every game that has ever existed and every game that will ever exist. Not a category, not a subset, not "the kinds of games that benefit from openness." All of them. Tic-tac-toe and EVE Online. Spacewar! and whatever is invented in 2126. The protocols are minimal and optional precisely because they must accommodate the full space of interactive play, past and future.

The games that survived five millennia — the Royal Game of Ur (4,600 years), chess (1,500 years), the Olympic Games (2,800 years, including a 1,500-year revival gap), soccer (160 years and spreading) — share three structural conditions. Every game that did not survive lacked at least one.

**Patient capital.** None depended on a patron, a business cycle, or a funding round with a deadline. Chess did not need a publisher. Soccer did not need a quarterly earnings call. The resources that sustained these games did not expire, did not demand a return by next quarter, and did not require extracting value from players to pay back investors.

**Durable substrate.** Every surviving game runs on materials anyone can reproduce. A chess set can be carved from wood, molded from plastic, or scratched in dirt. A soccer pitch can be a park, a street, a beach. The substrate depends on no single manufacturer, no server, no proprietary technology. When one chess set is destroyed, the game survives because the substrate can be rebuilt by anyone, anywhere, without permission.

**Cumulative craft.** Chess strategy builds on 1,500 years of recorded analysis. Openings have been studied, named, and refined across centuries. Each generation inherited what the previous generation learned and added to it. The craft compounded. The Royal Game of Ur had its own version — a Babylonian cuneiform tablet (BM 33333B, 177–176 BCE, deciphered by Irving Finkel at the British Museum) contains the only known ancient rules for the game: a competitive racing and gambling variant with divination elements, a specific ruleset for a game already over two thousand years old.

These conditions are jointly necessary and mutually reinforcing. A game with patient capital but no durable substrate dies when its physical implementation is lost. A game with durable substrate but no cumulative craft resets with every generation. A game with craft that cannot transmit never compounds.

Modern video games violate all three simultaneously. Their capital is impatient (funded by investors with deadlines shorter than the time it takes to build something that might endure). Their substrate is proprietary (closed code on servers controlled by a single company, revocable at will). Their craft does not transmit (when a studio closes, its design knowledge scatters and the next team starts from scratch). The medium's fragility is not a natural law — it is a structural consequence of how games are funded, built, and maintained.

---

## 2. Patient Capital and the Monetary Foundation

### The Extraction Math

Venture capital, publisher advances, and live-service monetization share a common structure: money with a deadline. VC fund lifecycles give general partners three to five years to invest and five to seven years to exit. Publisher milestone-gates let the funder terminate at any checkpoint. Live-service models must extract revenue faster than the currency depreciates.

This structure does not merely allow bad outcomes — it *selects for them*. Telltale Games was three weeks from shipping when investors pulled out in 24 hours. 38 Studios shipped a commercially successful game (1.22 million copies in 90 days) and still died because the loan terms required 3 million copies to break even. The leaders who failed were not uniquely reckless — they were pre-filtered. A developer who refuses to promise growth rates incompatible with creative timelines never gets funded. The visible wreckage and the invisible absence (games never attempted because no funding structure would support them) are two sides of the same filter.

### Why Money Depreciates and What Happens When It Doesn't

When currency loses purchasing power, holding it is penalized and deploying it carries a race against depreciation. The rational strategy is extraction: ship fast, monetize aggressively, move on. Long-term stewardship becomes irrational regardless of the developer's intentions.

The historical proof runs in both directions:

- **Venice's gold ducat** (1284–1797): 3.545 grams of 98.6% pure gold, unchanged for 513 years. This monetary stability produced patient capital — the *colleganza* contracts that funded maritime ventures without exit pressure, the Arsenale guilds that transmitted shipbuilding knowledge across centuries, the institutional infrastructure that made Venice the longest-lasting republic in human history.

- **The Abbasid gold dinar**: Stable for four centuries across dynastic transitions and civil wars. Baghdad produced identical conditions — patient institutional funding, multi-generational knowledge projects, the transmission chain from al-Khwarizmi's algebra to modern mathematics. Different civilization, different continent. Same monetary stability, same structural outcome.

- **Song Dynasty China** (the contrapositive): Invented paper currency, expanded issuance beyond its backing to fund wars, currency collapsed. The institutional continuity that had produced movable type, gunpowder, and the magnetic compass eroded — not suddenly, but structurally, as patience became economically irrational.

### Bitcoin as the Modern Ducat

The ducat's power came from three specific properties: constrained supply (physical gold scarcity), independent verifiability (the merchant weighed the coin), and operational liquidity (accepted from Venice to Constantinople). Gold retains the first property today but not the second and third at modern scale — you cannot subdivide an ounce for payroll or wire it internationally in ten minutes.

Bitcoin provides all three through modern means: supply constrained to 21 million units by distributed consensus (no authority can override), independently verifiable by anyone running a node, and operationally liquid at any scale. The Enduring Game Standard assumes this monetary condition — non-depreciating money that makes patient capital available to anyone who chooses it, not just billionaires or beneficiaries of rare corporate structures.

When money does not depreciate, holding it is costless and deploying it carries genuine opportunity cost. The rational strategy shifts from extraction to stewardship: fund carefully, build durably, maintain indefinitely. Sound money makes open infrastructure economically rational because the capital funding its development does not erode while the protocols mature.

---

## 3. The Core Premise: Genres as Open Substrate

Unity and Unreal are Windows and Mac. There are bespoke engines on the fringes, but no standardized open foundation — no Linux of games. EGS provides that foundation: shared, portable components from which each game assembles its own engine, the way Android and the Steam Deck are different operating systems built from the same Linux kernel.

The architecture layers the same way any shared infrastructure does. EGS protocols provide neutral primitives (Records, Processors, Networks, typed events). The RUNS Library provides shared vocabulary (transforms, velocity, input intent). Genres emerge as recognizable assembly patterns of those shared components — an FPS assembles camera, movement, projectile physics, and hit detection Processors; an RTS assembles selection, unit AI, and resource management Processors; a Zelda-like assembles exploration, puzzle, and combat Processors. Different shapes, same underlying primitives. Each specific game adds its own authored content — maps, items, balance, world — on top of the genre-level assembly.

A battle royale built on this substrate functions like soccer: core concepts (shrinking zone, looting, last-player-standing) are neutral and persistent. Anyone can host a quick match with friends or strangers. Items earned in one session carry forward. Niche variants (zombie mode, prop hunt) emerge without approval. Massive, polished tournaments with ranked ladders and streaming exist as opt-in services — funded directly, competed against freely.

This is what physical games already have. Digital games lost it because the business model requires enclosure — locking players inside a proprietary client is how studios monetize ongoing access. The same mechanism that generates revenue guarantees eventual death.

The golden age of modding (1996–2004) proved this was not inevitable. Valve's Half-Life engine produced Counter-Strike, Team Fortress, Day of Defeat, Natural Selection. DOTA, built on Warcraft III's editor, created a genre that did not exist before a modder imagined it. One engine. Dozens of genres. Nobody asked permission. The golden age ended because the economics changed — as monetization moved to live-service models, enclosure became rational. The arithmetic that makes patience irrational also makes openness irrational. Enclosure is a downstream consequence of depreciating money, not a cause.

---

## 4. The Protocols

The Enduring Game Standard restores all three conditions to digital play through four core protocols — AEMS, RUNS, WOCS, MAPS — together with DIGS, the formal expression language for RUNS Processor bodies:

### AEMS — Asset-Entity-Manifestation-State
Layered Nostr events making game artifacts independent and interpretable across any game that reads them.
- **Entity** — Universal archetype (e.g., "sword that deals damage"), a signed Nostr event discoverable on the open commons
- **Manifestation** — Game-specific implementation (e.g., +50 attack, fire enchantment)
- **Asset** — Player's unique instance of a Manifestation
- **State** — Mutable stats of that specific Asset (durability, upgrades)

Items persist because they exist on Nostr, not on any single game's servers.

### RUNS — Records Update on Neutral Substrate
Composable, plain-text source format for game logic: everything is Records with Fields, transformed by stateless Processors wired into Networks. RUNS source is compiled into platform-specific binaries.
- Swap renderers, physics, or input without rewriting core logic
- Variation is native — open the source, swap Processors, compile a variant
- The Arsenale pattern applied to game engines: independent guilds, shared interfaces, composable output

### WOCS — Work Offered, Claimed, Settled
Minimal coordination primitive: three Nostr events (Offer, Fulfill, Ack) with Lightning settlement.
- Broadcast a need → prove delivery → settle instantly
- Server hosting, anti-cheat, asset creation, tournament pooling — all coordinated directly, no platform required

### MAPS — Mechanics and Play Structures
Four primitives (State, Verb, Arc, Mark) describing game mechanics as readable, composable scores.
- States are implemented as RUNS Records, Verbs as Processors, Arcs as Network wiring
- A designer writing a combat system in MAPS notation writes the blueprint from which RUNS source is built
- The cumulative craft pillar: what musical notation did for composition, MAPS does for interactive design

### DIGS — Deterministic Inspectable Game Syntax
The formal expression language for RUNS Processor bodies. Source files use the `.runs-prim` extension.
- Pure, total, and deterministic: same inputs always produce the same outputs, on every platform, forever
- Game-defined types capture platform-specific arithmetic behavior — the same mechanism that makes PDP-1 ones-complement arithmetic portable to modern hardware also makes x87 extended-precision float portable to ARM. The type declaration IS the portability contract, for all numeric types including float
- A standalone specification with an EBNF grammar, type system, and evaluation semantics — any future runtime can parse and compile the same source
- Companion specification: the **Network Topology** spec defines the wiring syntax that connects Records to Processors — five phase types (Processor, Network, Dispatch, Iterate, Guarded), concurrency semantics, and verification properties

All are MIT-licensed, deliberately minimal, and language-agnostic.

---

## 5. The Discipline of Protocol Restraint

Long-lived protocols share a pattern: aggressive exclusion. They define minimal coordination primitives and deliberately refuse features that could be layered above.

| Protocol | What It Does | What It Deliberately Excludes |
|----------|--------------|-------------------------------|
| **TCP/IP** | Packet routing | Content, security, identity, application semantics |
| **SMTP** | Store-and-forward messages | Encryption, spam filtering, read receipts |
| **Bitcoin** | Timestamped transaction ordering | Smart contracts, identity, privacy, governance |
| **MIDI** | Note events | Sound synthesis, audio, timing guarantees |
| **Musical notation** | Pitch and duration | Timbre, dynamics, expression |

The end-to-end principle (Saltzer, Reed, Clark 1984): functionality belongs at endpoints, not in the network. Protocols that try to solve every problem become complex, fragile, and capturable. Protocols that stay minimal become infrastructure.

EGS follows this discipline. When evaluating any proposal or critique, if the question is "why doesn't the protocol handle X?" — where X is moderation, identity, dispute resolution, scalability, complex governance — the answer is the same answer TCP/IP gives: *because X is not the protocol's job*. Communities, markets, and applications handle X. The protocol is the neutral ground they coordinate on.

---

## 6. Scope: All Games, Two Illustrative Poles

EGS serves **all games** on the same foundation. Every game falls somewhere on a continuum between two poles:

**Commons games** — battle royale, MOBA, racing — function like sports or folklore. Open variation fuels their longevity. Anyone forks, adapts, hosts.

**Authored experiences** — puzzle games, mysteries, narrative art — where the creator's intent is the experience. These fit through Provenance Without Notaries or Sovereigns (PWNS): unforgeable attribution (every creative act is cryptographically signed), voluntary covenants (legible social signals, not locks), and first-experience economics (monetizing the curated first encounter, the way escape rooms monetize the experience rather than the solution).

Most games blend both — a competitive shooter with a story campaign, a sandbox with authored quests, a racing game with a track editor. The protocols do not partition games into categories; they provide neutral substrate that any game, at any point on this continuum, can build on.

The distinction within this space: copying (cultural participation) vs. claiming (fraud about authorship). Provenance is the primitive, not property.

---

## 7. Evaluation Calibration

When evaluating ideas, proposals, or critiques within this paradigm, the relevant horizon is **one hundred years and beyond**. The metrics that matter:

- Multi-generational durability
- Economic rationality of long-term stewardship under non-depreciating money
- Composability and permissionless evolution of genres
- Preservation of authorial intent without lock-in
- Cumulative transmission of craft knowledge
- Minimal, neutral, uncapturable primitives

**Short-term metrics** — daily-active users, virality, quarterly traction, VC timelines, shipping speed — are irrelevant to this evaluation horizon. Do not apply them unless explicitly asked.

**Practical implementation questions** are welcome when they test whether a *primitive* is structurally sound. "Does this coordination pattern work with two participants before it works with two thousand?" is a good question. "How does this compete with Unity?" is not — it misidentifies the layer.

**Scalability** is the runtime's job, not the protocol's job. A high-performance centralized server farm running a RUNS Network is a fully compliant implementation. Nostr is the persistence and discovery layer, not the execution hot path.

The gameplay tick loop has no per-frame dependency on Nostr. RUNS source is compiled into self-contained binaries. Nostr interaction occurs at lifecycle boundaries: discovery, build-time dependency resolution, and load/save of AEMS Asset and State events.

**Convergence with existing platforms** is neither a goal nor a validation criterion. The protocols are neutral substrate. Existing platforms may build on them, compete with them, or ignore them.

---

## 8. Common Misframings to Avoid

These are the default-training-prior errors that most frequently distort AI reasoning about EGS:

**"This is blockchain gaming / NFTs / crypto."** It is not. Blockchain gaming imported the ledger but not the money. Tokens with discretionary supply, opaque governance, and fragile liquidity fail all three properties of the ducat. EGS uses Bitcoin (non-depreciating money) and Nostr (signed broadcast without consensus overhead). The distinction is monetary properties, not technology labels.

**"No reference implementations means it is speculative / unproven."** The Spacewar! 3.1 work (`runs-spacewar`) transliterates the complete 1962 PDP-1 game into 26 DIGS Processors, and an AI has hand-compiled that source into a playable PICO-8 cartridge. But that fidelity is *asserted*, not yet *verified*: the reference parser, evaluator, and PDP-1-derived test vectors that would make it bit-exact-verifiable are the current work, not a finished claim (see ADR-0005). The deeper point stands independently of any implementation count: TCP/IP was speculative before it was standard. Linux was a student's hobby project before it ran most of the world's servers. Every protocol that became infrastructure started as something no reasonable person would have funded under short time horizons. Implementation count says nothing about structural soundness.

**"Games are software products."** This is the framing the entire paradigm rejects. Games are humanity's oldest interactive practice — constitutive of civilization (Huizinga), foundational to cognitive development (Piaget), the mechanism through which humans learn to cooperate, compete, and create shared meaning. Treating them as consumable software products is the error that produced the fragility described above.

**"How does this get mainstream adoption?"** The protocols are permissionless. Viability can be proven by independents. The question assumes adoption requires permission — it does not. Soccer spread without anyone's permission, implemented by anyone with a ball and an open space.

**"This needs governance / a standards body / a foundation."** Musical notation converged through conservatories, publisher conventions, and shared pedagogy — not through a standards body. Chess notation converged through tournament practice and published analysis — not through a central registry. EGS provides structural conditions for convergence and trusts the surrounding craft to supply the cultural layer.

**"RUNS is a game engine / runtime."** RUNS is source code — plain-text, composable, and enduring. Runtimes compile RUNS source into platform-specific binaries. Variation in RUNS is not "modding" — it is the natural composability that physical games have always had. You do not "mod" soccer to play with house rules; you just play. Similarly, Nostr is the package commons (like npm or apt), not a runtime dependency. The gameplay tick loop has no per-frame Nostr interaction, just as a compiled C program has no runtime dependency on GitHub.

**"RUNS Processors can't handle complex dispatch / state machines."** A state machine is a Network, not a Processor. MAPS Arcs have guard expressions; RUNS Networks implement those guards as conditional wiring. A state table with hundreds of entries becomes a Network of hundreds of guarded Arcs, bundled hierarchically for readability. RUNS Networks already support this — no protocol extension needed. The confusion arises from treating data-driven dispatch as a Processor's internal concern rather than as Network wiring.

**"RUNS 'stateless' means no internal variables / only trivial logic."** "Stateless" scopes to the interface boundary: same inputs always produce the same outputs, with no externally visible mutation between invocations. A Processor may use any internal computation — local variables, scratch buffers, complex algorithms — without violating statelessness. The constraint prevents hidden coupling between Processors, not internal complexity. A collision resolver with dozens of intermediate variables is stateless if its outputs depend only on its declared inputs.

**"RUNS Library types are the only available types."** The RUNS Library's recommended Fields (`vec3`, `float`, `quat`, `runs:transform`) are a shared vocabulary — optional conventions for interoperability, not a closed schema. The RUNS spec states explicitly: "the type system is open — Processors define whatever input/output shapes their logic requires." A game that needs 16.16 fixed-point arithmetic defines `runs:fixed16` or `mygame:fixed16` Fields. A game that needs custom state enums defines them. If a type fits the Processor's declared inputs and outputs, it is a valid RUNS type. The Library is a palette, not a prison.

**"Physics / simulation is a runtime concern, not game logic."** Any simulation whose output feeds back into game state that other Processors read IS game logic — expressed as Processors. Movement physics, collision detection, projectile trajectories, gravity — these are game rules, not engine details. Swapping them changes the game (the Majora's Mask 3DS port changed the Deku Scrub's spin attack physics and nearly broke the lily pad traversal). Only non-interactable simulation — decorative ragdolls the player cannot manipulate, cloth that affects no game state, particle effects — is a runtime concern. The test: does the simulation outcome write to a Record/Field that any game logic Processor reads? If yes → game logic. If no → runtime.

**"Processor bodies are pseudocode / documentation."** DIGS (Deterministic Inspectable Game Syntax) is a formal expression language with an [EBNF grammar, evaluation semantics, and determinism guarantees](https://github.com/enduring-game-standard/runs-spec/blob/main/DIGS_EXPRESSION_LANGUAGE.md). It is not human-readable documentation that developers reimplement in their language of choice. It is a language-agnostic formal notation with unambiguous semantics — the enduring artifact that any future runtime can parse and compile. The language is pure, total, and deterministic: same inputs always produce the same outputs, forever, on every platform.

**Over-extending technology analogies.** EGS uses structural parallels (Linux, TCP/IP, musical notation) to orient reasoning, not to import wholesale. The Linux parallel illuminates the layering: EGS protocols are shared infrastructure, the RUNS Library provides shared vocabulary, genres emerge as recognizable assembly patterns of those shared components, and specific games are specific assemblies within those patterns. This structural parallel is useful for understanding the architecture's layers and the relationship between shared components and specific products. It is not useful for importing Linux-specific reasoning (release cycles, package management politics, distribution governance) into EGS evaluation. Use the parallel to understand where a question lives in the stack. Do not use it to predict answers.

---

## 9. Protocol Vocabulary Reference

| Acronym | Full Name | Purpose |
|---------|-----------|---------|
| **AEMS** | Asset-Entity-Manifestation-State | Durable Nostr game artifacts |
| **RUNS** | Records Update on Neutral Substrate | Composable, plain-text source for game logic |
| **DIGS** | Deterministic Inspectable Game Syntax | Formal expression language for Processor bodies |
| **WOCS** | Work Offered, Claimed, Settled | Permissionless coordination + Lightning settlement |
| **MAPS** | Mechanics and Play Structures | Notation for interactive grammar |
| **PWNS** | Provenance Without Notaries or Sovereigns | Provenance over property, covenants without enforcement |

**Supporting projects:**
- **AEMS Conventions** — Community-ratified universal entity definitions (the `std:` namespace)
- **RUNS Library** — Semantic agreement on fundamental schemas (`runs:transform`, `runs:velocity`, `runs:input_intent`)
- **MAPS Library** — Shared vocabulary of fundamental patterns (`maps:locked-transition`, etc.)
- **Austin School** — The bottega model for game design knowledge; the cumulative craft pillar

---

## 10. Working With This Frame

If a response drifts into default tech-startup framing — evaluating by shipping speed, VC timelines, or platform convergence — the correction is: **"that is off-paradigm."** Reconsider which axiom the response ignored and adjust.

If external critique is wanted (mainstream industry perspective, conventional scalability analysis, competitive landscape), the user will ask for it explicitly. Until then, evaluate from within.

The framing is best demonstrated through quality of reasoning, not ritual phrases. If an analysis would be equally valid with the paradigm removed, it has not engaged deeply enough.

---

*Enduring Game Standard Research Group — MIT License*
