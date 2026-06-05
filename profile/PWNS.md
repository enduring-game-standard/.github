# Provenance Without Notaries or Sovereigns (PWNS)

**A Voluntary Protocol Extension for Attribution, Covenants, and Creator Economics — Conceptual, January 2026**

---

The Enduring Game Standard serves games that resemble sports or folklore — commons by nature, where openness fuels longevity. But what of authored experiences? Puzzle games where the solution is the treasure. Mystery narratives where a single spoiler destroys the journey. Interactive art where the creator's intent is the experience itself.

These creations sit uneasily in purely open systems. The instinct is to reach for protection — DRM, enforcement, access control. But the most durable creative economies in history found a different path, one that turns openness into an advantage rather than a vulnerability.

The art world has operated on this path for centuries: provenance as the primitive, attribution as the currency, social accountability as the enforcement layer. These mechanisms predate copyright by five thousand years.

PWNS applies this principle to digital games. Every creative act carries its author's cryptographic signature via Nostr. Anyone can see who made it first. Communities enforce attribution norms the way academia enforces citation norms: through reputation, not litigation. And authors of revelation-dependent experiences — puzzles, mysteries, narrative games — can fund their work through first-experience economics, the same model that sustains a $7.9 billion escape room industry.

This works because Nostr provides the three properties that make provenance without gatekeepers possible: unforgeable identity (every user has a cryptographic key pair), open relay architecture (no single point of failure for provenance records), and permissionless discoverability (anyone can query for events by a given author across any relay). Without Nostr, PWNS would require a central authority to maintain provenance records. With it, the math replaces the institution.

## The Moral Intuition Worth Preserving

If you have ever built a puzzle game, written a mystery, or crafted a narrative with a deliberate reveal — you already know something that both IP-maximalists and IP-abolitionists tend to overlook. The discomfort when someone strips your name from your creation is not about ownership. It is about honesty.

What actually bothers authors is not copying. It is:

- **Attribution erasure** — Someone passing off a creative act as theirs
- **Parasitic extraction** — Harvesting value someone else created without reciprocity
- **Context destruction** — Characters that meant something in one world become hollow when ripped from it

These are relational violations — failures of honesty, reciprocity, and respect for creative labor. Traditional IP law bundles them with property rights, but the two are separable. Stephan Kinsella's *Against Intellectual Property* (2001) demonstrates the separation: genuine property rights apply to scarce resources, and copying a non-scarce good does not deprive the original creator of anything. Yet even Kinsella distinguishes copying from plagiarism. Plagiarism is fraud — a lie about origin. The moral claim against false attribution stands on its own, independent of any property framework.

PWNS preserves the moral claims that matter — attribution, reciprocity, honesty — while letting abundance flow.

## The Distinction: Copying vs. Claiming

Consider a sealed letter. Anyone *can* open it before the intended time. But the seal *signals* "this was meant to be opened by X, at Y moment." Breaking the seal is socially understood to diminish the experience. The letter still exists if opened early — it is not destroyed.

Mesopotamian cylinder seals worked this way from 3500 BC: rolled across wet clay to authenticate documents and signal authorial intent. They did not prevent opening. They provided tamper evidence. A broken seal indicated violation. The social cost was understood without legal mandate — for five thousand years before the first copyright statute.

Now consider two scenarios with a creative work:

**Scenario A: Copying**
Someone reads a novel, loves a character, and writes fan fiction. They extend the world, clearly derivative, often with attribution. The character lives beyond the original pages. This is participation in a cultural commons — often flattering, sometimes transformative.

**Scenario B: Claiming**
Someone takes a character wholesale, changes the name slightly, and publishes it as their original creation. They claim someone else's creative act as theirs. This is fraud — a lie about origin.

The first is cultural propagation. The second is theft of credit.

The moral distinction between them is sound, and PWNS makes it operationally precise. Every creative act carries its author's cryptographic signature. Copying flows naturally — culture propagates. Claiming is refuted by the math — the timestamp and signature prove who created what, and when.

## Core Principles

### 1. Unforgeable Attribution

Every AEMS Entity, RUNS Processor, or gamified element is a signed Nostr event with a timestamp. Every Nostr event is signed with the author's private key and carries a verifiable public key. If someone appropriates a character and claims authorship, the provenance chain refutes them instantly. The original is timestamped and immutably attributed.

