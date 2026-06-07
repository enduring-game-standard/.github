# Enduring Game Standard — Working Vocabulary

The canonical domain language for the EGS protocol family — the words the team and its
agents think in, and the single source of truth for terminology (ADR-0008). This is the full,
precise vocabulary; the **public** quick-reference (acronym table + protocol roster) lives in
the org profile (`profile/README.md` § Glossary). The standalone `profile/GLOSSARY.md` was
retired — its content was redundant with both this file and the profile README. Definitions
describe what each term *is*, not how it is implemented.

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
Network **guard** expressions. Governed and shipped inside RUNS (the `runs` repo) — a
sub-language of RUNS, **not** a fifth protocol. Scoped to the RUNS world: not used to write
MAPS (which has its own notation).
**Mandatory core vs. opt-in reach-contracts** (ADR-0017): DIGS guarantees a small **core** on
every Processor — **verb/noun purity** + **per-tick totality** (terminates on every input;
structural, *not* a static step-ceiling) + **evaluation-order determinism** (precedence-fixed
order; no reassociation/FMA). Layered on top are **three optional, type-declared
reach-contracts**, each trading expressible games/targets for hardware reach, *none* required
for compliance: **(1) arithmetic determinism** (game-defined numeric types pin cross-platform
bit-exactness — else FP diverges, Quake-style); **(2) static execution bound** (optional WCET
for real-time — a tooling/Realization concern, not implied by totality); **(3) static memory
footprint** (optional list bounds for constrained/no-allocator targets — not implied by
purity/determinism/totality). The unifying rule: *the declaration IS the contract*, generalized
from arithmetic to execution and memory. Repeatedly, the draft doc **over-claims** by fusing a
reach-contract into the core ("pure ∴ deterministic"; "total ∴ statically bounded"; "pure ∴
bounded memory") — each is a separate property.
_Avoid_: "the RUNS language" (RUNS is the source format; DIGS is its expression language);
calling DIGS a protocol; reading "pure/total/deterministic" as implying static bounds or fixed
arithmetic (those are reach-contracts, ADR-0017).

**MAPS** (Mechanics and Play Structures) — *the rules*:
The design-time notation for game mechanics. The blueprint RUNS source is built from.

**WOCS** (Work Offered, Claimed, Settled) — *the coordination*:
The permissionless coordination + Lightning-settlement protocol for ecosystem services. It is
**purely optional for everything** — a formalized way to coordinate service provision and
maintenance through Austrian-economic incentives instead of centralized services. *An* enduring
solution, not a requirement: nothing in EGS depends on it. (Not yet formally grilled — this is
a placeholder pending its own pass.)
_Note_: WOCS coordinates *who provides a service and is paid* (hosting, audits, tournaments,
multiplayer-server provision) — **not** the real-time gameplay packets themselves. In-game
multiplayer **transport** is a Realization/Runtime concern (see `Processor` → Realization
Processor), filling and draining boundary Records; RUNS' kernel contains no transport.

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
through (every inter-Processor handoff lands in a Record). A Record **stores its value until a
Processor produces a new one — write once, never overwrite** (ADR-0012); a "change" is a fresh
value in a fresh place, not in-place mutation. **Reuse is a spectrum**: from
**common** Records the conventions repo recommends for interop (`runs:player_location`) to
**game-specific** schemas nobody else imports (`spacewar:object`). All are published (so the
source on the commons is complete and findable), content-addressed, and referenced by ID. A
Record is data, not a transformer — it holds Fields and has no input/output signature of its
own.
_Avoid_: entity (reserved for AEMS), struct, component (use only as an ECS analogy).

**Field**:
A typed, named value on a Record (e.g. `position_x`, `runs:transform`). The type system is
open; `runs-conventions` recommends shared shapes but does not close the set.

**Processor**:
A **verb**: a transformation that reads Fields and writes Fields and holds no state of its
own. The verb/noun split (a Processor transforms, a Record stores) is the **only** purity the
base protocol knows — it is the petri-net transition/place distinction, nothing more.
**Category follows behavior, not name** — a Processor is whatever transforms Fields, a Record
is whatever stores them (the duck test). By that fit `runs:transform` reads as a Processor (it
names an action) and the data bundle it implies is a Record (e.g. `runs:player_location`). The
protocol mandates **no** naming convention and **no** flavor distinction; at the schema level
there is exactly one kind of Processor. "Stateless" scopes to the **interface boundary**: a
body may use any number of intermediate computations, but its semantics are **single-assignment
dataflow** — imperative-looking reassignment is versioning and loops are bounded folds
(ADR-0012), never shared mutable state.

**Granularity is a spectrum, and decomposition is convention — not compliance** (ADR-0016).
A Processor's scope ranges from a fat **monolith** (a whole DOOM function re-expressed
verb-for-verb in DIGS) down to an atomic primitive (`vec_x`); *both ends are spec-compliant*.
A monolith that passes every test vector is flawlessly, deterministically the game — it is
merely **undecomposed**. The convention pulls rightward — split a body into DRY,
commons-published, forkable sub-Processors (referenced by Nostr ID, the way an npm package
pulls in others), each splittable until atomic, then bundled as a **Sub-Network** — but the
**spec mandates none of it**; there is no governing body on the commons to enforce it, and
compliance rests on the four primitives + DIGS, never on granularity. Corollary of the duck
test: **composition follows behavior, not original intent** — `vec_3` composes `vec_x/y/z` on
what they *compute*, not on what any of them was once "meant" for. (See `Phase / Dispatch /
Guard` for how this makes ADR-0013's "no Processor steers the Network" *decomposition-relative*:
vacuous for a monolith, binding once routing targets are distinct Processors.)

