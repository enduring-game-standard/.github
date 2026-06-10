# Frequently Asked Questions: Enduring Game Standard

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary)**

---

## Overview

**What is the Enduring Game Standard?**  
Four minimal, interlocking protocols — AEMS (Asset-Entity-Manifestation-State), RUNS (Records Update on Neutral Substrate), WOCS (Work Offered, Claimed, Settled), and MAPS (Mechanics and Play Structures) — designed to make digital games as durable as the rulesets humanity has played for millennia. Built on existing open infrastructure (Nostr for persistent data and Lightning for settlement), they define durable game entities, composable execution, permissionless coordination, and a notation for interactive grammar. The aim is games that persist for generations, like chess or Go, without depending on any single company or server.

**Why does this matter?**  
Most digital games are architecturally fragile: they rely on centralized servers that can be switched off, revocable licenses that can be taken away, and proprietary engines that lock in assets and progress. The result is cultural loss — entire worlds, inventories, and communities disappear when support ends. This standard proposes an alternative path where players retain meaningful control, creators earn directly, and games can evolve indefinitely through open contribution.

**Is this production-ready?**  
Not yet. These are conceptual specifications with no reference implementations. The focus is on clear, minimal protocols that anyone can experiment with today. This is a long-term architectural proposal, not a finished product.

## For Players

**What could change for me as a player?**  
Over time, you might:
- Carry items, characters, or progress across different games via shared AEMS entities
- Retain ownership of assets through cryptographic keys, independent of any studio
- Earn sats for contributions (hosting servers, testing, or creating content) via WOCS
- Play games that survive studio closures, maintained by communities

**Will gameplay feel different?**  
Gameplay feels like whatever the runtime provides. The standard defines how game components are described, composed, and coordinated — it does not constrain how they execute. A high-performance centralized server running a RUNS Network produces the same player experience as any other high-performance game. Nostr is the persistence and discovery layer (where entity definitions and coordination offers live), not the execution hot path — the same way a game's database isn't queried 120 times per second during a raid.

**How can I get involved now?**  
- Follow development discussions
- Try posting an AEMS Entity definition on Nostr — no coding required
- Share feedback on what features matter most for longevity and ownership

## For Developers and Modders

**What advantages could this offer developers?**  
- AEMS allows defining entities once and reusing them across projects
- RUNS enables truly modular engines — replace renderers, physics, or input systems without breaking everything
- MAPS provides a notation for describing game mechanics as readable, composable scores — like sheet music for interactive grammar
- WOCS enables direct coordination of server hosting, asset creation, and feature development — instant settlement, no platform rents

**How realistic is adoption for indie or solo developers?**  
The protocols are deliberately lightweight. A solo developer could start by importing AEMS entities into a simple RUNS-style pipeline and accepting WOCS payments for custom work. No large team or funding required to experiment.

**What are the current technical limitations?**  
Nostr relay availability varies; some events may require multiple relays for reliable retrieval. Lightning payments work well for micro-transactions but routing can occasionally fail. These are known constraints of the underlying infrastructure, not the standards themselves.

**How do I start building?**  
Read the individual protocol READMEs and experiment freely. Start by publishing an AEMS Entity event, then try sketching a MAPS Score or wiring a simple RUNS Network.

## For Studios and Larger Teams

**What strategic benefits might studios see?**  
- Reduced duplication of effort through shared entity definitions
- New revenue models from persistent, player-owned economies
- Flexibility to pivot technology stacks without losing core content
- Games that continue generating value long after active development ends

**Does this require abandoning existing engines?**  
No. RUNS patterns can be incrementally adopted within existing codebases. AEMS entities can be imported alongside proprietary assets. Studios can participate selectively — monitoring, contributing feedback, or running internal experiments.

**How do we maintain creative control?**  
Games retain full authority over their Manifestations (AEMS) and Processor selection (RUNS). Shared entities are optional; proprietary ones remain possible. The standards enable interoperability without mandating it.

**Can this describe any kind of game?**  
Yes. MAPS can notate the mechanical grammar of any game — from Pong to a 200-player MMO. The notation describes structure and rules; it does not execute them. A RUNS runtime that executes a massive real-time game may be a centralized high-performance server farm — and that is a fully compliant RUNS runtime. The standard defines how components are described, composed, and coordinated. Execution architecture is the runtime's job. Nostr is the persistence and discovery layer — the distributed database where entity definitions and coordination offers live — not the execution hot path, any more than a library shelf is the concert hall.