This mechanism is inherent to the EGS protocol stack — no additional infrastructure needed. AEMS Entities carry their author's signature as a structural property, making attribution native to the data format rather than bolted on after the fact.

The result is stronger than traditional IP in one specific sense: proof does not require lawyers, registration, or courts. The cryptography proves origin the way a wax seal proved the sender — except the seal cannot be forged.

This mirrors how provenance works in the art world. A Vermeer painting's value comes from its documented chain of custody — gallery records, exhibition history, correspondence — not from a lock on the frame. When Han van Meegeren forged Vermeers in the 1930s, it was provenance research that exposed the fraud. PWNS provides the digital equivalent: an unforgeable provenance chain that anyone can verify.

### 2. Voluntary Covenants

Authors can attach use expectations to their creations — not as locks, but as legible social signals.

```yaml
entity:
  # ... standard AEMS fields ...
  covenant:
    license: "CC-BY-NC"
    attribution_required: true
    commercial_contact: "npub1abc..."
    covenant_text_cid: "ipfs://..."
```

These covenants are social signals, readable by communities and tools alike. Communities can filter, preference-rank, and curate based on covenant compliance. Reputation aggregators — themselves WOCS services that anyone can offer — track which author identities (Nostr public keys) honor covenants. Authors who consistently respect covenants accumulate social capital; their public keys become trusted brands.

You already participate in systems that work this way.

**Creative Commons**: 2.5 billion works licensed, rarely litigated, widely respected. The system functions through social norms. A journal that strips CC attribution loses academic standing. Courts have upheld the licenses when tested, but the day-to-day enforcement is reputational.

**Academic citation**: No law requires citing sources. Plagiarism detection is run by institutions, not courts. The consequences are social — failed grades, retracted papers, ruined careers, inability to secure funding. The system works because academic reputation is the currency of the profession.

**Open-source software**: Git tracks authorship at the line level. Code plagiarism is enforced socially — community disapproval, loss of maintainer trust, damaged professional reputation. No legal mandate. The system works because developer reputation is career capital.

Elinor Ostrom's research (Nobel Prize in Economics, 2009) demonstrates why these systems succeed. Her eight design principles for governing shared resources — clearly defined boundaries, monitoring, graduated sanctions, conflict resolution — describe exactly how PWNS covenants function. Covenants define boundaries. Communities monitor compliance. Reputation costs provide graduated sanctions. Dispute mediation services emerge via WOCS.

### 3. First-Experience Economics

For authored experiences — puzzles, mysteries, narrative games — the value is often in the first encounter. A mystery loses its tension once solved. A puzzle game's reward is the journey to solution.

This creates natural economics that already operate at massive scale.

**Escape rooms** are a $7.9 billion global industry (2022, projected $31B by 2032). Customers pay $25–45 per person for an experience that can be completely spoiled by a single search engine query. The solutions are not secret. The value is in the curated first encounter — the locked room, the timed pressure, the collaborative discovery. Escape room operators manage spoiler risk through community norms, not enforcement: no-photography guidelines, game master facilitation, and voluntary restraint by players who value preserving the experience for others. Profit margins exceed 50%.

**Theater tickets** work the same way. The script of Hamilton is published. Any high school can perform it. People pay $300+ for a Broadway ticket because the specific performance — the staging, the cast, the shared audience — is rivalrous even though the content is not.

**Bandcamp's pay-what-you-want model** has directed $1.3 billion to artists despite every album being freely streamable elsewhere. On Bandcamp Fridays, half of fans voluntarily pay above the minimum price. The payment is reciprocity, not access control.

PWNS applies this model to digital games through sealed content markers:

```yaml
experience:
  sealed_content:
    revelation_order: ["prologue", "act1", "twist", "finale"]
    sealed_elements:
      - id: "twist"
        reveal_condition: "act1_complete"
    experience_license: "unsealing-payment"
```

**A worked example.** An author publishes a three-act mystery game. Act One is open — anyone can play it, share it, discuss it freely. Acts Two and Three carry sealed markers indicating that the author intended a specific revelation sequence. A compliant client presents a WOCS payment prompt at the Act One boundary. The payment, settled via Lightning, compensates the author for the curated first experience. After completion, the player has received what they paid for. Whether the full content becomes open afterward is the author's choice, expressed through the covenant.

