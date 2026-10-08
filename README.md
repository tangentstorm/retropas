# retropas

_retro pascal_ — the result of a June 2013 Turbo Pascal coding spree.

## What this is

A single-file, 831-line DOS program (`RETP.PAS`) that builds a tiny
object-oriented runtime environment in text mode. It combines three
things: a stack-machine VM with its own opcode set, an actor/morph scene
graph with message passing, and an interactive command shell — all drawn
with Turbo Vision units (`uses crt, dos, objects, drivers, views`). It's
a weekend toy, but a revealing one: VMs, object systems, message
passing, and live interactive environments are all themes he's still
working on.

## Contents

- **`RETP.PAS`** (831 lines) — the entire program. Turbo Pascal 7-era
  object syntax (`object(...)` with virtual methods), built on Turbo
  Vision's `TObject` hierarchy.
- **`README.md`** — this file. (Replaces the original one-line
  `README.TXT`, which just said "retro pascal".)

### What's inside RETP.PAS

- **Object hierarchy** — `BaseObj` (extends `objects.TObject`) fans out
  into atoms (`AtomObj` → `IntObj`, `StrObj`, `RefObj`), tagged objects
  (`TaggedObj` → `SymbolObj`, `TokenObj` → `TupleObj`), type/field
  definitions (`TypeDefObj`, `FieldDefObj`), and actors
  (`ActorObj` → `GroupObj` → `MorphObj` → `MachineObj`, `ClockObj`,
  `ShellObj`). Classic OOP-in-Pascal with constructors/destructors and
  virtual `Update`/`Render`/`Handle`.
- **`MachineObj`** — a stack VM living inside a morph. Has data and
  address stacks, a memory buffer, and input/output buffers, and executes
  an opcode set covering stack shuffling (`opDup`, `opSwp`, `opRot`),
  logic (`opNot`, `opXor`, `opAnd`), arithmetic (`opAdd`…`opDvm`,
  shifts, comparisons). `opFrk` (fork) and `opSpn` (spawn) are stubbed
  `{-- todo --}` — the concurrency idea never got implemented.
- **`ShellObj`** — an interactive shell morph with its own VM instance,
  a word dictionary (`DictObj`), and a clock morph on screen. Typing a
  known word prints its value in green; unknown commands get a red/yellow
  "unknown command" error. Mouse support is initialized (`ShowMouse` /
  `HideMouse`).
- **`ClockObj`** — a clock morph (commit: "created a clock morph"),
  registered alongside the shell in the demo scene.
- **Message/event system** — `MessageObj` / `EventObj` with virtual
  `Handle` dispatch; the final commit "fleshed out group objects and
  message passing."

The main loop is pure morphic style:
`Create; repeat Update; Render; until numActors = 0; Destroy` — dead
actors are disposed and swapped out of the actor list each frame.

## Historical note

Seven commits over three days, June 3–6, 2013 — a classic coding-spree
repo. This is deep-backlog Michal: pre-Minavo, pre-everything public.
The DOS text-mode aesthetic and Turbo Pascal nostalgia ("retro pascal")
put it in the same lineage as his later array-language and VM play
(J, concatenative languages, the bend informalizer). The morphic
Update/Render loop and the message-passing actors are an early, rough
draft of the live-environment instincts that show up in his later work.

## Status

Untouched since June 2013; no issues, no PRs, no CI, no dependencies
beyond the Turbo Pascal 7 / Turbo Vision units it was written against.
It will not compile as-is with a modern toolchain — it's a DOS-era
artifact (real-mode `crt`/`dos` units, Turbo Vision object model).
Kept as-is for the historical value: a clean little time capsule of the
spree, and the whole thing fits in one file.