## Technical Quick Reference

**Identity**: Your Nostr keypair is your universal identity across all protocols.

**Multiplayer**: Transport-agnostic. WOCS coordinates matchmaking; actual networking uses WebRTC, dedicated relays, or hybrids.

**Anti-cheat/trust**: No built-in enforcement. Third parties offer verification services coordinated via WOCS.

**Variants**: a variant opens a Network and swaps or adds Processors, then compiles anew — what other ecosystems call "mods," except enduring games evolve through variation rather than modding. Coordinated via WOCS.

**Protocol integration**: MAPS (what the grammar is) → AEMS (what the things are) → RUNS (how it executes) → WOCS (how ecosystem services are coordinated).

## Authored Experiences and Provenance

**Does this standard only work for "sports-like" games? What about puzzle games, mysteries, or narrative experiences?**  
The core protocols naturally serve commons-style games — those that benefit from open variation like folklore or sports. But authored experiences (puzzles, mysteries, artistic visions) fit the same foundation through [Provenance Without Notaries or Sovereigns (PWNS)](https://github.com/enduring-game-standard/.github/blob/main/profile/PWNS.md).

**How can authored works exist in an open system without being "spoiled"?**  
The key insight is that authored experiences monetize the *first encounter*, not eternal exclusivity. A puzzle game's value is in the journey of solving it — once solved, the solution naturally becomes known. PWNS enables:
- **Sealed content markers** — Authors signal which elements are revelation-dependent
- **First-experience payments** — Players pay (via WOCS) for the curated experience of proper revelation
- **Post-encounter openness** — Content may open naturally after the experience, contributing to cultural commons

This is like paying for a theater ticket: you pay for the experience of first revelation, not perpetual ownership of the script.

**What about someone copying my characters or story beats?**  
PWNS distinguishes between *copying* (cultural participation) and *claiming* (fraud about authorship). Every creative act is cryptographically signed with your Nostr keypair — origin is mathematically provable. Someone can riff on your work, but they cannot claim to have created it. The provenance chain proves you made it first.

**Can I set terms for how my work is used?**  
Yes, through voluntary covenants — metadata attached to your creations signaling your expectations ("attribution required," "commercial use needs license," etc.). These are purely social signals; the protocol doesn't enforce them. But communities and reputation markets (funded via WOCS) can track who respects vs. ignores covenants, creating social costs for violators.

**Isn't this just trusting people to be good?**  
Partly — but with transparency. Covenants are legible. Provenance is unforgeable. Reputation aggregators can surface who consistently honors vs. exploits creator expectations. The protocol enables markets for verification and dispute mediation, compensated via WOCS. Enforcement isn't baked in, but coordination tools can emerge.

## Long-Term Perspective

**What is the multi-decade vision?**  
An ecosystem of games that compound cultural value: assets that travel between titles, engines that evolve without breaking old content, communities that self-fund preservation and innovation. Games as living protocols, not disposable products.

**What if major publishers ignore this?**  
Open standards don't require permission. Viability can be proven by independents first, creating player demand that larger studios eventually respond to — or not. The protocols remain available either way.

**How will the standards evolve?**  
The protocol primitives are designed to be stable across generations. Evolution happens at the library and convention layers — new MAPS Patterns, new RUNS Library schemas, new AEMS Conventions — which version independently through community RFC processes. The primitives themselves change only when genuinely new concepts emerge, an event expected to be rare. No central governance — convergence through adoption, shared vocabularies, and institutional memory.

## Getting Started

**Easiest first step**: Publish an AEMS Entity event on Nostr defining a simple Entity (no coding required). See the [AEMS standard](https://github.com/enduring-game-standard/aems-schema) for event structure.

**Meaningful milestone**: A demo where an AEMS entity is shared between two game prototypes, with WOCS-compensated contributions and persistent state.

## Closing Note

The gaming medium deserves architectures that match its cultural significance. Centralized models have delivered incredible experiences but at the cost of premature obsolescence and captured value. This standard offers a different foundation — one rooted in openness, resilience, and direct human coordination. Whether it succeeds broadly or inspires better alternatives, the conversation itself moves the industry forward.

Experiment, question, build. The protocols are here for anyone to use.

**MIT License** — All specifications are free to implement and extend.