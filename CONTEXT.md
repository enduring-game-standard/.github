# Enduring Game Standard — Working Vocabulary

The canonical domain language for the EGS protocol family — the words the team and its
agents think in. This supersedes the narrower acronym table in
[`profile/GLOSSARY.md`](profile/GLOSSARY.md) (kept as a public quick-reference until the
inbound links across the corpus are repointed). Definitions describe what each term *is*,
not how it is implemented.

> Multi-context note: the workspace is a meta-repo over several independently-published
> protocol repos. A `CONTEXT-MAP.md` and per-repo `CONTEXT.md` files are deferred until the
> workspace folder structure settles (egs-workspace#5). For now this is the single
> cross-protocol glossary.

## Language

### Protocol family

**EGS** (Enduring Game Standard):
The umbrella standard — four core protocols (AEMS, RUNS, WOCS, MAPS) plus DIGS and the PWNS
extension — built on Nostr (the commons) and Lightning (settlement). Aimed at a 100-year
horizon, not product-market fit.

**AEMS** (Asset-Entity-Manifestation-State) — *the things*:
The schema for durable, signed game content artifacts on the commons.

**RUNS** (Records Update on Neutral Substrate) — *the game*:
The plain-text source format for game logic. Not an engine; each game compiles its own.
Three layers: data (Records/Fields), wiring (Networks), computation (Processor bodies,
written in DIGS).

**DIGS** (Deterministic Inspectable Game Syntax) — *the computation*:
The pure/total/deterministic expression language of RUNS. Its home is Processor **bodies**
(`.runs-prim`, the full language with statements); its expression grammar is also reused for
Network **guard** expressions. Governed and shipped inside RUNS (the `runs-spec` repo) — a
sub-language of RUNS, **not** a fifth protocol. Scoped to the RUNS world: not used to write
MAPS (which has its own notation).
_Avoid_: "the RUNS language" (RUNS is the source format; DIGS is its expression language);
calling DIGS a protocol.

**MAPS** (Mechanics and Play Structures) — *the rules*:
The design-time notation for game mechanics. The blueprint RUNS source is built from.

**WOCS** (Work Offered, Claimed, Settled) — *the coordination*:
The permissionless coordination + Lightning-settlement protocol for ecosystem services.

**PWNS** (Provenance Without Notaries or Sovereigns):
An extension for authored experiences — provenance over property, covenants without
enforcement.

### RUNS terms

**Neutral Substrate**:
The plain-text RUNS source medium itself — the "Substrate" in the name. Neutral because
it binds to no engine, vendor, renderer, or hardware generation: Records *update on* it,
runtimes compile *from* it, and it is the thing that endures while platforms churn. The
neutrality is the whole point of the name.
_Avoid_: using "substrate" loosely for the runtime or the kernel — those *consume* the
substrate, they are not it. Not Nostr (that is the commons/distribution layer, a separate
concern).

**Record**:
A named collection of typed Fields — a data store. Holds state; the only thing data flows
through (every inter-Processor handoff lands in a Record). **Reuse is a spectrum**: from
**common** Records the conventions repo recommends for interop (`runs:player_location`) to
**game-specific** schemas nobody else imports (`spacewar:object`). All are published (so the
source on the commons is complete and findable), content-addressed, and referenced by ID. A
Record is data, not a transformer — it holds Fields and has no input/output signature of its
own.
_Avoid_: entity (reserved for AEMS), struct, component (use only as an ECS analogy).

**Field**:
A typed, named value on a Record (e.g. `position_x`, `runs:transform`). The type system is
open; the RUNS Library recommends shared shapes but does not close the set.

**Processor**:
A pure, stateless transformation that reads Fields and writes Fields. "Stateless" scopes to
the **interface boundary** — internal locals and scratch are unrestricted; the contract is
that outputs depend only on declared inputs. **Category follows behavior, not name** — a
Processor is whatever transforms Fields, a Record is whatever stores them (the duck test).
The protocol mandates **no** naming convention; recommended conventions (e.g. verb-named
Processors, noun-named Records) are the conventions repo's job, not the spec's. By that
conceptual fit `runs:transform` reads as a Processor — it names an action — and the data
bundle it implies is a Record (e.g. `runs:player_location`).
_Avoid_: function, system (ECS analogy only), kernel.

**Network**:
The explicit bipartite wiring of Records to Processors that defines execution order for one
tick. Data flows strictly **Record → Processor → Record**: never Record → Record (no
Processor means no verb, so no transformation) and never Processor → Processor (no Record
means no data). The Network draws a line from a specific Field into a slot on a Processor —
and is the only thing that knows both sides: Records and Processors are **mutually opaque**
(a Record knows nothing of a Processor's internals; a Processor knows nothing of a Record's,
nor where its inputs came from or its outputs go). A state machine is a Network (of guarded
arcs), never a Processor.

A Network is a **confluent colored Petri net**: places = Records, tokens = field values (the
colors), transitions = Processors, guarded arcs = gates. Its determinism is structural, not
conventional, and rests on two rules (ADR-0010): **single-assignment purity** (no shared
mutable place — a shared resource like the PRNG is a *threaded chain* of distinct values,
`prng_v0 → … → prng_vN`) and **partitioning guards** (every dispatch's guard set is mutually
exclusive *and* exhaustive over the color space). Given both, one tick is a DAG (the
cross-tick loop is the `state:` Records), incomparable Processors commute, and **execution
order is derived, never authored** — any topological order yields identical results, so a
runtime may parallelize freely.
_Avoid_: treating `phases:`/`order:` as primitives — they are a crutch for resources that
should be threaded; pure wiring makes order a derived fact (ADR-0010).

**Sub-Network**:
A Network used as a Processor from the outside — typed inputs/outputs at its boundary,
phases inside. The bundling/composition mechanism: a chunk of Record–Processor–Record logic
bundled into a larger function (e.g. `vec_x`, `vec_y`, `vec_z` → `vec_3`). A bundle is never
severed from its constituent parts — their provenance chain endures inside it (the bundle's
content references them by ID). The popular unit is usually the bundle; the atoms stay
addressable. Games compose from a finite set of shared atomic primitives the way a Linux
distro composes from coreutils.

**Boundary Record**:
A Record crossing the game-logic ↔ runtime line. **Inbound** (`requires:`) are written by
the runtime before a tick; **outbound** (`produces:`) are read by the runtime after.

**Runtime**:
The program *outside* the Network — it parses, compiles, schedules, renders, and feeds
boundary Records. A compliant runtime may be a constrained console or a server farm.
_Avoid_: engine (there is no "RUNS engine"); do not conflate with **hand-compilation**
(see Flagged ambiguities).

**Phase / Dispatch / Guard**:
A **Phase** is one ordered execution stage of a tick. **Dispatch** is a bounded iteration
over an entity collection. A **Guard** is a DIGS boolean on a Network arc that routes
to a Processor (the RUNS implementation of a MAPS Arc condition).

### Evolution

**Variant**:
A game derived from another by opening its Network and swapping, adding, or replacing
Processors, then compiling anew — the way chess and soccer accrue house-rule variants over
time. First-class and permissionless: enduring games *evolve through variation*; they are
not modified or forked. (ADR-0006)
_Avoid_: **mod / modding** — imports a second-class, needs-the-studio's-permission framing
EGS exists to reject. **fork** — acceptable only as the *act* of starting a variant; the
*result* is a **variant**, never "a fork".

### AEMS terms

**Entity**:
A universal, IP-free archetype ("Sword"). A signed Nostr event on the commons.

**Manifestation**:
A game-specific implementation of an Entity ("Iron Sword: 6 dmg").

**Asset**:
A player's specific instance of a Manifestation, with ownership/history.

**State (AEMS)**:
Mutable condition on an Asset (current durability, charges). See Flagged ambiguities — this
"State" is distinct from a MAPS State.

### MAPS terms

**State (MAPS)**:
An observable condition ("Airborne", "Charging"). Compiles to a RUNS Record.

**Verb**:
An available action ("Jump"). Compiles to a Processor.

**Arc (MAPS)**:
A directed transition linking States through Verbs, carrying conditions/effects. Compiles
to Network wiring (guarded arcs).

**Mark**:
A quantifiable resource (health, ammo, score).

### WOCS terms

**Offer / Fulfill / Ack**:
The three coordination events — a broadcast need with committed payment, proof of delivery,
and confirmation with instant Lightning settlement.

### Correspondence (design → source)

MAPS **State** → RUNS **Record** · MAPS **Verb** → **Processor** · MAPS **Arc** →
**Network** wiring · MAPS **Mark** → a **Field**.

## Flagged ambiguities

- **"State" is overloaded three ways.** (1) AEMS **State** = mutable Asset condition;
  (2) MAPS **State** = an observable game condition; (3) an entity's runtime status (e.g.
  Spacewar's `object_state` enum: ship/torpedo/exploding/…). Always qualify: "AEMS State",
  "MAPS State", or "object state".
- **"Arc"** = MAPS Arc (a notation primitive) vs. a Network arc (a guarded route in RUNS
  wiring). The latter *implements* the former. Qualify when context is unclear.
- **"Runtime" vs "hand-compilation"** — _Canon (ADR-0005)._ A **runtime/compiler** is
  a program that mechanically parses and executes RUNS source. A **hand-compilation** is a
  human or AI manually emitting target code (e.g. Lua) from RUNS source by reading it. The
  Spacewar `spacewar.p8` is a hand-compilation artifact; calling it "a runtime" is the
  source of a live contradiction in the public corpus.
- **"Verified" vs "asserted"** — _Canon (ADR-0005)._ **Verified** = output checked
  against an oracle (test vectors derived from PDP-1 emulator behavior). **Asserted** = a
  human/AI read the source and judged it correct. Today, every fidelity claim in the corpus
  is *asserted*; none is *verified*. The corpus currently uses "verified" for the latter.

## Example dialogue

> **Dev:** Is the Spacewar gravity Processor verified?
> **Maintainer:** No — it's *asserted*. An agent hand-compiled it to PICO-8 and judged it
> faithful, but there's no test vector checked against the PDP-1 emulator, so it isn't
> *verified*. Those are different claims and we keep them apart.
> **Dev:** But the framing doc says "bit-exact verification."
> **Maintainer:** That's the contradiction we corrected (ADR-0005). "Bit-exact" is a *Verified*-tier
> word; the `.p8` is a *hand-compilation*, and it even used adapted substitutions, so it
> isn't strict-evaluated either. The honest claim is "hand-compiled to a playable cartridge;
> mechanical verification is the open work."
