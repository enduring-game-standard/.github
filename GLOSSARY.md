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
The language of Game Logic Processor bodies; Guards use its expressions. A DIGS body is pure (it
reads only its inputs and writes only its outputs), total (it halts on every input) and
deterministic (its order of evaluation is fixed, so the same inputs give the same bits on every
machine). It is inspectable because nothing in it is hidden: every value enters and leaves by a
declared input or output, and every loop states its bound. Every game declares its arithmetic in
DIGS: its number types and their operations, exact to the bit. The arithmetic is Game Logic: it
is to DOOM what the six-sided die is to Yahtzee, and DOOM computed with floats is not DOOM. A
game may also declare a fixed bound on its running time or its memory, to reach machines that
need one; neither follows from totality. DIGS is part of RUNS, not a protocol of its own.
_Avoid_: "the RUNS language" (RUNS is the source format; DIGS is its expression language);
calling DIGS a protocol; treating arithmetic as optional; reading "total" as a fixed time or
memory bound (each is declared on its own)

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
**transport** is a Presentation concern (see `Presentation`) that fills and
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
A Network inside another Network, holding its own Records and Processors. A Record belongs to
the smallest Sub-Network that holds every Processor touching it, so a weapon's cooldown count
belongs to the cooldown Sub-Network. Where Processors inside and outside a Sub-Network touch the
same Record, both levels share that one Record. Each copy of a Sub-Network has its own local ID
and its own Records: `vec3` holds three copies of `vec1`, and two ships keep two cooldown counts.
Sub-Networks nest down to single Processors, and each one collapses to one box or expands to its
parts; the game is the same at every zoom. A Sub-Network may keep a Record from one tick to the
next, so purity belongs to its Processors.
_Avoid_: treating a Sub-Network as a Processor; subroutine, function call (no Processor calls
another)

**Bundle**:
A Sub-Network packaged for download: the Sub-Network, every Sub-Network and Processor inside it,
and the Records it owns, each part under its own hash. The game that uses it connects the
Records at its edge to Records of its own. Editing any part makes a new part with a new hash.
_Avoid_: bundle for a Sub-Network inside a game (that is the Sub-Network itself)

**`runs:` namespace**:
The standard's own prefix: a small core of Records and Processors that any game may use so its
parts work with parts from other games, as coreutils is a small core of Unix tools. Only the
standard defines a `runs:` name, and a published `runs:` name keeps its Fields, their types and
their meaning forever; a changed one is a new name. Using `runs:` is optional: a game that uses
only Records, Fields, Processors, Networks and DIGS is compliant under any prefix. The ecosystem
grows under other prefixes, and `runs:` stays small.
_Avoid_: treating `runs:` as required for compliance; adding one game's Records to `runs:`

**Umbrella Prefix**:
A short label before a name (`pong:object`, `hades:`), written for people to read. It is never
identity: a name resolves by its author's public key and its content hash, so two authors who
both publish `pong:object` publish two different Records, and nobody polices prefixes. `runs:` is
the one exception (see `runs:` namespace).
_Avoid_: treating a prefix as a unique name; reading it as a package name (in npm the name is the
identity; here the hash is)

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
keep. Its Processors are written in whatever the machine demands; the standard names no language
for them. Multiplayer transport is Presentation too: remote inputs arrive as Interface Records,
exactly like local ones.
_Avoid_: Realization, Realization region; "presentation" for the output half only; shell,
wrapper (it is not the lesser part)

**Kind**:
The side a Record or Processor declares: Game Logic, Interface, or Presentation. A convention:
from the Separated stage on, every Record and Processor declares one, and a check confirms they
agree, so no Game Logic Processor touches a Presentation Record. So every Processor is a Game Logic
Processor or a Presentation Processor. The protocol ignores Kinds.
_Avoid_: deriving a Processor's side from the Records it touches; type (a Field has a type)

**Declarations**:
What one machine needs to carry a game's RUNS source, none of which runs: where each Record and
routine lives, which register or word holds each value, how each routine is entered and left,
how each value is encoded in bits, and which device carries each input and output. Play cannot
observe any of it, so it holds no Game Logic. The Declarations belong to the machine, not the
game: with the Original machine's Declarations the compiler rebuilds the Original exactly, and a
Game Port brings the Declarations of its own machine.
_Avoid_: binding (the retired name)

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
- **"Bundle"** = a RUNS Bundle (a Sub-Network packaged for download) vs. an AEMS bundle (a signed event that carries the events it cites; draft in aems-schema
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