**Determinism is a separate, narrower property**, layered on by convention rather than the
protocol: a Processor whose outputs depend only on its declared inputs (pure, total,
deterministic). The enduring-games **pattern** (a conventions-repo convention, *not* a base
primitive) splits a game's Processors into two flavors by this property — named for their
*function*, not their implementation:
- **Rules Processor** — the game logic. Written in DIGS, hence platform-agnostic and
  deterministic; lives in the Rules region. Change one → a **Variant** (ADR-0011).
- **Realization Processor** — the layer that realizes the game on hardware in both
  directions: timing and input (inbound), rendering and audio (outbound), and **multiplayer
  transport** (remote inputs arrive as inbound boundary Records, exactly like local input).
  Platform-tailored; verb-pure but determinism-exempt; lives in the Realization region.
  Rip-and-replace the set → a **Port** (ADR-0011). The deterministic Rules region is what makes
  lockstep/rollback netcode possible, yet contains none of the transport. (*"Deterministic" here
  is the core **evaluation-order** determinism — identical results on one runtime, any topological
  order. **Cross-machine** lockstep additionally requires **arithmetic determinism**,
  reach-contract #1: without it floating point diverges across hardware (Quake desyncs); with it,
  value-level determinism plus no observable addresses give Factorio-style cross-platform lockstep.
  The unqualified "deterministic" throughout this glossary means the core property; cross-platform
  bit-exactness is always the opt-in contract. ADR-0017.*)
  **Its body is *not* DIGS, and the standard deliberately does not specify a language for it**
  — a Realization Processor is written in whatever its target demands (a throwaway Lua/Python
  script, a CUDA/Vulkan pipeline in C++ or Rust, MIPS-bound N64 homebrew). This is the exact
  inverse of DIGS: Realization bodies are **infinitely important but time-bound and
  hardware-bound**, where DIGS is time- and hardware-*un*bound. Specifying a portable
  Realization language would re-import the platform-coupling the boundary exists to quarantine.
  The fat/thin-Runtime line (see `Runtime`) decides how much Realization code a game even
  has; the boundary Records are the *entire* contract between the two languages. (ADR-0015)
The two flavors are joined only by boundary Records.
_Avoid_: function, system (ECS analogy only), kernel; treating the Rules/Realization split as
a base-protocol primitive (it is a conventions-repo pattern, not a schema distinction).

**Network**:
The explicit bipartite wiring of Records to Processors that defines execution order for one
tick. Data flows strictly **Record → Processor → Record**: never Record → Record (no
Processor means no verb, so no transformation) and never Processor → Processor (no Record
means no data). The Network draws a line from a specific Field into a slot on a Processor —
and is the only thing that knows both sides: Records and Processors are **mutually opaque**
(a Record knows nothing of a Processor's internals; a Processor knows nothing of a Record's,
nor where its inputs came from or its outputs go). A state machine is a Network (of guarded
arcs), never a Processor.

