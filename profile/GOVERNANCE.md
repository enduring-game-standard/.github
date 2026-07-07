# Governance

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 🧭 **[Why](./WHY.md)** · 📍 **[Status](./STATUS.md)** · ❓ **[FAQ](./FAQ.md)**

How the standard changes, who decides, and what your options are if you disagree.

## The Present Arrangement: A Cathedral

The base specifications — the AEMS schema, the RUNS spec and DIGS, the MAPS primitives, the WOCS events — are **closed**: they change when their author changes them. There is no RFC process, no committee, no voting, and none is implied.

This is a deliberate posture for a pre-paradigmatic standard. Before a first implementation has corrected the specs, design-by-committee would average away exactly the coherence a standard needs most; the priority is that the primitives stay minimal and mutually consistent while they harden. Musical notation and chess notation both converged through practice and publication, not through a standards body — the bet is that convergence here works the same way.

## The Open Doors

The layers **above** the kernel are where outside input lands today:

- **Conventions and libraries** — [AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions), the [RUNS Library](https://github.com/enduring-game-standard/runs-library), the [MAPS Library](https://github.com/enduring-game-standard/maps-library) — accept issues and pull requests. Decisions are currently made by the author; proposals are weighed for neutrality, minimality, and composability.
- **Per-repo issue trackers** — each specification takes bug reports, ambiguity reports, and hard questions in its own tracker. An ambiguity in a standard is a defect, not a cosmetic issue; reporting one is a contribution.
- **The commons itself** is permissionless by construction. Publishing entities, patterns, scores, or processor bodies requires nobody's approval, including the author's.

## Forking Is Legitimate

Everything here is MIT-licensed plain text. If you think a specification is wrong and the author disagrees, forking is not a hostile act — it is the mechanism working. A legitimate fork:

- takes any spec, changes what it wants, and publishes under its own name;
- does not need permission, and will not be discouraged;
- should not *claim to be* the Enduring Game Standard — call it something else, so implementers know which contract they are reading. That is the only ask.

## Supersession

The standard claims no exclusivity over the problem. If a better notation, schema, or source format exists or emerges, it should win, and this project will say so — the diagnosis (see [WHY.md](./WHY.md)) matters more than these particular protocols. Concretely: where prior art already solves a sub-problem well, the specs defer to it rather than duplicating it, and a demonstrated-better alternative to any layer is grounds for deprecating that layer, not defending it.

## The Bus Factor

This standard has one author. The mitigations are structural: every specification is plain text under an MIT license in public repositories; the design rationale is recorded in the specs and at book length in *Enduring Games*; and nothing grants the author ongoing control — the day the specs are worth continuing without him, anyone can.

## When This Page Changes

The cathedral posture is scoped to the pre-implementation era. Once a reference implementation exists and the first *verified* claims land (see [STATUS.md](./STATUS.md)), the base specs will have met reality, and this page will be revisited — at minimum to define how implementation experience feeds back into spec changes.

**MIT License**