The sealed markers are signals — invitations to respect the author's intended experience. Clients that honor seals earn community trust, the same way Reddit communities organically developed spoiler-tag norms. No law prohibits spoilers. The norm arises because people value each other's first experience and recognize that the community is better when everyone participates in that respect.

**Implementation note**: Sealed content mechanics are conceptual. Clients may implement any approach to revelation sequencing — honor-system based, payment-gated, or hybrid. The standard specifies *what* (sealed markers, revelation order) not *how* (enforcement mechanism). RUNS containers carry the sealed markers. Clients choose how to respect them.

## Market Coordination via WOCS

The protocol does not enforce covenants, but markets can emerge to provide verification, reputation, and dispute resolution services — all coordinated via WOCS micro-economics.

**A concrete scenario.** A game designer publishes a puzzle game with covenant tags requesting attribution and first-play payment. Six months later, a second developer releases a similar game using the same puzzle mechanics. The original designer suspects appropriation. She posts a WOCS offer: "50,000 sats to anyone who can provide a verifiable provenance comparison." An attribution verification service — itself a small business — accepts the offer, retrieves both games' Nostr event chains, compares timestamps and content signatures, and publishes a public analysis. The escrow settles when the analysis is acknowledged.

This is one example. The same WOCS coordination enables:

- **Covenant compliance tracking** — Aggregators maintain public records of which identities honor covenants
- **Reputation markets** — Provenance chains become social capital; strong histories attract commissions
- **Dispute mediation** — Voluntary arbitration services for contested attribution

These are market opportunities, not protocol mandates. The No-Code Rule is preserved: if a feature can be implemented by a third party, it belongs to the third party.

## What PWNS Deliberately Excludes

PWNS focuses entirely on making provenance undeniable and expectations legible. Like copyright registration proves creation date independent of any enforcement action, PWNS proves origin — and leaves everything else to social coordination.

| Excluded | Why | Where It Belongs |
|----------|-----|------------------|
| **DRM / access control** | The protocol serves abundance, not artificial scarcity | Client-side choices, business models |
| **Enforcement** | The protocol signals expectations, not polices behavior | Social pressure, reputation markets |
| **Takedowns** | The protocol is permissionless | Community moderation, legal systems |
| **Content verification** | The protocol proves origin, not quality | Curation services |
| **Licensing compliance** | The protocol publishes covenants, not audits | Third-party monitoring via WOCS |
| **Trusted authorities** | No signers, lineages, or governance gatekeepers | Decentralized verification |
| **Friction on honest users** | Attribution and provenance are zero-cost | Inherent to Nostr signed events |

PWNS makes provenance undeniable and expectations legible. Everything else is social coordination — enabled by the protocol, not embedded in it.

## The Unified Vision

Authors of authored experiences and curators of commons games coexist on the same foundation:

- A puzzle game publishes sealed content with covenants requesting first-play payment
- A folklore-like battle royale publishes fully open with "attribution appreciated"
- Both persist on Nostr, discoverable and inheritable without corporate gatekeepers
- Both benefit from WOCS-funded infrastructure
- Both rely on social coordination and provenance, the same mechanisms that sustain every durable creative economy

The authored work's value is in the experience — captured at revelation, not locked forever. The commons work's value is in infinite variation — enabled by openness. Both are products of creative labor deserving attribution. Neither requires DRM.

When expressed in MAPS Notation, a puzzle designer's revelation structure becomes composable. Other designers can study the pattern — the sequencing of sealed elements, the arc from open prologue to gated climax — and build on it. Provenance chains make this lineage legible. The craft compounds across creators the same way chess openings compounded across centuries of recorded analysis.

Abundance operates through reciprocity: Nostr makes every creative act attributable, and communities naturally reward those who participate honestly. Provenance is the primitive: the creator's revenue comes from first-experience economics — from being the *origin* — the way a bottega master's commissions came from reputation, not from locking technique behind guild walls. The signature proves origin. The community recognizes it. The craft compounds.

---

## Status and Invitation

This is a conceptual proposal for how authored experiences fit within the EGS philosophy. It invites experimentation:

- Publish an Entity with covenant tags
- Build a simple reputation aggregator service
- Design a client that respects sealed content markers
- Explore first-experience payment flows

Feedback welcomed on: covenant semantics, sealed content mechanics, reputation market design, and philosophical gaps.

**MIT License** — Open for implementation, adaptation, and critique.
