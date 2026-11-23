# Enduring Game Standard: Working Vocabulary

The canonical domain language of the EGS protocol family, and the single source of truth for
cross-protocol terms. Each bold entry name is the handle other repos cite. Each
definition says what a term *is*, not how it is implemented. The public quick reference (the
acronym table and the protocol roster) is `profile/README.md` § Glossary.

**Capitalize a defined term** wherever it carries its glossary meaning, in this file and in every
EGS doc: a Record, the Game Logic, a Port. Lowercase means the ordinary English word.

> Multi-context note: the workspace is a meta-repo over several independently published
> protocol repos. AEMS owns its glossary in
> [aems-schema/CONTEXT.md](https://github.com/enduring-game-standard/aems-schema/blob/main/CONTEXT.md),
> and this file only summarizes those terms. A `GLOSSARY-MAP.md` waits until the workspace
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
Game development in plain-text source, with Game Logic as a first-class part kept apart from the
Presentation, so digital games endure for millennia. Not an engine; each game compiles its own.
Three layers: data (Records and Fields), wiring (Networks), and computation (Processor bodies:
Game Logic Processors are written in DIGS, Presentation Processors in whatever their machine needs;
see `Processor`).

**DIGS** (Deterministic Inspectable Game Syntax), *the computation*:
The pure, total, deterministic expression language of RUNS. Its home is Processor **bodies**
(a `.runs-prim` file declares Processors in the full language, with statements); Network
**Guard** expressions reuse its expression grammar. A sub-language of RUNS, governed and shipped
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
  **Arithmetic is Game Logic.** Spacewar's 18-bit ones' complement is to Spacewar what the six-sided
  die is to Yahtzee, and DOOM computed with floats is not DOOM. A machine that computes a game's
  numbers differently plays a different game, the way a chess board that altered what a knight
  does would not be chess.
- **Two opt-in, type-declared reach-contracts**, each trading the targets a game can reach:
  **(1) a static execution bound** (WCET for real-time; a tooling and Presentation concern, not
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
**transport** is a Presentation concern (see `Processor`, Presentation Processor) that fills and
drains Interface Records; the RUNS kernel contains no transport.

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

**Petri net**:
The pattern the RUNS Network is built on, taken from colored Petri nets: Records hold the data,
Processors transform it, and the wiring between them states the order. RUNS takes the pattern for
what that structure shows (order in the wiring, independence where wires are absent, every loop
in the topology), and none of Petri-net vocabulary, semantics, or syntax.
_Avoid_: token, place, transition (no RUNS term is a synonym of a Petri-net term)

**Record**:
A named set of typed Fields holding the game's current values. Only a Processor changes a
Record.
_Avoid_: place, state, entity (an AEMS term), struct, component

**Field**:
A typed, named variable on a Record (`position_x`).

**Processor**:
The verb of the Network: it takes values on its inputs, computes, and gives values on its
outputs. It names no Record; the Network wires a Field to each input and output.
_Avoid_: transition, function, system, kernel; port or pin (for an input or output)

**Determinism is a separate, narrower property**, layered on by convention rather than the
protocol: a Processor whose outputs depend only on its declared inputs (pure, total,
deterministic). The enduring-games **pattern** (a `runs-library` convention, *not* a base
primitive) splits a game's Processors into two flavors by this property, named for their
*function*, not their implementation:
- **Game Logic Processor**: written in DIGS, hence platform-agnostic and
  deterministic. Change one and the result is a **Variant**
  (ADR-0011).
- **Presentation Processor**: a Processor of the Presentation, which carries the game to and
  from the machine in both directions: timing and input (inbound), rendering and audio
  (outbound), and **multiplayer transport** (remote inputs arrive as inbound Interface Records,
  exactly like local input). Platform-tailored; verb-pure but determinism-exempt; lives in the
  Presentation. Replace the whole set and the result is a **Port** (ADR-0011). The
  deterministic Game Logic is what makes lockstep and rollback netcode possible, yet it
  contains none of the transport.
  **Its body is *not* DIGS, and the standard deliberately specifies no language for it.** A
  Presentation Processor is written in whatever its target demands (a throwaway Lua or Python
  script, a CUDA or Vulkan pipeline in C++ or Rust, MIPS-bound N64 homebrew). This is the exact
  inverse of DIGS: Presentation bodies are **infinitely important but time-bound and
  hardware-bound**, where DIGS is time- and hardware-*un*bound. A portable Presentation language
  would re-import the platform coupling the boundary exists to quarantine. The Interface Records
  are the *entire* contract between the two languages. (ADR-0015)
The two flavors are joined only by Interface Records. Whatever its status in the base protocol,
this split is what lets a game endure: a game endures only if its Game Logic is pure.
**"Deterministic"**, unqualified, means the core evaluation-order determinism throughout this
glossary: identical results on one runtime, in any topological order. **Cross-machine**
lockstep also needs arithmetic determinism, which every game has because every game declares
its arithmetic (see `DIGS`). Without it floating point diverges across hardware (Quake
desyncs); with it, value-level determinism plus no observable addresses give Factorio-style
cross-platform lockstep. (ADR-0017)
_Avoid_: function, system (ECS analogy only), kernel; treating the Game Logic/Presentation split as a
base-protocol primitive (it is a `runs-library` pattern, not a schema distinction).

**Network**:
The wiring of Records to Processors: which Fields each Processor reads and writes, and the
Guards on them. It sets no order of its own; a Processor fires once the Records it reads are
ready, so the order follows from the wiring. A tick is one run of it. Data flows strictly **Record → Processor → Record**: never Record → Record (no Processor
means no verb, so no transformation) and never Processor → Processor (no Record means no data).
So **no Processor calls another**: a value one Processor hands to another is stored in a Record.
The Network draws a line
from a specific Field to an input or output of a Processor, and it alone knows both sides:
Records and Processors are **mutually opaque** (a Record knows nothing of a Processor's
internals; a Processor knows nothing of a Record's, nor where its inputs came from or where its
outputs go). A state machine is a Network of guarded arcs, never a Processor. The Network holds
the whole structure of the game: its logic and all its timing. The game advances in ticks, and
nothing computes between them; every step the original program took is a firing in the
Network. Composition is bundling (see `Sub-Network`).

A RUNS Network is a **colored Petri net**: places = Records, tokens = Field values (the
colors), transitions = Processors, guarded arcs = gates. The **confluence (determinism)
guarantee scopes to the Game Logic**, the subgraph of Game Logic Processors bounded by boundary
Records, **not** to the whole graph (ADR-0010, narrowed). Within that region determinism is
structural, not conventional, and rests on two rules: **every change through a Processor** (a
Processor takes a value from a place and puts its result back, and the wires and Guards order
any two Processors that touch one place, so nothing races: the PRNG's values
`prng_v0 → … → prng_vN` pass through one place in wired order) and **partitioning Guards**
(every Guard set is a complete partition; see `Guard`). Given both, the
firings of one tick of the Game Logic form a DAG apart from bounded loops (below; the
cross-tick loop is the `state:` Records), incomparable Processors commute, and **the wires and
Guards are the order**. Where order changes a result, a wire or a Guard makes one Processor
wait for the other, like a sync in SpaceChem. Processors the wiring leaves unordered are ones
whose order changes nothing, so any order the wires allow gives identical results, and a
runtime may run them in parallel. The **Presentation** beyond the Interface Records has
the **same Petri-net topology** but is **exempt** from confluence: Presentation Processors are
platform-tailored I/O and trade determinism for hardware reach.

**The Processor is BlooP; the Network is FlooP** (ADR-0017). Each Processor is a bounded, total
piece (one tick = a DAG, apart from bounded loops). A loop is **bounded** when its count is
fixed before it starts and cannot change during it: no `break`, `continue` or early return. The
count may be any value known at run time, so a loop over a collection of any length is bounded. **Loops within a tick.** Every loop within a
tick runs through a counter Record set before the loop starts: each pass consumes one token,
and the loop ends when the counter reaches zero or a Guard stops it. This is a rule for the
whole graph, and a loop need not sit in a bundle of its own (owner, 2026-11-14). The **only**
unbounded loop is the **tick feedback** through the `state:` Records. It lives in the *wiring*,
never in a body, so it is **visible in the topology** (the **I** in DIGS). Those cross-tick
Records carry unboundedness in **both time** (the game runs forever) **and space** (a `state:`
list may grow each tick: Factorio's factory, a roguelike's accumulating world). All of it stays
**deterministic and in the Game Logic**: unbounded *deterministic* live state belongs here,
*not* in the Presentation, because pushing it across the boundary would put the most deterministic
part of a lockstep game into the determinism-exempt region (it is determinism at the **value**
level, addresses never observed; this is how Factorio does cross-machine lockstep). A
consequence: the Network, unlike a Processor, **can hang** (a feedback loop whose Guard never
fires) and **can grow a `state:` list without bound** (a *deterministic, reproducible* leak).
Both are game bugs for **tooling** to catch, not language guarantees. True
*non-deterministic* leaks live only in the Presentation.

"Network" is **scale-free**. The whole game is one Network; every bundled chunk is *also* a
Network (a Sub-Network when viewed from outside as a Processor). Fully unbundled it is a single
flat bipartite graph of atomic Records and Processors. There is **no privileged "sub-network"
entity sitting apart** from the whole, only **regions at different zoom levels** (the Game Logic
region and the Presentation are two such regions, cut apart by Interface Records).
Decomposition is always possible, but complexity explodes at nearly every level, so all
language and tooling assume free **bundling and unbundling** into manageable lumps as the normal
way to view and edit the graph; the bundle is the unit of manageability.
_Avoid_: execution order; step for a tick (a Petri net step is one set of firings, many to a
tick); treating `phases:`/`order:` as primitives (the wires and Guards state the order)

**Sub-Network**:
A Network used as a Processor from outside: typed inputs and outputs at its boundary, its own
wiring inside, in its own Network file. A **bundle** is a Sub-Network, and bundling is how RUNS
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
DIGS alone**; a game built entirely on its own Umbrella Prefix (`spacewar:`) is fully
compliant. Mental model: `runs:` is the **coreutils of EGS**, a deliberately *small*, basic,
predictable core set of shapes and utilities, easy to read through and cherry-pick. Its value is
in staying minimal and foundational; the ecosystem grows in **Umbrella Prefixes**, not by
swelling `runs:`. The `runs:` palette lives in `runs-library`, which is a **curation index, not
a code vault**: it owns the interop *contracts* (recommended shapes and Processor signatures),
naming and governance, the manifest format, and a **blessed-by-ID index** of reference bodies.
The bodies themselves live on the commons (Nostr), referenced by ID. Forced by ADR-0009 (one
artifact, one home), the permissionless ethos, and Nostr rendering directly. *Today* the commons
is empty and no evaluator exists, so the repo carries **conceptual examples** instead (see
Flagged ambiguities). (ADR-0014, amended)

**Umbrella Prefix**:
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

**Game Logic**:
The part of a game that decides what happens in it, kept apart from the Presentation so the
game outlasts every machine it runs on. It is pure, written in DIGS, and works on any machine. A
Variant changes it; a Port keeps it exactly.
_Avoid_: rules, Rules region, Rules Processor

**Interface Record**:
A Record crossing the line between Game Logic and Presentation. **Inbound** (`requires:`) are
written by the Presentation before a tick; **outbound** (`produces:`) are read by the
Presentation after. The
boundary is **Records only, never Processors**, and necessarily so: it joins two paradigms
(deterministic DIGS and the platform-tailored machine), and only a noun can sit on that seam. A
Record is inert data, hence paradigm-neutral; a Processor is a verb that always carries a
paradigm, so putting one "in the boundary" forces it to pick a side, at which point it is no
longer the boundary but the first Processor of whichever layer it joined. Any needed
shape-translation is therefore a Processor on one side *up to* the boundary, never *in* it.

**Presentation**:
How a game reaches a player on one machine: the Processors and Records that read the machine's
inputs and drive its screen, its sound and its pacing. With the Game Logic and the Interface
Records it makes up the whole game; everything that is neither is Presentation. A Conversion
keeps the Original's Presentation whole, so its code records how the Original looked, sounded,
played and was paced, down to its tick rate. A Port replaces it and reads it to decide what to
keep.
_Avoid_: Realization, Realization region; "presentation" for the output half only; shell,
wrapper (it is not the lesser part)

**Kind**:
The side a Record or Processor declares: Game Logic, Interface, or Presentation. A convention:
from the Separated stage on, every Record and Processor declares one, and a check confirms they
agree, so no Game Logic Processor touches a Presentation Record. The protocol ignores Kinds.
_Avoid_: deriving a Processor's side from the Records it touches; type (a Field has a type)

**Declarations**:
The part of a machine's platform code that never runs: where the original program kept its
data and how its routines were called. They record the original programmer's choices about the
machine, nothing the game computes. A Port leaves them behind with the Presentation.

**Conversion**:
An existing game's original program rewritten into RUNS through every stage of the Conversion
Process, with the game exactly the same. A program partway along is *in conversion*, at its
stage.
_Avoid_: "Conversion" for a program short of Standard; Port (new Presentation); Variant (changed
Game Logic)

**Conversion Process**:
The route from an Original to a Conversion. Each stage has its own check:
- **Original**: the binary, identified by its hash.
- **Lifted**: readable code that rebuilds the binary.
- **Rebuilt**: valid RUNS source that rebuilds the binary.
- **Separated**: Game Logic split from Presentation; the Game Logic run alone still matches the
  Original.
- **Standard**: the source complies with the RUNS conventions, so its Records and Processors can
  be shared and compared.
_Avoid_: level (a game term), rung, phase

**Original**:
The binary of a game as it shipped: the exact image its machine loads, identified by its hash.
A game's GitHub repo keeps the binary's source, the toolchain that builds it, and its hash in
place of the binary; it rebuilds the binary from them and verifies it by the hash (owner,
2026-11-15: "it doesn't make sense to keep the binary in a GitHub repo for so many reasons. but
a hash of the binary is a fine shortcut"). On its original machine it has everything,
uninterpreted. Each shipped version is its own Original: Ocarina of Time
1.0 keeps Glitches that later versions fixed. A game born in RUNS has no binary original; its
RUNS source is the Original.
_Avoid_: "the original" for the source listing or for what the designer meant.

**Glitch**:
Gameplay the designer did not foresee, emerging from the Game Logic. A Glitch is **not Game
Logic** of its own. In a digital game the machine fills every gap in the Game Logic, the same way
every time, so the Game Logic is complete whether meant or not: the original program running on
its original machine. The Game Logic
are the **boundary of the possibility space** (every state reachable from power-on by any input
sequence, and how each moves to the next); gameplay is the paths taken inside it, Glitches
included. "Intended" and "unintended" describe the designer's map, not the space. Super Mario
64's backwards long jump emerges from an uncapped backward speed. A glitchless speedrun category
is a constraint players add on top; the game is unchanged. Removing a Glitch from the game itself
writes different Game Logic into the gap: a **Variant**.
**Sorting at the machine's edge.** A behavior is sorted by following its data across the
**original machine's edge**, never by intent. **In**: whatever the program reads from a device
(controller, switches, clocks, video-frame counts) and memory the program never set (power-on
contents). **Out**: the screen and the speaker. Inside the edge, a value that only ever flows
out is **Presentation**; everything else is **Game Logic**, including how the program's own
operations behave on the hardware: arithmetic, memory past an array, code the program writes
for itself. Code the program never contained, such as bytes the player writes and the machine
then runs, is the machine's behavior, not the game's. Spacewar's
toroidal wrap falls out of its ones'-complement coordinate arithmetic. Boom fixed vanilla
DOOM's overflows and broke vanilla demos; PrBoom added compatibility levels; Chocolate Doom
emulates the overflows to keep vanilla demos in sync. **Lag** reaches the Game Logic, if at all, as a
clock reading: an input. On the original machine that input carries the lag; a Port feeds it
different values; the Game Logic is the same and the gameplay differs. "Better" and "worse" describe
gameplay, not the game (GoldenEye's lookdown; Ocarina of Time on the N64 and on the Wii Virtual
Console: one game, separate leaderboards). Space Invaders moves one invader per video interrupt,
an input, by its Game Logic; the speed-up as invaders die emerges from them, and it is not lag. An
effect that only flows out stays in the reference with the whole original Presentation, and a
Port changes it.
So a Conversion states the Game Logic completely, and Glitches emerge on any machine, found or not.
Completeness is judged on the Game Logic, never by a Glitch list. For a game born in RUNS, the RUNS
semantics are the machine.
**Open**: what counts as the machine: which revision, and where specified behavior ends and
physical fault begins (a flipped bit is a fault; a documented CPU quirk is behavior). It is a
judgment made once per machine, in that machine's description. Also open: DOOM's
visplane-overflow crash is Presentation, and the reference keeps it, but it ends the program and
so stops the Game Logic too; how RUNS states a Presentation effect that ends the program is not
settled.
_Avoid_: "recreate a glitch", "purge a glitch"; sorting by intended vs unintended; bug (for the
original's behavior).

**Port**:
The same Game Logic, exactly, under a new Presentation for another machine. How much of the
Original's Presentation it keeps (its look, its sound, its controls, its tick rate) is the
porter's choice, and the Original's Presentation is the reference for each choice: DOOM run at
70 tics a second is still a Port, only a less faithful one. Identical Game Logic is what makes
a program a Port; no label or side document does.
_Avoid_: Port for anything that changes the Game Logic, even to fit a smaller machine (it is a
Variant); pure Port, Variant Port

**Guard**:
A DIGS condition on a Network arc that decides whether its Processor fires. The Guards on one
Record obey two laws. Exactly one of them is true, always: a closed enum makes this checkable,
and otherwise an `else` arc closes the set. And a Guard only reads a fact a Processor already
computed; it never computes one. So the decisions live in the arcs and the work lives in the
Processors: no Processor holds an `if` that picks which Processor runs next.
_Avoid_: Phase (the wiring sets the order); Dispatch for a loop (dispatch means choosing which
code runs); dispatcher Processor

**Precondition**:
A DIGS boolean a Processor declares as **what must hold when it is invoked** (`divisor != 0`,
`object.state == spacewar:ship`). It is the **body-side dual of arc-Guard totality**: the
**Guard *asserts* the partition on the arc** (Network plane), and the **Precondition *assumes*
it in the body** (body plane), the same fact stated on both sides of the boundary. In a
well-formed Network the Guards **discharge** every Precondition, so the body never enters the
violated case, and DIGS's determinism is contracted **conditionally**: *same inputs → same
outputs, given the Preconditions hold*. The undefined behavior of a violated Precondition is
therefore **unreachable in a conforming game**: it lives strictly in **malformed-Network**
territory, *outside* the determinism contract, not a hole in it. There is **no RUNS engine**, so
"the runtime may halt, log, or substitute" is not language semantics but evidence of **poorly
written code**; catching a violation is a **tooling** concern (a validator, and tests proving
the Guards discharge the Preconditions), never a runtime feature. (ADR-0013)

### Evolution

**Variant**:
A game whose Game Logic differs from the game it came from: Processors swapped, added or
removed in its Network, the way chess and soccer grow house-rule Variants over time. Removing a
Glitch makes a Variant, and so does changing the arithmetic or a limit to fit a smaller
machine. Changed Game Logic is what makes a program a Variant, whatever the reason; no label or
side document does. Variants are first-class and need no one's permission.
_Avoid_: mod, modding; fork for the result; house rule; deviation manifest, divergent
compilation

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

- **"State"**: bare "state" means a Record. Qualify the others: "AEMS State" (a claimed Asset
  condition), "MAPS State" (an observable game condition), and "object state" (a Field value,
  such as Spacewar's `object_state`: ship, torpedo, exploding).
- **"Bundle"** = a RUNS Processor bundle (a Sub-Network seen from outside as one Processor)
  vs. an AEMS bundle (a signed event that carries the events it cites; draft in aems-schema
  `ANCHOR.md`). Qualify which.
- **"Arc"** = MAPS Arc (a notation primitive) vs. a Network arc (a guarded route in RUNS
  wiring). Qualify when context is unclear.
- **Compiler vs hand-compilation**: a compiler translates RUNS source mechanically. A
  **hand-compilation** is a person or AI writing target code (Lua, say) by reading the source. The earlier
  Spacewar `spacewar.p8` PICO-8 cartridge was a hand-compilation. The PDP-1 compiler in
  runs-spacewar is a compiler: it builds the original binary from the RUNS source.
- **"Conceptual example" vs "reference implementation"**, _Canon (ADR-0014 amended)._ A
  **conceptual example** illustrates the *shape* of an idea for a human reader, with no
  implementation behind it (possibly wrong): what `runs-library` carries today, because the
  commons is empty. A **reference implementation** is an actual body authored against a working
  DIGS evaluator, born on the commons, and blessed by ID in the `runs-library` index. They are
  *different kinds of thing*, not maturity stages: a conceptual example has **no privileged
  claim** to become the blessed body. None of the bodies in the corpus today
  (`integrate_velocity`, the `enemy_behavior` Network, the Spacewar Conversion) are reference
  implementations.
- **"Verified" vs "asserted"**, _Canon (ADR-0005)._ **Verified** = checked mechanically against
  an oracle. For a Conversion the oracle is the original binary, rebuilt exactly from the RUNS
  source. **Asserted** = a person or AI read the source and judged it correct. The Spacewar
  Conversion is verified by binary identity: it is the same game. That does not show its source
  is pure; purity is a separate check (see `profile/STATUS.md`).

## Example dialogue

> **Dev:** Is the Spacewar Conversion verified?
> **Maintainer:** Its binary is. The RUNS source compiles to the original PDP-1 binary, bit for
> bit, so it is the same game. That is *verified*.
> **Dev:** So the source is pure?
> **Maintainer:** Not shown yet. Purity is a separate claim, with its own check, and we keep the
> two apart.
