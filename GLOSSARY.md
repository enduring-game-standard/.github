# Enduring Game Standard: Working Vocabulary

The canonical domain language of the EGS protocol family, and the single source of truth for
cross-protocol terms. Each bold entry name is the handle other repos cite. Each
definition says what a term *is*, not how it is implemented. The public quick reference (the
acronym table and the protocol roster) is `profile/README.md` § Glossary.

**Capitalize a defined term** wherever it carries its glossary meaning, in this file and in every
EGS doc: a Record, the Game Logic, a Game Port. Lowercase means the ordinary English word.

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
  Presentation. Replace the whole set and the result is a **Game Port** (ADR-0011). The
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
The wiring of Records to Processors: which Field feeds each input and output of each Processor,
and the Guards on them. It computes nothing and sets no order of its own; a Processor fires once
the Records it reads are ready, so the order follows from the wiring. A tick is one run of it.
Data flows only Record → Processor → Record, so no Processor calls another: a value one
Processor hands to the next is stored in a Record. Only the Network knows both sides: a Record
knows nothing of the Processors that use it, and a Processor knows nothing of where its inputs
come from or where its outputs go. A state machine is a Network of Guards, never a Processor.

Within the Game Logic the wiring makes each tick deterministic. Every change to a Record goes
through a Processor, the wires and Guards order any two Processors that touch the same Record,
and the Guards obey their two laws (see `Guard`). Processors the wiring leaves unordered give the
same result in any order, so a runtime may run them in parallel. The Presentation has the same
structure but need not be deterministic.

Every loop within a tick is bounded: its count is fixed before it starts, and there is no
`break`, `continue` or early return. The count may be any value known at run time, so a loop over
a list of any length is bounded, and a Processor always halts. The one unbounded loop runs from
one tick to the next, through the Records the game keeps between ticks, so it shows in the
wiring. That is how a game runs forever, and how its state may grow without limit, as a Factorio
factory does; both stay in the Game Logic and stay deterministic. So a Network, unlike a
Processor, can hang or grow without bound. Both are game bugs for tools to find.

The whole game is one Network. Any part of it can be collapsed into a Sub-Network or expanded
into its parts, down to single Records and Processors; no level is privileged.
_Avoid_: execution order; step for a tick (a Petri net step is one set of firings, many to a
tick); treating `phases:`/`order:` as primitives (the wires and Guards state the order)

**Sub-Network**:
A Network that another Network uses as one Processor. From outside it is only its inputs and
outputs; inside it is its own wiring, down to single Processors. It names no Record of any game,
so any game can wire it in. Any part of a Network can be collapsed into a Sub-Network or expanded
back into its parts. Every Sub-Network is also a Bundle: Sub-Network names its place in the
wiring, Bundle names it as a thing published and downloaded.
_Avoid_: subroutine, function call (no Processor calls another)

**Bundle**:
A piece of RUNS logic as published on the commons: a whole game's Network, a Sub-Network or a
single Processor, with every Processor inside it and a template of the Records it expects. It
works like a package from a package manager: found by name and description (Ocarina of Time's
combat), downloaded by hash, with its provenance attached, and each part inside it stays
addressable by its own hash. Whoever downloads one wires it to their own Records and edits their
copy as they like. What is unique to a game is its top Network's wiring; the rest can be shared
Bundles.

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
Variant changes it; a Game Port keeps it exactly.
_Avoid_: rules, Rules region, Rules Processor

**Interface Record**:
A Record that both the Game Logic and the Presentation touch: the only place they meet. The
Presentation writes the inputs the Game Logic reads, and reads the results the Game Logic
writes. No Processor sits on the line; any translation is a Processor on one side.
_Avoid_: boundary Record; "interface" for a top Network's `requires:` and `produces:` (those
cross between the program and the machine)

**Presentation**:
How a game reaches a player on one machine: the Processors and Records that read the machine's
inputs and drive its screen, its sound and its pacing. With the Game Logic and the Interface
Records it makes up the whole game; everything that is neither is Presentation. A Conversion
keeps the Original's Presentation whole, so its code records how the Original looked, sounded,
played and was paced, down to its tick rate. A Game Port replaces it and reads it to decide what to
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
machine, nothing the game computes. A Game Port leaves them behind with the Presentation.

**Conversion**:
An existing game's original program rewritten into RUNS through every stage of the Conversion
Process, with the game exactly the same. A program partway along is *in conversion*, at its
stage.
_Avoid_: "Conversion" for a program short of Standard; Game Port (new Presentation); Variant (changed
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
One shipped version of a game: the exact binary its machine loads. It is what a Conversion must
match. Each regional release and each revision is its own Original. A game born in RUNS has no
binary; its RUNS source is its Original.
_Avoid_: "the Original" for the source listing or for what the designers meant

**Glitch**:
Gameplay the Game Logic allows that its designers did not foresee, such as Super Mario 64's
backwards long jump. A Glitch is not separate Game Logic, so a Conversion that states the Game
Logic completely keeps every Glitch, found or not, without listing any. Removing one changes the
Game Logic: the result is a Variant.
_Avoid_: bug (for the Original's behavior); "recreate" or "remove" a Glitch in a Conversion;
sorting a behavior by intended or unintended

**Game Port**:
The same Game Logic, exactly, under a new Presentation for another machine. How much of the
Original's Presentation it keeps (its look, its sound, its controls, its tick rate) is the
porter's choice, and the Original's Presentation is the reference for each choice: DOOM run at
70 tics a second is still a Game Port, only a less faithful one. Identical Game Logic is what makes
a program a Game Port; no label or side document does.
_Avoid_: Game Port for anything that changes the Game Logic, even to fit a smaller machine (it is a
Variant); Port alone; pure Port, Variant Port

**Guard**:
A DIGS condition on a Network arc that decides whether its Processor fires. The Guards on one
Record obey two laws. Exactly one of them is true, always: a closed enum makes this checkable,
and otherwise an `else` arc closes the set. And a Guard only reads a fact a Processor already
computed; it never computes one. So the decisions live in the arcs and the work lives in the
Processors: no Processor holds an `if` that picks which Processor runs next.
_Avoid_: Phase (the wiring sets the order); Dispatch for a loop (dispatch means choosing which
code runs); dispatcher Processor

**Precondition**:
A condition that must be true before a Processor fires, such as `divisor != 0`. The Processor
states it, and the Guards in the Network make sure it holds. If a Precondition is ever false,
the Network has a bug; the game never depends on what happens then.
_Avoid_: assertion (the Processor does not check it; the Network guarantees it)

### Evolution

**Variant**:
A game whose Game Logic differs from the game it came from: Processors swapped, added or
removed in its Network, the way chess and soccer grow house-rule Variants over time. Changing
the arithmetic or a limit to fit a smaller machine makes a Variant too. Changed Game Logic is what makes a program a Variant, whatever the reason; no label or
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
