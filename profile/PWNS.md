# Provenance Without Notaries or Sovereigns (PWNS)

**A Voluntary Protocol Extension for Attribution, Covenants, and Authored Works — Conceptual, July 2026**

---

The Enduring Game Standard serves games that resemble sports or folklore — commons by nature, where openness fuels longevity. Authored experiences are different. Puzzle games where the solution is the treasure. Mystery narratives where a single spoiler ends the journey. Interactive art where the creator's intent is the experience itself.

The instinct is to treat this as a conflict: either the author locks the work away and it stands outside the standard, or the work joins the commons and the author's livelihood evaporates. PWNS rejects the conflict by separating two properties that are usually welded together:

- **Provenance** — who made this, and that they made it first. Always public. Always maximal.
- **Openness** — who can experience the content, and when. A dial the author turns, from encrypted-forever to immediate commons.

Publishing provenance does not publish the work. A signed Nostr event carries a *hash* of the content, not the content; the bytes live wherever the author chooses — encrypted, sold through a store, or fully open. Scholarly publishing has run this way at planetary scale for decades: a DOI is a public, permanent, citable record of authorship and priority over a paper that may sit behind a paywall forever. The identifier proves the claim. It reveals nothing the author did not choose to reveal.

## The Seal and the Dial

Mesopotamian cylinder seals authenticated documents from 3500 BC: rolled across wet clay, they proved who sent a thing and made tampering evident. They did not control who eventually read it. Authentication and access were separate concerns five thousand years before the first copyright statute. They still are.

**The seal is the standard's one invariant.** Every participating work publishes a signed provenance event: author key, content hash, covenant. Commons games and sealed mysteries carry the same seal.

**The dial belongs to the author.** The covenant states where the content sits and when, if ever, that changes:

| Dial setting | What it means |
|--------------|---------------|
| **Open** | Content public at publication. The folklore posture. |
| **Windowed** | Content access-controlled during a commercial window; the covenant names a commons arrival — a date, a condition, an estate decision. |
| **Reserved** | Content access-controlled indefinitely. |

One tradeoff is stated rather than hidden: a Reserved work whose keys are lost dies with them. The standard does not force any work into the commons. It makes the enduring path *available* and *legible* — the covenant is where an author who wants their work to outlive them says so, in a form tools and communities can read.

## Copying vs. Claiming

Consider two scenarios with a creative work:

**Scenario A: Copying.** Someone reads a novel, loves a character, and writes derivative fiction, with attribution. The character lives beyond the original pages. Cultural propagation.

**Scenario B: Claiming.** Someone takes the character wholesale, changes the name slightly, and publishes it as their original creation. A lie about origin.

Traditional IP law bundles these together with property rights. They are separable — even Stephan Kinsella's *Against Intellectual Property*, the strongest case against IP-as-property, distinguishes copying from plagiarism: plagiarism is fraud, and the moral claim against false attribution stands on its own, independent of any property framework.

PWNS makes the distinction operationally precise. Copying is governed by the author's covenant. Claiming is refuted by the seal: the signed, anchored hash proves who published what, first, without revealing anything the author kept sealed.

## Core Principles

### 1. Attribution That Holds

Every AEMS Entity, RUNS Processor, or authored work is referenced by a signed Nostr event. The signature is tamper-evident: any alteration breaks it, and anyone can verify it against the author's public key. No lawyers, registries, or courts are required to check it.

Two honest qualifications, built into the design rather than papered over:

- **"First" requires an anchor.** A Nostr event's timestamp is asserted by the publishing client and can be backdated. A priority claim is proven by anchoring the event hash to an external clock that nobody can rewind — an OpenTimestamps attestation in a Bitcoin block proves the work existed *no later than* that block. Sealed works claiming priority should anchor.
- **Signed is not stored.** Relays are not archives; an event no relay holds is gone. Tamper-evidence and persistence are different properties. Authors, communities, and services that care about a provenance record keep relays that hold it — the same way institutions keep the citation record.

This is how provenance works in the art world. A Vermeer's value rests on its documented chain of custody, not on a lock on the frame — and when Han van Meegeren forged Vermeers, it was provenance research that exposed him. PWNS is the digital equivalent: a chain of custody anyone can verify, for works nobody is required to expose.

### 2. Covenants: The Dial Made Legible

Authors attach their expectations to the seal — not as locks, but as machine-readable social signals.

```yaml
entity:
  # ... standard AEMS fields ...
  provenance:
    author: "npub1abc..."
    content_hash: "sha256:9f2c..."
    anchor: "ots:..."            # OpenTimestamps attestation
  covenant:
    attribution_required: true
    content: "windowed"          # open | windowed | reserved
    commons_at: "2036-07-01"     # date | condition | never
    commercial_contact: "npub1abc..."
```

Covenants work the way attribution norms already work at scale. Creative Commons licenses are respected overwhelmingly through social convention, not litigation. Academic citation is enforced by institutions and careers, not statutes. Git tracks authorship at the line level, and code plagiarism costs maintainer trust, not legal fees. In each system, reputation is the currency and attribution is how it is minted.

