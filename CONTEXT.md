# Enduring Game Standard: Working Vocabulary

The canonical domain language of the EGS protocol family, and the single source of truth for
cross-protocol terms (ADR-0008). Each bold entry name is the handle other repos cite. Each
definition says what a term *is*, not how it is implemented. The public quick reference (the
acronym table and the protocol roster) is `profile/README.md` § Glossary.

> Multi-context note: the workspace is a meta-repo over several independently published
> protocol repos. AEMS owns its glossary in
> [aems-schema/CONTEXT.md](https://github.com/enduring-game-standard/aems-schema/blob/main/CONTEXT.md),
> and this file only summarizes those terms. A `CONTEXT-MAP.md` waits until the workspace
> folder structure settles (egs-workspace#5).

## Language

### Protocol family

**EGS** (Enduring Game Standard):
The umbrella standard: four core protocols (AEMS, RUNS, WOCS, MAPS) plus DIGS and the PWNS
extension, built on Nostr (the commons) and Lightning (settlement). Aimed at a 100-year
horizon, not product-market fit.

**AEMS** (Asset-Entity-Manifestation-State), *the things*:
Four signed Nostr event kinds (Entity, Manifestation, Asset, State) linked by parent
references. It stands alone and needs no other EGS protocol.

**RUNS** (Records Update on Neutral Substrate), *the game*:
The plain-text source format for game logic. Not an engine; each game compiles its own.
Three layers: data (Records and Fields), wiring (Networks), and computation (Processor bodies:
Rules Processors are written in DIGS, presentation Processors in whatever their machine needs;
see `Processor`).

**DIGS** (Deterministic Inspectable Game Syntax), *the computation*:
The pure, total, deterministic expression language of RUNS. Its home is Processor **bodies**
(a `.runs-prim` file declares Processors in the full language, with statements); Network
**guard** expressions reuse its expression grammar. A sub-language of RUNS, governed and shipped
inside RUNS (the `runs` repo), **not** a fifth protocol. It writes RUNS only; MAPS has its own
notation.
**The core, declared arithmetic, and opt-in reach-contracts** (ADR-0017; arithmetic made
mandatory 2026-11-15). Each is a separate property, and for each *the declaration IS the
contract*:
- **The core**, guaranteed on every Processor: **verb/noun purity**; **per-tick totality**
  (terminates on every input; structural, *not* a static step-ceiling); and
  **evaluation-order determinism** (precedence fixes the order; no reassociation, no FMA).
- **Declared arithmetic**, required of **every RUNS game**, a Conversion or born in RUNS: its
  number types and their operations, which pin the game's numbers bit for bit on every machine.
  **Arithmetic is a rule.** Spacewar's 18-bit ones' complement is to Spacewar what the six-sided
  die is to Yahtzee, and DOOM computed with floats is not DOOM. A machine that computes a game's
  numbers differently plays a different game. In the owner's words (2026-11-11): left to "use
  math here", each platform would choose its own algorithm, and "that would be like every single
  chess board slightly altering what a knight does... and it wouldn't be chess"; and
  (2026-11-15) "any change to the rules is a different game".
- **Two opt-in, type-declared reach-contracts**, each trading the targets a game can reach:
  **(1) a static execution bound** (WCET for real-time; a tooling and Realization concern, not
  implied by totality); **(2) a static memory footprint** (list bounds for constrained or
  no-allocator targets, not implied by purity, determinism, or totality).
_Avoid_: "the RUNS language" (RUNS is the source format; DIGS is its expression language);
calling DIGS a protocol; fusing a contract into the core ("pure ∴ deterministic"; "total ∴
statically bounded"; "pure ∴ bounded memory"); reading "pure/total/deterministic" as implying a
fixed arithmetic (the game declares that); treating arithmetic as optional.

**MAPS** (Mechanics and Play Structures), *the design knowledge*:
The design-time notation for game mechanics. **The notation is open** (2026-11-15): whether the
current draft is the right notation for games is not settled, and neither is any relation
between MAPS and RUNS beyond a general Petri-net shape.

**WOCS** (Work Offered, Claimed, Settled), *the coordination*:
The permissionless coordination and Lightning-settlement protocol for ecosystem services: a
formalized way to coordinate service provision and maintenance through Austrian-economic
incentives instead of centralized services. It is **purely optional for everything**, *an*
enduring solution, and nothing in EGS depends on it. Not yet formally grilled; this entry is a
placeholder pending its own pass.
_Note_: WOCS coordinates *who provides a service and is paid* (hosting, audits, tournaments,
multiplayer-server provision), **not** the real-time gameplay packets. In-game multiplayer
**transport** is a Realization concern (see `Processor`, Realization Processor) that fills and
drains boundary Records; the RUNS kernel contains no transport.

**PWNS** (Provenance Without Notaries or Sovereigns):
An extension for authored experiences: provenance over property, covenants without
enforcement. It separates the always-public *seal* (a signed hash showing which key signed it,
with an optional anchor showing it existed by a given time) from the author-controlled *dial*
(content openness: open, windowed, reserved). Publishing provenance never publishes the work.

### RUNS terms

**Neutral Substrate**:
The plain-text RUNS source medium itself, the "Substrate" in the name. Neutral because it binds
to no engine, vendor, renderer, or hardware generation: Records *update on* it, runtimes
compile *from* it, and it endures while platforms churn.
_Avoid_: "substrate" for the runtime or the kernel (they *consume* the substrate); Nostr (the
commons and distribution layer, a separate concern).

**Record**:
A named collection of typed Fields: a data store, and the only thing data flows through (every
handoff between Processors lands in a Record). A Record is data, not a transformer; it has no
input or output signature of its own. It **stores its value until a Processor produces a new
one; only a Processor changes a Record** (ADR-0012): a Processor takes the value out, computes,
and puts the result back, in the same place or another.
**Reuse is a spectrum**: from **common** Records `runs-library` recommends for interop
(`runs:player_location`) to **game-specific** schemas nobody else imports (`spacewar:object`).
All are published (so the source on the commons is complete and findable), content-addressed,
and referenced by ID.
_Avoid_: entity (reserved for AEMS), struct, component (ECS analogy only).

**Field**:
A typed, named value on a Record (`position_x`, `runs:transform`). The type system is open;
`runs-library` (the curation repository) recommends shared shapes but does not close the set.

**Processor**:
A **verb**: it takes values on its inputs and gives values on its outputs, holds no state of
its own, and names no Record. The Network wires a Record Field to each input and output, and a
Processor reaches another only through the Network (see `Network`). Records, Processors, and the
Network are separate files; in the owner's words (2026-11-11), "processors are literally dry
stateless individual files that are wired together with record files via a separate network
file".
The verb/noun split (a Processor transforms, a Record stores) is the **only** purity the base
protocol knows: the Petri-net transition/place distinction. **Category follows behavior, not
name** (the duck test): a Processor is whatever transforms Fields, a Record is whatever stores
them. By that test `runs:transform` reads as a Processor (it names an action), and the data
bundle it implies is a Record (`runs:player_location`). The protocol mandates **no** naming
convention and **no** flavor distinction; at the schema level there is exactly one kind of
Processor.
**"Stateless" scopes to the interface boundary.** A body may compute with any number of locals,
and a local never leaves the body. Its semantics are **single-assignment dataflow**:
imperative-looking reassignment is versioning, and loops are bounded folds (ADR-0012), never
shared mutable state.

**Granularity is a spectrum, and decomposition is convention, not compliance** (ADR-0016).
A Processor's scope ranges from a fat **monolith** (a whole DOOM function re-expressed verb for
verb in DIGS) down to an atomic primitive (`vec_x`); *both ends are spec-compliant*. A monolith
that passes every test vector is flawlessly, deterministically the game; it is merely
**undecomposed**. The convention pulls toward the small end: split a body into DRY,
commons-published, forkable smaller Processors (referenced by Nostr ID, the way an npm package
pulls in others), each splittable until atomic, then bundled as a **Sub-Network**. The **spec
mandates none of it**: no governing body on the commons enforces it, and compliance rests on the
four primitives and DIGS, never on granularity. A corollary of the duck test: **composition
follows behavior, not original intent**; `vec_3` composes `vec_x/y/z` on what they *compute*,
not on what any of them was once "meant" for. This makes ADR-0013's "no Processor steers the
Network" *decomposition-relative* (see `Phase / Dispatch / Guard`). **Open:** the shape of the
granularity (see `Sub-Network`).

**Determinism is a separate, narrower property**, layered on by convention rather than the
protocol: a Processor whose outputs depend only on its declared inputs (pure, total,
deterministic). The enduring-games **pattern** (a `runs-library` convention, *not* a base
primitive) splits a game's Processors into two flavors by this property, named for their
*function*, not their implementation:
- **Rules Processor**: the game logic. Written in DIGS, hence platform-agnostic and
  deterministic; lives in the Rules region. Change one and the result is a **Variant**
  (ADR-0011).
- **Realization Processor** (also called the **presentation**): the layer that realizes the
  game on hardware in both directions: timing and input (inbound), rendering and audio
  (outbound), and **multiplayer transport** (remote inputs arrive as inbound boundary Records,
  exactly like local input). Platform-tailored; verb-pure but determinism-exempt; lives in the
  Realization region. Replace the whole set and the result is a **Port** (ADR-0011). The
  deterministic Rules region is what makes lockstep and rollback netcode possible, yet it
  contains none of the transport.
  **Its body is *not* DIGS, and the standard deliberately specifies no language for it.** A
  Realization Processor is written in whatever its target demands (a throwaway Lua or Python
  script, a CUDA or Vulkan pipeline in C++ or Rust, MIPS-bound N64 homebrew). This is the exact
  inverse of DIGS: Realization bodies are **infinitely important but time-bound and
  hardware-bound**, where DIGS is time- and hardware-*un*bound. A portable Realization language
  would re-import the platform coupling the boundary exists to quarantine. The fat/thin Runtime
  line (see `Runtime`) decides how much Realization code a game has; the boundary Records are
  the *entire* contract between the two languages. (ADR-0015)
The two flavors are joined only by boundary Records. Whatever its status in the base protocol,
this split is what lets a game endure: "There is no endurance if the rules are impure" (owner,
2026-11-15; see `Conversion`).
**"Deterministic"**, unqualified, means the core evaluation-order determinism throughout this
glossary: identical results on one runtime, in any topological order. **Cross-machine**
lockstep also needs arithmetic determinism, which every game has because every game declares
its arithmetic (see `DIGS`). Without it floating point diverges across hardware (Quake
desyncs); with it, value-level determinism plus no observable addresses give Factorio-style
cross-platform lockstep. (ADR-0017)
_Avoid_: function, system (ECS analogy only), kernel; treating the Rules/Realization split as a
base-protocol primitive (it is a `runs-library` pattern, not a schema distinction).

**Network**:
The explicit bipartite wiring of Records to Processors that defines the execution order of one
tick. Data flows strictly **Record → Processor → Record**: never Record → Record (no Processor
means no verb, so no transformation) and never Processor → Processor (no Record means no data).
So **no Processor calls another**: a value one Processor hands to another is stored in a Record.
In the owner's words (2026-11-11): "The network is what should be connecting them, not the
processor, not the record"; and "it stores it in a record. always". The Network draws a line
from a specific Field to an input or output of a Processor, and it alone knows both sides:
Records and Processors are **mutually opaque** (a Record knows nothing of a Processor's
internals; a Processor knows nothing of a Record's, nor where its inputs came from or where its
outputs go). A state machine is a Network of guarded arcs, never a Processor. The Network holds
the whole structure of the game: its logic and all its timing. The game advances in ticks, and
nothing computes between them; every step the original program took is a firing in the
Network. Composition is bundling (see `Sub-Network`).

A RUNS Network is a **colored Petri net**: places = Records, tokens = field values (the
colors), transitions = Processors, guarded arcs = gates. The **confluence (determinism)
guarantee scopes to the Rules region**, the subgraph of Rules Processors bounded by boundary
Records, **not** to the whole graph (ADR-0010, narrowed). Within that region determinism is
structural, not conventional, and rests on two rules: **every change through a Processor** (a
Processor takes a value from a place and puts its result back, and the wires and guards order
any two Processors that touch one place, so nothing races: the PRNG's values
`prng_v0 → … → prng_vN` pass through one place in wired order) and **partitioning guards**
(every guard set is a complete partition; see `Phase / Dispatch / Guard`). Given both, the
firings of one tick of the Rules region form a DAG apart from bounded loops (below; the
cross-tick loop is the `state:` Records), incomparable Processors commute, and **the wires and
guards are the order**. Where order changes a result, a wire or a guard makes one Processor
wait for the other, like a sync in SpaceChem. Processors the wiring leaves unordered are ones
whose order changes nothing, so any order the wires allow gives identical results, and a
runtime may run them in parallel. The **Realization region** beyond the boundary Records has
the **same Petri-net topology** but is **exempt** from confluence: Realization Processors are
platform-tailored I/O and trade determinism for hardware reach.

**The Processor is BlooP; the Network is FlooP** (ADR-0017). Each Processor is a bounded, total
piece (one tick = a DAG, apart from bounded loops). **Loops within a tick.** Every loop within a
tick runs through a counter Record set before the loop starts: each pass consumes one token,
and the loop ends when the counter reaches zero or a guard stops it. This is a rule for the
whole graph, and a loop need not sit in a bundle of its own (owner, 2026-11-14). The **only**
unbounded loop is the **tick feedback** through the `state:` Records. It lives in the *wiring*,
never in a body, so it is **visible in the topology** (the **I** in DIGS). Those cross-tick
Records carry unboundedness in **both time** (the game runs forever) **and space** (a `state:`
list may grow each tick: Factorio's factory, a roguelike's accumulating world). All of it stays
**deterministic and in the Rules region**: unbounded *deterministic* live state belongs here,
*not* in Realization, because pushing it across the boundary would put the most deterministic
part of a lockstep game into the determinism-exempt region (it is determinism at the **value**
level, addresses never observed; this is how Factorio does cross-machine lockstep). A
consequence: the Network, unlike a Processor, **can hang** (a feedback loop whose guard never
fires) and **can grow a `state:` list without bound** (a *deterministic, reproducible* leak).
Both are game bugs for **tooling** to catch, not language guarantees. True
*non-deterministic* leaks live only in Realization.

"Network" is **scale-free**. The whole game is one Network; every bundled chunk is *also* a
Network (a Sub-Network when viewed from outside as a Processor). Fully unbundled it is a single
flat bipartite graph of atomic Records and Processors. There is **no privileged "sub-network"
entity sitting apart** from the whole, only **regions at different zoom levels** (the Rules
region and the Realization region are two such regions, cut apart by boundary Records).
Decomposition is always possible, but complexity explodes at nearly every level, so all
language and tooling assume free **bundling and unbundling** into manageable lumps as the normal
way to view and edit the graph; the bundle is the unit of manageability.
_Avoid_: treating `phases:`/`order:` as primitives; they are a crutch for resources that should
be threaded. The wires and guards state the order (ADR-0010).

**Sub-Network**:
A Network used as a Processor from outside: typed inputs and outputs at its boundary, its own
wiring inside, in its own network file. A **bundle** is a Sub-Network, and bundling is how RUNS
composes: a chunk of Record–Processor–Record logic bundled into a larger function (`vec_x`,
`vec_y`, `vec_z` → `vec_3`). In the owner's words (2026-11-11): "the high level network file
should only be writing the fully composed bundles. within each bundle will be its own subnet
bundle network file - that's the entire point". A bundle is never severed from its parts: their
provenance endures inside it (the bundle's content references them by ID). The popular unit is
usually the bundle; the smallest Processors inside stay addressable. Games compose from a finite
set of shared atomic primitives the way a Linux distro composes from coreutils.
Bundles exist so stateless logic can be shared between games by description. A top bundle is
named and described ("Spacewar's gravity"); someone finds it, wires it into another game's
Records, and edits their own copy. What makes a bundle unique is its wiring, not the
Processors inside, which are shared bundles down to the smallest. A bundle whose inputs fit only
one game's Records cannot be shared, and that is a problem to solve. **Open:** the shape of the
granularity: how far down Processors split and how bundles nest.

**`runs:` namespace**:
The reserved prefix of the standard. **Reserved** (only the standard may *define* `runs:` keys:
anti-squatting) and **semantically fixed** (a `runs:` key carries exactly the standard's keys,
types, and semantics; nobody may redefine it), but **optional to use**: targeting `runs:` shapes
buys interop and is not required for compliance. **Compliance rests on the four primitives and
DIGS alone**; a game built entirely on its own umbrella prefix (`spacewar:`) is fully
compliant. Mental model: `runs:` is the **coreutils of EGS**, a deliberately *small*, basic,
predictable core set of shapes and utilities, easy to read through and cherry-pick. Its value is
in staying minimal and foundational; the ecosystem grows in **umbrella prefixes**, not by
swelling `runs:`. The `runs:` palette lives in `runs-library`, which is a **curation index, not
a code vault**: it owns the interop *contracts* (recommended shapes and Processor signatures),
naming and governance, the manifest format, and a **blessed-by-ID index** of reference bodies.
The bodies themselves live on the commons (Nostr), referenced by ID. Forced by ADR-0009 (one
artifact, one home), the permissionless ethos, and Nostr rendering directly. *Today* the commons
is empty and no evaluator exists, so the repo carries **conceptual examples** instead (see
Flagged ambiguities). (ADR-0014, amended)

**Umbrella prefix**:
A short, human-readable namespace label on a key (`spacewar:object`, `pong:`, `hades:`): pure
**convenience**, never identity. Real identity is **cryptographic and two-layered**: the
**npub** (the author's Nostr public key) roots *authorship and namespace* and never collides
(SHA-256-class assurance), and the **content hash** (the Nostr event id) roots the *artifact*
(ADR-0009 content-addressing). References resolve **by ID, not by prefix string**, so two
authors both publishing `pong:object` is **harmless**: different npubs, different IDs, two
distinct schemas. In a package-manager world the string name *is* identity and a collision is
catastrophic; here the string is never identity, so EGS does **not** police namespaces. The lone
exception is **`runs:`**, which *is* governed, not to prevent resolution collisions but because
its *string* carries fixed, shared interop semantics that must mean the same thing for everyone
(anti-squatting and semantic stability; see `runs:` namespace, ADR-0014).
_Avoid_: treating a prefix as a globally unique name or a resolution key; reasoning about RUNS
namespaces with package-manager (npm, Maven) intuitions.

**Boundary Record**:
A Record crossing the line between game logic and runtime. **Inbound** (`requires:`) are written
by the runtime before a tick; **outbound** (`produces:`) are read by the runtime after. The
boundary is **Records only, never Processors**, and necessarily so: it joins two paradigms
(deterministic DIGS and the platform-tailored machine), and only a noun can sit on that seam. A
Record is inert data, hence paradigm-neutral; a Processor is a verb that always carries a
paradigm, so putting one "in the boundary" forces it to pick a side, at which point it is no
longer the boundary but the first Processor of whichever layer it joined. Any needed
shape-translation is therefore a Processor on one side *up to* the boundary, never *in* it.

**Runtime**:
The generic, **game-agnostic host** that runs Builds and honors the Runtime Interface: a
browser, a console, a libretro-style core, a server farm. It exposes primitive platform
capabilities and the boundary contract: it writes **inbound** boundary Records before a tick
and reads **outbound** ones after. A game's I/O lives in the **Realization region**, which is
baked into the **Build** and runs *on* a Runtime. The line between the Realization region and
the Runtime is **platform-dependent**: a fantasy console exposing `draw_sprite` makes the
Runtime fat and the Realization region thin; a raw framebuffer reverses it.
_Avoid_: engine (there is no "RUNS engine"); "Runtime" for the game-specific I/O (that is the
**Realization region**); conflating it with **hand-compilation** (see Flagged ambiguities).

**Build**:
The playable, distributable artifact that **baking** emits: a native binary, a wasm bundle, a
JS/Canvas app, a cartridge. *Not* source: one Rules region × one Realization region × one target
→ one Build. The thing players run; it carries no dependency on Nostr or relays at play time.
_Avoid_: reading "build" as "compiled" in the literal sense; a Build is the playable version
however it was produced (compiled, interpreted and frozen, hand-emitted).

**Baking**:
The build-time process that turns source into a **Build**: resolve dependencies (Nostr or a
local cache), flatten the Network graph, and compile the chosen Rules region and Realization
region into a self-contained artifact for one target. It always happens; nothing streams DIGS
or Nostr at play time. (The Lifecycle "Build" phase is baking.) The commons (Nostr) is touched
**only at lifecycle boundaries** (discovery and build, AEMS load and save), **never on the hot
path**, and this is *forced, not chosen*: Nostr is a **publication layer, not a real-time
database**. It carries signed events between sessions, not per-frame state. Baking bridges the
commons to the ephemeral tick loop.

**Declarations**:
The part of a machine's platform code that never runs: where the original program kept its
data and how its routines were called. They record the original programmer's choices about the
machine, nothing the game computes. A Port leaves them behind with the presentation.

**Compiler**:
The program that turns RUNS source into a Build for one machine. There is one general compiler
per machine, shared by every game on that machine. It holds that machine's details (registers,
addresses, scratch words, the instructions that move a word) and no game's logic or timing,
which live in the Network. A change to it is a general rule for the machine that a second
program on that machine uses, never a fix to make one game's shape compile. In the owner's
words: "there should be no changes at all to the compiler... Everything should be in how the
network is structured" (2026-11-11, meaning no bending it for one game), "the compiler being a
general pdp1 compiler" (2026-11-11), and "that's the job for the compiler to make sure that it
runs on the PDP-1" (2026-11-14).

**Conversion**:
Rewriting an existing game's original program into RUNS while the game stays exactly the same:
the **reference** later Ports start from. It is neither a Port (which strips the presentation,
the Realization region, and puts in new presentation Records and Processors for another
machine) nor a Variant (which changes the rules). A conversion keeps the rules pure and separate
from the presentation, *and keeps the whole original presentation*: compiled for the original
machine it is the original game, and run headless its rules run alone on any machine.
**Rule or machine.** The rules include everything that changes what happens in the game: the
arithmetic (Spacewar's 18-bit ones' complement is Spacewar's six-sided die), the limits, the
order wherever order changes a result, and what the original machine does the same way every
time (memory past an array, code the program runs, timing the program produces), because the
machine fills every gap in a digital game's rules (see `Glitch`). The test is asked of the
rules, never decided by intent: **intent is not the rules**. What changes nothing (registers,
addresses, scratch words) belongs to the original machine: its declarations and its compiler. A
conversion neither recreates nor removes glitches; they emerge from the complete rules.
**The oracle is the binary**: the RUNS source, compiled for the original machine, rebuilds the
original binary exactly. Comparing play only proves the inputs tested; the identical binary
proves every input, presentation included, which is why the oracle is not escapable. The binary
is the **Original** and the RUNS source is its **Edition** (see `Edition`). The reference
archive keeps the original's source, its toolchain, and its hash (see `Original`), the edition,
and the edition's exclusion list.
The conversion is the artifact meant to go into the archive untouched; the work is the
**process**, documented so it repeats for every game that exists (and, for games born in RUNS,
a process for building them from scratch), not a PDP-1 procedure.
Settled so far: assemble the original into the binary oracle, lift it to readable C, and build
a compiler that turns the source back into the same binary. **Open:** how to go from the
readable code to a pure Network. The owner's intent: each deep module first becomes one large
Processor that shows the shape of the tick, then is refactored into game-logic groupings,
separate from the presentation, as automatically as possible.

**Original**:
The binary of a game as it shipped: the exact image its machine loads, identified by its hash.
A game's GitHub repo keeps the binary's source, the toolchain that builds it, and its hash in
place of the binary; it rebuilds the binary from them and verifies it by the hash (owner,
2026-11-15: "it doesn't make sense to keep the binary in a GitHub repo for so many reasons. but
a hash of the binary is a fine shortcut"). On its original machine it has everything,
uninterpreted. Each shipped version is its own original with its own Edition: Ocarina of Time
1.0 keeps glitches that later versions fixed. A game born in RUNS has no binary original; its
RUNS source is the original.
_Avoid_: "the original" for the source listing or for what the designer meant.

**Edition**:
The RUNS source of a Conversion, read against its Original (owner-approved, 2026-11-15). The
rules for it:
(1) The binary is the original, identified by its hash; the archive keeps what rebuilds and
verifies it (see `Original`). (2) The RUNS source is an edition of
it and **carries everything by default**: every behavior of the binary is in the rules, and
glitches are never purged. (3) A departure is a stated **Exclusion**, never a silence. (4) Every
exclusion carries its evidence and who decided it; intent never decides silently, and a later
editor may disagree and make another edition while the original stays as it is. (5) **The
check**: the rules run alone match the original everywhere except the listed exclusions, the
only places they may differ. (6) A game born in RUNS has no exclusions: it has no binary, and
its RUNS source is the original.
Compiled for the original machine, an edition still rebuilds the original binary, the oracle;
the exclusions bound only what the rules run alone are held to.
Why: inside one deterministic system every line is drawn by whoever looks, and intent cannot be
read from the binary. A line by "who writes the program" fails: Screeps and Baba Is You make
writing code and rewriting rules the game. Textual scholarship met the same problem (an author's
intent against errors in transmission) and settled not the intent question but how to handle the
judgment: keep the original witness untouched, and make each editorial judgment explicit, with
its evidence, attributed, and open to revision.
_Avoid_: "conversion with fixes"; calling an exclusion a fix, a purge, or a Variant.

**Exclusion**:
A stated departure of an Edition from its Original. It names the **exit point**, the state and
the step where play leaves the edition; the behavior past it is the original's alone. It carries
its evidence (developer statements, the manual, speedrun categories, sequels, port communities'
choices) and who decided it. The standard example is code the player writes and the program
then runs. Ocarina of Time's arbitrary code execution leaves the edition at the step where
control jumps into bytes the player chose; Zelda Wiki says it is "commonly restricted or placed
in its own category", and TASBot's talk is listed as "How to hack an N64 through the controller
ports". Code the program fixes for itself is no exclusion: Spacewar's outline code, which the
program writes from its own tables, and a jump into the program's own bytes at the wrong entry
are the original's behavior, and the edition carries them. DOOM's candidates are the intercepts
overflow (Chocolate Doom's users advise leaving its emulation off unless a demo needs it) and any
code execution. RUNS has no form for an exit point yet.
_Avoid_: exception, waiver, "purge".

**Exclusion list**:
An Edition's exclusions, each with its exit point, its evidence, and who decided it. An empty
list is a claim with evidence too: Spacewar's is empty (runs-spacewar `docs/exclusions.md`).

**Glitch**:
Gameplay the designer did not foresee, emerging from the rules. A glitch is **not a rule**. In a
digital game the machine fills every gap in the rules, the same way every time, so the rules are
complete whether meant or not: the original program running on its original machine. The rules
are the **boundary of the possibility space** (every state reachable from power-on by any input
sequence, and how each moves to the next); gameplay is the paths taken inside it, glitches
included. "Intended" and "unintended" describe the designer's map, not the space. Super Mario
64's backwards long jump emerges from an uncapped backward speed. A glitchless speedrun category
is a rule players add on top; the game is unchanged. Removing a glitch from the game itself
writes a different rule into the gap: a **Variant**. An Exclusion removes nothing: it states
where play leaves the Edition.
**Sorting at the machine's edge.** A behavior is sorted by following its data across the
**original machine's edge**, never by intent. **In**: whatever the program reads from a device
(controller, switches, clocks, video-frame counts) and memory the program never set (power-on
contents). **Out**: the screen and the speaker. Inside the edge, a value that only ever flows
out is **presentation**; everything else is the **rules**, including what the hardware does the
same way every time: arithmetic, memory past an array, code the program runs. Spacewar's
toroidal wrap falls out of its ones'-complement coordinate arithmetic. Boom fixed vanilla
DOOM's overflows and broke vanilla demos; PrBoom added compatibility levels; Chocolate Doom
emulates the overflows to keep vanilla demos in sync. **Lag** reaches the rules, if at all, as a
clock reading: an input. On the original machine that input carries the lag; a Port feeds it
different values; the rules are the same and the gameplay differs. "Better" and "worse" describe
gameplay, not the game (GoldenEye's lookdown; Ocarina of Time on the N64 and on the Wii Virtual
Console: one game, separate leaderboards). Space Invaders moves one invader per video interrupt,
an input, by a rule; the speed-up as invaders die emerges from them, and it is not lag. An
effect that only flows out stays in the reference with the whole original presentation, and a
Port changes it.
So a Conversion states the rules completely, and glitches emerge on any machine, found or not.
Completeness is judged on the rules, never by a glitch list. For a game born in RUNS, the RUNS
semantics are the machine.
**Open**: what counts as the machine: which revision, and where specified behavior ends and
physical fault begins (a flipped bit is a fault; a documented CPU quirk is behavior). It is a
judgment made once per machine, in that machine's description. Also open: DOOM's
visplane-overflow crash is presentation, and the reference keeps it, but it ends the program and
so stops the rules too; how RUNS states a presentation effect that ends the program is not
settled.
_Avoid_: "recreate a glitch", "purge a glitch"; sorting by intended vs unintended; bug (for the
original's behavior).

**Port**:
The same game, with an *identical* Rules region, made to run on different hardware by swapping
its **Realization Processors** wholesale and leaving the Rules Processors and boundary Records
untouched. PDP-1 renderer → N64 renderer; DOOM's exact logic under an N64 shell; The Last of
Us's exact logic under a low-poly shell. A port preserves the game: it is **not** a Variant
(which changes rules) and **not** a reinterpretation (re-vibing the source "good enough", which
yields a new variant). Porting is what makes "any rules on any hardware" real: N64, a TI-83, a
toaster, or a computing paradigm 400 years from now. (ADR-0011)
A Port keeps every rule, the hardware's same-every-time behavior included, and changes what
only flows out. Its inputs are its own: where they differ, as a clock that carries less lag, the
same rules take different paths (see `Glitch`).
The line is drawn by **bit-exact Rules-region output**: a *pure* Port runs the **exact same
algorithms**, deterministic even where that is inefficient on the new hardware. A **pure Port
preserves the Rules region's declared arithmetic and any reach-contracts it takes up** bit for
bit: **arithmetic, execution, and memory** (ADR-0017); RUNS pins the original's arithmetic
through game-defined numeric types precisely so a Port can stay bit-exact. A re-target that
swaps in **hardware-native algorithms whose output differs from the original** (the classic case
is a machine-optimized floating-point routine: Quake's floating-point demos desync across
hardware for exactly this reason, and fixed-point DOOM's do not), or that otherwise cannot hold
them, changes the Rules Processors and so is a **Variant Port**, not a pure Port (see
`Variant`). **The industry conflates this distinction**: the PS5 and Switch builds of DOOM 2016
are both called "ports", but with divergent arithmetic they are **different games** by this
definition. RUNS requires the split. "Any rules on any hardware" is mechanical only where the
target holds the identical Rules region.
_Avoid_: **port** for the *act only* (both the act and its result are a port); calling a
fidelity-reducing re-vibe a port (it is a **variant**); calling a divergent-arithmetic *or
reduced-memory* re-target a "port" (industry usage; it is a **Variant Port**).

**Phase / Dispatch / Guard**:
A **Phase** is one ordered execution stage of a tick. The wires and guards state the order
(ADR-0010), so `phases:` is a demoted **crutch**, not a primitive (see `Network`).
A **Dispatch** is a bounded iteration over an entity collection, a **threaded fold** (ADR-0012);
in the Network it is a loop through a counter that starts at the count (see `Network`, loops).
"Bounded" here means the **BlooP rule**: the count is **fixed before the loop and immutable
during it** (no `break`, `continue`, or early return; the loop variable and the collection stay
unchanged mid-iteration). It does **not** mean the bound is a compile-time constant or a
finitely typed value. A `range(n)` or collection fold whose `n` or length is a runtime value,
**unbounded `int` included**, is total: it terminates for every input (compare Dhall's
`Natural/fold`, primitive recursion). A *static* step-ceiling (WCET) is a **separate, opt-in
reach-contract** (ADR-0017), never part of totality.
A **Guard** is a DIGS boolean on a Network arc that routes to a Processor. A guarded branch
obeys **two laws** (ADR-0013):
- **Totality** (determinism): the guards form a complete partition, exactly one true, always
  (mutually exclusive and exhaustive). A **closed-enum discriminant** makes this tool-checkable;
  an open-typed discriminant (int, string) requires an explicit **`else` arc**.
- **Thinness** (composability): a guard *reads* a precomputed fact (a tag Field, such as
  `can_see_player`); it **never computes one**.
Decision structure lives in the arcs; computation and actions live in Processors. **No
Processor contains an `if` that steers the Network**, so every branch target is
state-machine-ignorant and fully composable (the perceive → route → act shape).
This law is **decomposition-relative** (see `Processor`, granularity; ADR-0016). It is
**vacuous for a monolith**: a fat undecomposed Processor may legally hold an internal
`if state == torpedo: …` dispatch, because the Network sees one opaque transition and has no
branch there to steer. It **binds once the routing targets are distinct Processors**: "which
Processor runs" is then a Network routing decision, and resurrecting it as a **dispatcher
Processor** (`if state == torpedo: torpedo_update() elif … ship_update()`) is the forbidden
steering; that routing must be **arc guards** instead. A body `if` that *computes* a
discriminant value (setting `state = exploding`) is **action, not steering**: producing a tag is
a Processor's job; only *reading* one to route is the guard's.

**Precondition**:
A DIGS boolean a Processor declares as **what must hold when it is invoked** (`divisor != 0`,
`object.state == spacewar:ship`). It is the **body-side dual of arc-guard totality**: the
**guard *asserts* the partition on the arc** (Network plane), and the **precondition *assumes*
it in the body** (body plane), the same fact stated on both sides of the boundary. In a
well-formed Network the guards **discharge** every precondition, so the body never enters the
violated case, and DIGS's determinism is contracted **conditionally**: *same inputs → same
outputs, given the preconditions hold*. The undefined behavior of a violated precondition is
therefore **unreachable in a conforming game**: it lives strictly in **malformed-Network**
territory, *outside* the determinism contract, not a hole in it. There is **no RUNS engine**, so
"the runtime may halt, log, or substitute" is not language semantics but evidence of **poorly
written code**; catching a violation is a **tooling** concern (a validator, and tests proving
the guards discharge the preconditions), never a runtime feature. (ADR-0013)

### Evolution

**Variant**:
A game derived from another by opening its Network and swapping, adding, or replacing **Rules
Processors**, then compiling anew, the way chess and soccer accrue house-rule variants over
time. A variant changes the *rules*; the realization can stay identical. Removing a glitch is a
variant (see `Glitch`); an Exclusion is not (see `Exclusion`). Its orthogonal twin, swapping the
**Realization Processors** while the rules stay identical, is a **Port**. First-class and
permissionless: enduring games *evolve through variation*; they are not modified or forked.
(ADR-0006, ADR-0011)
A variant arises **three ways**, distinguished only by *how* the Rules-region output changed:
**authored** (hand-edit, add, or replace Rules Processors), **re-vibe** (reinterpret the source
"good enough", reducing fidelity), and **declared at build time**: a *divergent compilation*
(formerly "adapted compilation", renamed as the antonym of **strict** evaluation) that
substitutes a non-bit-identical algorithm (native sqrt, hardware-native floating point) for a
strict Rules Processor, recorded in a machine-readable **deviation manifest** (the itemized
divergences). All three change the deterministic behavior, so all three are variants; the
manifest is the divergent variant's **declared diff from canon** (the build-time face of
asserted vs verified, ADR-0005).
A **Variant Port** is a Variant whose *motive* is reaching constrained hardware and whose
*means* is altering the Rules region's declared arithmetic or one of its reach-contracts:
**divergent arithmetic** (hardware-native floating point) or a **reduced memory contract**
(smaller list bounds to fit RAM). *Chocolate DOOM on a TI-83 is impossible as a pure Port*: the
smaller entity caps are a Rules change, not a Realization swap. Because the change is to the
*Rules* Processors, the result is a **Variant, not a Port**, despite the industry "port" label
(see `Port`; ADR-0017).
_Avoid_: **mod / modding** (it imports a second-class, needs-the-studio's-permission framing EGS
exists to reject); **fork**, acceptable only as the *act* of starting a variant (the *result* is
a **variant**, never "a fork").

### AEMS terms

The AEMS glossary is owned by
[aems-schema/CONTEXT.md](https://github.com/enduring-game-standard/aems-schema/blob/main/CONTEXT.md).
Each entry below is a one-line summary. Where they differ, that file wins.

**Entity**:
A signed naming of a concept, ownerless (`sword`) or authored (`master-sword`).

**Manifestation**:
A game-specific version (interpretation) of one or more Entities ("Iron Sword: 6 dmg").

**Asset**:
One piece made from a Manifestation, signed by whoever made it. Who holds it is separate,
moved by a signed handoff from the current holder; the handoff's encoding and what a double
handoff means are open (aems-schema #29, #31).

**State (AEMS)**:
The claimed current condition of one Asset (charges, durability, wear). Distinct from a MAPS
State (see Flagged ambiguities).

### MAPS terms

**State (MAPS)**:
An observable condition ("Airborne", "Charging").

**Verb**:
An available action ("Jump").

**Arc (MAPS)**:
A directed transition linking States through Verbs, carrying conditions and effects.

**Mark**:
A quantifiable resource (health, ammo, score).

### WOCS terms

**Offer / Fulfill / Ack**:
The three coordination events: a broadcast need with committed payment, proof of delivery, and
confirmation with instant Lightning settlement.

## Flagged ambiguities

- **"State" is overloaded three ways.** (1) AEMS **State** = claimed Asset condition;
  (2) MAPS **State** = an observable game condition; (3) an object's runtime status (Spacewar's
  `object_state` enum: ship, torpedo, exploding, …). Always qualify: "AEMS State", "MAPS State",
  or "object state".
- **"Bundle"** = a RUNS processor bundle (a Sub-Network seen from outside as one Processor)
  vs. an AEMS bundle (a signed event that carries the events it cites; draft in aems-schema
  `ANCHOR.md`). Qualify which.
- **"Arc"** = MAPS Arc (a notation primitive) vs. a Network arc (a guarded route in RUNS
  wiring). Qualify when context is unclear.
- **"Runtime" vs "hand-compilation"**, _Canon (ADR-0005)._ A **runtime or compiler** is a
  program that mechanically parses and executes RUNS source. A **hand-compilation** is a human
  or AI manually emitting target code (Lua, say) from RUNS source by reading it. The earlier
  Spacewar `spacewar.p8` PICO-8 cartridge was a hand-compilation. The PDP-1 compiler in
  runs-spacewar is a compiler: it builds the original binary from the RUNS source.
- **"Conceptual example" vs "reference implementation"**, _Canon (ADR-0014 amended)._ A
  **conceptual example** illustrates the *shape* of an idea for a human reader, with no
  implementation behind it (possibly wrong): what `runs-library` carries today, because the
  commons is empty. A **reference implementation** is an actual body authored against a working
  DIGS evaluator, born on the commons, and blessed by ID in the `runs-library` index. They are
  *different kinds of thing*, not maturity stages: a conceptual example has **no privileged
  claim** to become the blessed body. None of the bodies in the corpus today
  (`integrate_velocity`, the `enemy_behavior` network, the Spacewar conversion) are reference
  implementations.
- **"Verified" vs "asserted"**, _Canon (ADR-0005)._ **Verified** = checked mechanically against
  an oracle. For a Conversion the oracle is the original binary, rebuilt exactly from the RUNS
  source. **Asserted** = a person or AI read the source and judged it correct. The Spacewar
  conversion is verified by binary identity: it is the same game. That does not show its source
  is pure; purity is a separate check (see `profile/STATUS.md`).

## Example dialogue

> **Dev:** Is the Spacewar conversion verified?
> **Maintainer:** Its binary is. The RUNS source compiles to the original PDP-1 binary, bit for
> bit, so it is the same game. That is *verified*.
> **Dev:** So the source is pure?
> **Maintainer:** Not shown yet. Purity is a separate claim, with its own check, and we keep the
> two apart.