**Two composition planes, seamed at the Record.** A DIGS **sub-Processor call** in a body
(`let r = ns:proc(x)`) *looks* like Processor → Processor with no Record between — but it is
**not a Network edge**. It is **body-plane** composition: a pure DAG of calls whose
intermediates are **anonymous body-locals** (`let` bindings), never Records, that collapses
into **one opaque Network transition** when viewed from outside (governed by the totality
DAG-rule, not the bipartite rule). **Network-plane** composition is the bipartite graph proper,
where every intermediate is a **Record** — the only kind of intermediate that can be cross-tick
**state**, a **boundary** Record, or a **guard**-dispatched discriminant. The two planes are
the same scale-free relation cut at the **Record line**, and **bundling/unbundling is exactly
the act of crossing it**: promoting a body-local to a Record *unbundles* (exposes the
intermediate in the Network — now inspectable, dispatchable, state-able); demoting a Record to
a body-local *bundles* (hides it inside a transition). So "no Processor → Processor" never
breaks — Network-plane composition always lands in a Record; body-plane composition is interior
to a transition. (A sub-Processor call therefore returns a record-*shaped* **value**, not a
canonical Record.) This is the same axis as ADR-0016's decomposition gradient.

A RUNS Network is a **colored Petri net**: places = Records, tokens = field values (the
colors), transitions = Processors, guarded arcs = gates. The **confluence (determinism)
guarantee scopes to the Rules region** — the subgraph of **Rules Processors** (written in
DIGS, hence platform-agnostic and deterministic) bounded by boundary Records — **not** to the
whole graph (ADR-0010, narrowed). Within that region determinism is structural, not
conventional, and rests on two rules:
**single-assignment purity** (no shared mutable place — a shared resource like the PRNG is a
*threaded chain* of distinct values, `prng_v0 → … → prng_vN`) and **partitioning guards**
(every dispatch's guard set is mutually exclusive *and* exhaustive over the color space).
Given both, one tick of the Rules region is a DAG (the cross-tick loop is the `state:`
Records), incomparable Processors commute, and **execution order is derived, never authored**
— any topological order yields identical results, so a runtime may parallelize freely. The
**Realization region** beyond the boundary Records is the **same petri-net topology** but
is **exempt** from confluence: Realization Processors are platform-tailored I/O and trade
determinism for hardware reach.

**The Processor is BlooP; the Network is FlooP** (ADR-0017). Each Processor is a bounded, total
piece (one tick = a DAG). The **only** unbounded loop is the **tick-feedback** through the
`state:` Records, and it lives in the *wiring*, never in a body — so it is **visible in the
topology** (the **I** in DIGS). Those cross-tick Records carry unboundedness in **both time**
(the game runs forever) **and space** (a `state:` list may grow each tick — Factorio's factory,
a roguelike's accumulating world). All of it stays **deterministic and in the Rules region**:
unbounded *deterministic* live state belongs here, *not* in Realization — pushing it across the
boundary would put the most deterministic part of a lockstep game into the determinism-exempt
region (it is determinism at the **value** level, addresses never observed; this is how Factorio
does cross-machine lockstep). A consequence: the Network — unlike a Processor — **can hang** (a
feedback loop whose guard never fires) and **can grow a `state:` list without bound** (a
*deterministic, reproducible* leak) — both are game bugs for **tooling** to catch, not language
guarantees. True *non-deterministic* leaks live only in Realization.

"Network" is **scale-free**. The whole game is one Network; every bundled chunk is *also* a
Network (a Sub-Network when viewed from outside as a Processor). Fully unbundled it is a
single flat bipartite graph of atomic Records and Processors — there is **no privileged
"sub-network" entity sitting apart** from the whole, only **regions at different zoom
levels** (the Rules region and the Realization region are two such regions, cut apart by
boundary Records). Decomposition is always possible, but complexity explodes at nearly every
level, so all language and tooling must assume free **bundling/unbundling** into manageable
lumps as the normal way to view and edit the graph; the bundle is the unit of manageability.
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

**`runs:` namespace**:
The reserved prefix of the standard. **Reserved** (only the standard may *define* `runs:`
keys — anti-squatting) and **semantically fixed** (a `runs:` key carries exactly the
standard's keys, types, and semantics; you may not redefine it), but **optional to use** —
targeting `runs:` shapes buys interop, it is not required for compliance. **Compliance rests
on the four primitives + DIGS alone**; a game built entirely on its own umbrella prefix (e.g.
`spacewar:`) is fully compliant. Mental model: `runs:` is the **coreutils of EGS** — a
deliberately *small*, basic, predictable core set of shapes/utilities, easy to read through and
cherry-pick. Its value is in staying minimal and foundational, not comprehensive; the ecosystem
grows in **umbrella prefixes**, not by swelling `runs:`. The `runs:` palette lives in
`runs-conventions`, which is a **curation index, not a code vault**: it owns the interop *contracts* (recommended shapes +
Processor signatures), naming/governance, the manifest format, and a **blessed-by-ID index** of
reference bodies — but the bodies themselves live on the commons (Nostr), referenced by ID.
Forced by ADR-0009 (one artifact, one home) + the permissionless ethos + Nostr rendering
directly. *Today* the commons is empty and no evaluator exists, so the repo instead carries
**conceptual examples** (see Flagged ambiguities). (ADR-0014, amended)

**Umbrella prefix**:
A short, human-readable namespace label on a key (`spacewar:object`, `pong:`, `hades:`) — pure
**convenience**, never identity. Real identity is **cryptographic and two-layered**: the
**npub** (the author's Nostr public key) roots *authorship/namespace* and never collides
(SHA-256-class assurance), and the **content hash** (the Nostr event id) roots the *artifact*
(ADR-0009 content-addressing). References resolve **by ID, not by prefix string**, so two
authors both publishing `pong:object` is **harmless** — different npubs, different IDs, two
distinct schemas. This is why EGS does **not** police namespaces the way npm/Maven must: in a
package-manager world the string name *is* identity and a collision is catastrophic; here the
string is never identity, so the whole concern evaporates. The lone exception is **`runs:`**,
which *is* governed — not to prevent resolution collisions but because its *string* carries
fixed, shared interop semantics that must mean the same thing for everyone (anti-squatting +
semantic stability; see `runs:` namespace, ADR-0014).
_Avoid_: treating a prefix as a globally unique name or a resolution key; reasoning about RUNS
namespaces with package-manager (npm/Maven) intuitions — the cryptographic-identity paradigm
makes them inapplicable.

**Boundary Record**:
A Record crossing the game-logic ↔ runtime line. **Inbound** (`requires:`) are written by
the runtime before a tick; **outbound** (`produces:`) are read by the runtime after. The
boundary is **Records only — never Processors**, and necessarily so: it joins two paradigms
(deterministic DIGS ↔ platform-tailored machine), and only a noun can sit on that seam. A
Record is inert data, hence paradigm-neutral; a Processor is a verb that always carries a
paradigm, so putting one "in the boundary" forces it to pick a side — at which point it is no
longer the boundary but the first Processor of whichever layer it joined. Any needed
shape-translation is therefore a Processor on one side *up to* the boundary, never *in* it.

**Runtime**:
The generic, **game-agnostic host** that runs Builds and honors the Runtime Interface — a
browser, a console, a libretro-style core, a server farm. It exposes primitive platform
capabilities and the boundary contract: it writes **inbound** boundary Records before a tick
and reads **outbound** ones after. It is *not* game-specific — a game's I/O lives in the
**Realization region**, which is baked into the **Build** and runs *on* a Runtime. The
Realization-region ↔ Runtime line is **platform-dependent**: a fantasy console exposing
`draw_sprite` makes the Runtime fat and the Realization region thin; a raw framebuffer
reverses it.
_Avoid_: engine (there is no "RUNS engine"); using "Runtime" for the game-specific I/O (that
is the **Realization region**); conflating with **hand-compilation** (see Flagged ambiguities).

**Build**:
The playable, distributable artifact that **baking** emits — a native binary, a wasm bundle,
a JS/Canvas app, a cartridge. *Not* source: one Rules region × one Realization region × one
target → one Build. The thing players run; it carries no dependency on Nostr or relays at play
time.
_Avoid_: reading "build" as "compiled" in the literal sense — a Build is the playable version
however it was produced (compiled, interpreted-and-frozen, hand-emitted).

**Baking**:
The build-time process that turns source into a **Build**: resolve dependencies (Nostr or
local cache) → flatten the Network graph → compile the chosen Rules region + Realization
region into a self-contained artifact for one target. Always happens; nothing streams DIGS or
Nostr at play time. (The Lifecycle "Build" phase is baking.) The commons (Nostr) is touched
**only at lifecycle boundaries** — discovery/build, and AEMS load/save — **never the hot
path**, and this is *forced, not chosen*: Nostr is a **permanence layer, not a real-time
database**; it makes state durable, not ephemeral, so per-frame commons access wouldn't work
even if attempted. Baking is what bridges the permanent commons to the ephemeral tick loop.

**Port**:
The same game — *identical* Rules region — made to run on different hardware by swapping its
**Realization Processors** wholesale, leaving the Rules Processors and boundary Records
untouched. PDP-1 renderer → N64 renderer; DOOM's exact logic under an N64 shell; The Last of
Us's exact logic under a low-poly shell. A port preserves the game: it is **not** a Variant
(which changes rules) and **not** a reinterpretation (re-vibing the source "good enough,"
which yields a new variant). Porting is what makes "any rules on any hardware" real — N64, a
TI-83, a toaster, or a computing paradigm 400 years from now. (ADR-0011)
The line is drawn by **bit-exact Rules-region output**: a *pure* Port runs the **exact same
algorithms**, deterministic even where that is inefficient on the new hardware. A re-target
that swaps in **hardware-native algorithms whose output differs from the original** (the
classic case: a machine-optimized floating-point routine — Quake's FP demos desync across
hardware for exactly this reason; fixed-point DOOM does not) is **by definition a Variant, not
a port** — RUNS pins the original's arithmetic via game-defined numeric types precisely so a
Port can stay bit-exact. **This is a new distinction the industry conflates**: the PS5 and
Switch builds of DOOM 2016 are both called "ports," but with divergent arithmetic they are
**different games** by this definition. RUNS requires the split.
More generally, a **pure Port preserves all three of the Rules region's reach-contracts**
bit-for-bit — **arithmetic, execution, and memory** (ADR-0017). A target that cannot hold them
forces a tradeoff to the *Rules* Processors and so yields a **Variant Port** (a Variant made to
fit a target), not a pure Port: hardware-native FP (arithmetic), or **shrinking limits to fit
RAM** (memory) — *Chocolate DOOM on a TI-83 is impossible as a pure Port*; the smaller entity
caps are a Rules change, not a Realization swap. "Any rules on any hardware" is mechanical only
where the target holds the identical Rules region.
_Avoid_: using **port** for the *act only* — both the act and its result are a port; a
fidelity-reducing re-vibe is a **variant**, not a port; calling a divergent-arithmetic *or
reduced-memory* re-target a "port" (industry usage) — it is a **Variant Port**.

**Phase / Dispatch / Guard**:
A **Phase** is one ordered execution stage of a tick — but ordering is *derived, not authored*
(ADR-0010), so `phases:` is a demoted **crutch**, not a primitive (see `Network`). **Dispatch**
is a bounded iteration over an entity collection — a **threaded fold** (ADR-0012). "Bounded"
here means the **BlooP rule**: the count is **fixed before the loop and immutable during it**
(no `break`/`continue`/early-return, loop variable and collection not mutated mid-iteration) —
**not** that the bound is a compile-time constant or a finitely-typed value. A `range(n)` or
collection fold whose `n`/length is a runtime value, **including unbounded `int`**, is total —
it terminates for every input (cf. Dhall's `Natural/fold`, primitive recursion). A *static*
step-ceiling (WCET) is a **separate, opt-in reach-contract** (ADR-0017), never part of totality.
A **Guard**
is a DIGS boolean on a Network arc that routes to a Processor (the RUNS implementation of a
MAPS Arc condition). A guarded branch obeys **two laws** (ADR-0013):
- **Totality** (determinism) — the guards form a complete partition: exactly one true, always
  (mutually exclusive + exhaustive). A **closed-enum discriminant** makes this tool-checkable;
  an open-typed discriminant (int, string) requires an explicit **`else` arc**.
- **Thinness** (composability) — a guard *reads* a precomputed fact (a tag Field, e.g.
  `can_see_player`); it **never computes one**.
Decision structure lives in the arcs; computation and actions live in Processors. **No
Processor contains an `if` that steers the Network**, so every branch target is
state-machine-ignorant and fully composable (the perceive → route → act shape).
This law is **decomposition-relative** (see `Processor` → granularity; ADR-0016). It is
**vacuous for a monolith**: a fat undecomposed Processor may legally hold an internal
`if state == torpedo: …` dispatch, because the Network sees one opaque transition — there is
no Network branch there to steer. It becomes **binding once the routing targets are distinct
Processors**: "which Processor runs" is then a Network routing decision, and resurrecting it as
a **dispatcher Processor** (`if state == torpedo: torpedo_update() elif … ship_update()`) is
the forbidden steering — that routing must be **arc guards** instead. A body `if` that
*computes* a discriminant value (e.g. setting `state = exploding`) is **action, not steering**:
producing a tag is a Processor's job; only *reading* one to route is the guard's.

**Precondition**:
A DIGS boolean a Processor declares as **what must hold when it is invoked** (`divisor != 0`,
`object.state == spacewar:ship`). It is the **body-side dual of arc-guard totality**: the
**guard *asserts* the partition on the arc** (Network plane), the **precondition *assumes* it
in the body** (body plane) — the same fact stated on both sides of the boundary. In a
well-formed Network the guards **discharge** every precondition, so the body never enters the
violated case, and DIGS's determinism is contracted **conditionally**: *same inputs → same
outputs, given preconditions hold*. The "undefined behavior" of a violated precondition is
therefore **unreachable in a conforming game** — it lives strictly in **malformed-Network**
territory, *outside* the determinism contract, not a hole in it. There being **no RUNS engine**,
the doc's "runtime may halt, log, or substitute" are not language semantics but evidence of
**poorly-written code**; catching a violation is a **tooling** concern (validator + tests
proving the guards discharge the preconditions), never a runtime feature. (ADR-0013)

### Evolution

**Variant**:
A game derived from another by opening its Network and swapping, adding, or replacing
**Rules Processors**, then compiling anew — the way chess and soccer accrue house-rule
variants over time. A variant changes the *rules*; the realization can stay identical. Its
orthogonal twin — swapping the **Realization Processors** while the rules stay identical — is
a **Port**, not a variant. First-class and permissionless: enduring games *evolve through
variation*; they are not modified or forked. (ADR-0006, ADR-0011)
A variant arises **three ways**, distinguished only by *how* the Rules-region output changed:
**authored** (hand-edit/add/replace Rules Processors), **re-vibe** (reinterpret the source
"good enough," reducing fidelity), and **declared at build time** — a *divergent compilation*
(the doc's "adapted compilation", renamed as the honest antonym of **strict** evaluation) that
substitutes a non-bit-identical algorithm (native sqrt, hardware-native floating point)
for a strict Rules Processor, recorded in a machine-readable **deviation manifest** (the
itemized divergences). All three change the deterministic behavior, so all three are variants;
the manifest is the divergent variant's **honest diff from canon** (the build-time face of
asserted-vs-verified, ADR-0005).
This is why a divergent-arithmetic re-target is a Variant despite the industry "port" label
(see `Port`). A **Variant Port** is the special case where the *motive* is reaching constrained
hardware but the *means* is altering a Rules-region reach-contract — **divergent arithmetic**
(hardware-native FP) or a **reduced memory contract** (smaller list bounds to fit RAM: Chocolate
DOOM on a TI-83). Because the change is to the *Rules* Processors, not a Realization swap, the
result is a **Variant, not a Port** (ADR-0017).
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
An observable condition ("Airborne", "Charging").

**Verb**:
An available action ("Jump").

**Arc (MAPS)**:
A directed transition linking States through Verbs, carrying conditions/effects.

**Mark**:
A quantifiable resource (health, ammo, score).

### WOCS terms

**Offer / Fulfill / Ack**:
The three coordination events — a broadcast need with committed payment, proof of delivery,
and confirmation with instant Lightning settlement.

### Correspondence (design → source) — aspirational, not asserted

MAPS and RUNS are **two distinct levels and purposes of notating games** — design-time
notation vs. compilable source. A clean term-for-term correspondence (State→Record,
Verb→Processor, Arc→wiring, Mark→Field) is **aspirational and unproven**: no one has earned it
by actually translating a MAPS Score into RUNS source (or back). The *aspiration* is
interoperability; whether the two levels actually interoperate — especially early in the EGS
lifecycle — is an open bet. Per the asserted-vs-verified discipline (ADR-0005) the
correspondence is **not canon and not highlighted** until real MAPS↔RUNS translations exist.
A side effect: the **encoding question** a translation would force — whether a MAPS State
becomes a *color* (a discriminant Field value, per the colored Petri net of ADR-0010) or a
*place* (its own Record) — is **parked**, not resolved.

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
- **"Conceptual example" vs "reference implementation"** — _Canon (ADR-0014 amended)._ A
  **conceptual example** illustrates the *shape* of an idea for a human reader, with no
  implementation behind it (possibly wrong) — what `runs-conventions` carries today, because the
  commons is empty and no evaluator exists. A **reference implementation** is an actual body
  authored against a working DIGS evaluator, born on the commons, and blessed by ID in the
  conventions index. They are *different kinds of thing*, not maturity stages: a conceptual
  example has **no privileged claim** to become the blessed body. None of the bodies in the
  corpus today (`integrate_velocity`, the `enemy_behavior` network, `RUNS-Spacewar/`) are
  reference implementations.
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