Reputation aggregators — WOCS services anyone can offer — track which author identities honor covenants. A public key that consistently respects the dial settings of others becomes a trusted brand.

### 3. What Pays: Credit, the Store, and the Next Work

PWNS is precise about what provenance can and cannot do economically.

**Provenance protects credit. It does not protect the sale.** A seal on a mystery does not stop the first buyer from posting the twist. Nothing at the protocol layer can, and PWNS does not pretend otherwise.

**The sale is protected where sales are always protected: at the store.** A client or storefront sells the windowed acts encrypted, exactly as any store does today. PWNS is deliberately neutral here — it neither requires nor forbids access control. The author keeps whatever paywall serves them; the difference is that the *record of authorship* no longer lives inside any store's database. The store can ban the account, shut down, or burn — the seal, the priority, and the covenant survive on the open relay network.

**What provenance buys is the career.** The bottega master's commissions came from his name, not from locking his technique behind guild walls — the technique circulated in treatises while patrons bid for the one thing that could not be copied: his hands and his next work. Provenance is how a body of work accrues undeniably to a name, and reputation is how that name converts — commissions, patronage, funding for the work to come. Why patient capital makes that conversion rational rather than romantic is the argument of the Enduring Games book; PWNS supplies the mechanism, the book supplies the economics.

**Reciprocity is invited, never tolled.** Clients may present a payment prompt at a natural boundary — the end of a free first act, the credits — settled over Lightning, addressed to the sealed author key. This is a thank-you made frictionless, not a gate. Where the author needs a gate, that is the store's job, above.

## Craft Compounds on Sealed Work

A sealed reveal does not seal the craft. Design knowledge has always compounded on closed works: chess theory grew from recorded games, not from access to any grandmaster's mind; film craft is taught from copyrighted films; the structure of a dungeon or a stamina system is read from play, notated, and transmitted without a line of source code changing hands.

The distinction is *studying the pattern* versus *forking the material*. Traditions compound by the first. MAPS Notation is built for it: a mystery's revelation structure — the sequencing of acts, the dependency graph of disclosures, the arc from open prologue to gated climax — can be notated spoiler-abstracted, structure without payload. Another designer studies the shape of the reveal without consuming it. The craft compounds across creators the way chess openings compounded across centuries, while every author's dial setting is respected.

## Market Coordination via WOCS

The protocol does not enforce covenants; markets can verify them.

**A concrete scenario.** A designer publishes a sealed puzzle game with an anchored provenance event. Six months later, another developer ships a suspiciously similar game and claims original authorship. The designer posts a WOCS offer: "50,000 sats for a verifiable provenance comparison." A verification service — itself a small business — retrieves both event chains, compares anchors and content hashes, and publishes the analysis. The anchored hash settles the question of who was first, without the sealed content ever being exposed.

The same coordination enables covenant-compliance tracking, reputation markets, and voluntary dispute mediation. These are market opportunities, not protocol mandates. The No-Code Rule is preserved: if a feature can be implemented by a third party, it belongs to the third party.

## What PWNS Deliberately Excludes

| Excluded | Why | Where It Belongs |
|----------|-----|------------------|
| **DRM / access control** | The protocol records expectations; it does not gate bytes | Stores and clients — which remain free to encrypt |
| **Revenue capture** | Provenance proves origin; it cannot make a sale | Stores (the sale), reputation markets (the career) |
| **Enforcement** | The protocol signals, not polices | Social pressure, reputation markets |
| **Takedowns** | The protocol is permissionless | Community moderation, legal systems |
| **Content verification** | The protocol proves origin, not quality | Curation services |
| **Trusted authorities** | No signers, lineages, or governance gatekeepers | Decentralized verification |

PWNS makes provenance undeniable and expectations legible. Everything else is social coordination — enabled by the protocol, not embedded in it.

## Scope, Stated Plainly

PWNS serves the full dial. A folklore battle royale publishes Open, and openness does the work. A novelist's interactive mystery publishes Windowed, sells through whatever store she trusts, and joins the commons on her schedule. A living author's magnum opus stays Reserved, its authorship provable for as long as anyone holds the record.

Two limits are stated because a standard that names its edges is stronger than one that claims none:

- **For one-shot revelation goods, the commercial window depends on the store's access control, not on the seal.** Provenance secures attribution and priority; only encryption secures an unspoiled first experience. PWNS interoperates with that machinery; it does not replace it.
- **Endurance requires eventual openness or persistent keys.** That is arithmetic, not policy. The covenant is where each author decides which side of it their work lives on.

---

## Status and Invitation

This is a conceptual proposal for how authored experiences fit within the EGS philosophy. It invites experimentation:

- Publish an Entity with an anchored provenance event and covenant tags
- Build a reputation aggregator that reads covenant compliance
- Design a client that renders dial settings legibly to players
- Notate a revelation structure spoiler-abstracted in MAPS

Feedback welcomed on: covenant semantics, anchoring practice, reputation market design, and philosophical gaps.

**MIT License** — Open for implementation, adaptation, and critique.
