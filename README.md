# Backsight

A folder-based cartographer that walks a body of work still in force, and leaves behind a **catalog and a small set of cards** that a later reader can enter at one point, answer one question, and leave.

*A backsight is the reading a surveyor takes back to an already-established point, to orient themselves before measuring anything new. One sighting against something known, then you proceed. That is what this leaves behind, and it is the whole budget: the catalog, then one card, then go.*

That later reader is usually **a model with no memory of the place**. It cannot ask a follow-up. It will believe what the map says. The whole design follows from that.

It tells you **what the nouns are, how they move, what else moves if you touch one, and what is live, leftover, or a ghost.** It does not tell you why anything failed, or what to fix.

## The one rule

**Load the catalog, then one card. Never the whole `objects/` folder.**

A map you have to read whole is a folder with extra steps, and it costs more than the territory it was supposed to save you from reading. The produced catalog states this budget itself, as its first line, because both humans and models handed a folder of small tidy files will read all of them.

## How to use it

1. **Drop this folder into a Claude project.** You need `identity.md`, `rules.md`, `examples.md` and `reference/`. Leave out `README.md`, `LICENSE` and `nfancurve-map/` — nothing in the procedure reads them.
2. **Give it a path and a reader.** *"Map `~/code/thing` for the next developer, who has never seen it."* Both are required. The path must be a folder-based body of work where the files *are* the system — a repo, a knowledge vault, a client delivery folder, an SOP pack. The reader is a role, not a task; without one it cannot judge what is worth carding, and defaults to carding the folder structure, which is a directory listing with confidence added.
3. **You don't need to say where the map goes.** It writes a sibling folder next to the territory — point it at `~/code/thing` and you get `~/code/thing-map/`. Name a location only if you want a different one.
4. **Read what comes back the way it tells you to:** the catalog, then one card — and walk it from a neutral working directory. A session started inside a folder with its own `CLAUDE.md`, `AGENTS.md` or routing table loads those before it ever opens the catalog, and then it is walking the map with the territory's own instructions already in context. This has been observed, not theorised.

Step 2 as one message, if you want something to paste:

```
Read identity.md, then follow rules.md.
Map <path> for <reader>, who will <what they change>.
```

## What comes back

`catalog.md`, plus N cards in an `objects/` folder.

The catalog carries the front door, one line per card, the name collisions resolved in place, the load budget, and — in the header, not buried — **the disagreement rule: where a card and the real file disagree, the file wins.**

Every card is the same fixed shape, so you can skim any of them without learning a new format. One field is conditional: only a ghost card carries `What points at it`.

```
# <noun>
Status:  live | leftover | ghost
Source:  <path(s)>

What it is:                 one or two sentences
Why it is shaped that way:  one or two sentences
What points at it:          ghost cards only — what makes the claim, with paths
Hits:                       what moves if you change this, each with why
Does not hit:               the wrong neighbour, and why the reader will reach for it
Open:                       what this card is unsure of
```

**`Does not hit` is required.** A card without one is a glossary entry and does not ship. `Open` is also required — it costs one line, and it is the only place the map admits what the survey was unsure of, which a reader who cannot ask a follow-up has no other way to learn.

## What it refuses

Refusal is a successful outcome here, not a failure to try.

- **Territories the folder only renders.** If the system actually lives in a SaaS admin panel, a database, or a ticket tracker and the folder is an export, it says so and stops. The test is *does changing these files change the system.*
- **Methodologies, skill folders, and itself.** A map of a mapping system teaches a reader nothing about a place.
- **Territories it cannot gather evidence in.** If it can see names but cannot establish readers, it says what it would need rather than producing a listing with statuses guessed on top.

## The words it uses

Not standard vocabulary, so they are defined once.

**Territory** — the body of work being mapped. A folder where the files *are* the system, not a folder that describes one.

**Noun** — a thing in the territory worth knowing about before you change something. Usually a file, a folder, or a convention. The most valuable ones are often not files at all: a status encoded by which directory something sits in, a naming rule, a field something reads. A directory listing cannot show you those, which is exactly why they get broken by accident.

**Card** — one page about one noun. Fixed shape, short, and always shorter than the thing it describes. A card that only repeats its source is a photocopy and gets cut.

**Catalog** — the entry page. It points at every card and stores almost nothing. You read it, pick one card, and stop.

**Hit** — *what else moves when you change this.* If you rename this file, edit this rule, delete this folder — what breaks, what silently changes behaviour, what stops matching. Each hit says **why** it moves, not just that it does. Hits are first-order only: the things that move directly. Chains three steps deep are guesses.

**Wrong neighbour** — the *"does not hit"* line, and the one that makes this a map instead of a glossary. It names the thing you would reasonably assume also moves, and does not. It is the more useful half, because you already suspect the hits; nobody warns you about the safe-looking neighbour you were about to edit for no reason, or the one you assumed was covered and isn't.

A worked pair, from the map in `examples.md`: the script finds its own running copy by matching its own filename. Renaming it **hits** that self-recognition — silently, two copies then issuing conflicting fan commands as the only symptom. It **does not hit** the systemd unit, which is the first file anyone opens before renaming a script, *because that unit was already pointing at a path this repo never produces.* Finding nothing to change there reads as confirmation the rename is safe. That is the kind of thing a map is for.

### The three statuses

Every noun gets exactly one, and the words mean what they say. They are not softened.

| Status | Test | What it costs a reader who trusts it |
|---|---|---|
| **live** | Something reads or writes it now, **and you can name the reader with a path** | Nothing. This is the map working. |
| **leftover** | It was live, nothing reads it now, and it is harmless | Near zero — nothing routes you there |
| **ghost** | A name with no wiring: something live points at it, and the target is missing, empty, or inert | **You implement the wrong world** |

**The evidence rule is the whole discipline: "live" requires a named reader at a named path.** A folder that exists is not evidence anything reads it. A name in a README is not a reader — documentation is precisely where intentions outlive implementations.

Ghosts are the payload. A ghost card is built around **what points at it**, because the pointer is the live half and the pointer is what the next reader will actually hit.

## What's in here

| File | Job | Called at |
|---|---|---|
| `identity.md` | Who the cartographer is; what it refuses | Before Pass 0 |
| `rules.md` | The procedure — six passes, discrimination rules, card shape | The spine |
| `reference/collisions.md` | Method for finding collisions — the four classes, how to find them, how to record them | Pass 2 |
| `reference/card-types.md` | The closed set of four card types | Pass 4 |
| `reference/walk-order.md` | How the finished map gets walked | Pass 4, and Pass 5 |
| `examples.md` | One worked map — catalog, five cards, a note on carving | Before writing the catalog |
| `nfancurve-map/` | The worked map as walkable files, so the two-hop walk can be tested | Never read by the procedure |
| `README.md` | Human-facing orientation | Never required by the procedure |
| `LICENSE` | MIT | Distribution only |

`identity.md` hands off to `rules.md`, which opens with an Inputs table calling `examples.md` and the three reference files at the passes shown. A reference file nothing calls is either dead or a missing step in the procedure.

**`identity.md` and `rules.md` are authoritative.** Where this page restates them it is summarising for orientation; if they disagree with this page, they win and this page is the bug.

## The method

Built on Interpretable Context Methodology — folder structure as agent architecture, plain markdown, one file one job, reference material held structurally apart from working input. Van Clief & McDermott, *Interpretable Context Methodology: Folder Structure as Agent Architecture*.

**The first version of this folder was generated with the [ICM Architect skill](https://github.com/RinDig/icm-architect)**, told that the repeating unit is a folder someone will change — which is how it picks the system map form rather than a pipeline. The walks below are what reshaped it from there.

The pipeline lives inside `rules.md` rather than in numbered stage folders, and `README.md` (human) and `identity.md` (agent) split the entry-file job. Both are departures from ICM's defaults, taken because the competition prescribes a flat five-item folder and the folder itself is the product — nothing is instantiated from it.

## How it was tested

| Territory | Shape | Size | Cards | Map |
|---|---|---|---|---|
| A GPU fan-curve utility | POSIX shell | 6 files, 459 lines | 5 | 86 lines — 19% |
| A knowledge vault | prose, captures, conventions | 413 files | 4 | not published |
| A colour-theming utility | three shell scripts | 3 files | 2 | not published |
| An assessment tool | Python + React | 76 files | 1 | 21 lines |
| A macOS app | Swift + SwiftPM | 123 files | 6 | 97 lines |
| A constructed-language site | Docusaurus, 950 markdown | 1,038 files | 3 | **49 lines** |

Only the first is published, in `examples.md`. Two are the author's own, three are not, and the assessment tool was cloned at a pinned commit and never opened by hand.

**Compression is the result.** Ninety-five per cent of a 1,038-file site fits in three cards, because the shelf a collaborator actually needs is not the 895 files: `static/words.csv` is the authoritative lexicon, and `docs/words/` is a build product the build script deletes and regenerates. A correction typed into the shelf is gone on the next build. That is what a directory listing cannot show anyone.

**Repeatability: the place, not the writing.** Eight consecutive runs against a frozen copy — same folder, same territory, same reader — produced identical territory findings every time: the same five nouns, the same two ghosts, the same three collisions. What forked was the phrasing. Rules naming a mechanical, checkable output land immediately and hold; rules about phrasing under judgement fork about half the time however they are worded, and enforcement did not move them.

**Cold reads: seven runs, two clean walks.** The bar is a fresh reader, given only `catalog.md` and one card, answering "what is this and what moves if I change it" and stopping. Every failure produced a deletion rather than an addition — the triangle rule is gone because it sent a reader to a second card to check a negative, `see other-card.md` pointers are forbidden outright, and the load budget is now the catalog's first line because three readers swept the whole folder before reading a word of it. Pass 3 now demands the same evidence for a negative as for a positive, and since you cannot cite an absence, it demands naming *where you looked* — a rule that has since caught four defects, three of them in this folder's own maps.

**Refusals: three territories, three refusals.** Pointed at itself, at a sibling method folder, and at a generated archive of 138 markdown files produced by a script from logs living elsewhere. The archive is the case worth having: it sits inside the published territory, it looks exactly like mappable material, and the test that catches it is behavioural — changing those files does not change the system.

**The published map is tool output, not hand-written.** Two runs of the current folder, in separate sessions, produced the same five cards, the same two ghosts and the same three collisions; `examples.md` and `nfancurve-map/` carry that output verbatim. A worked example the tool cannot itself produce sets a bar it will miss in front of a stranger.

## What it cannot know

- **A `live` status is a claim about the day it was written.** The catalog carries that date. Nothing re-verifies it afterward — re-running the cartographer is the maintenance path.
- **`Hits` is first-order only, by design.** Deeper waterfalls are guesses dressed as maps. It will understate the blast radius of a large change.
- **It cannot map what it cannot read.** Binaries, encrypted stores, and anything behind an API are invisible.
- **A map is not an audit.** It says what is there, not whether it is any good. If you want to know why something failed, this is the wrong instrument.
- **It is a map, not an assistant, and it cannot enforce that.** Given a task the catalog names as out of scope, a tested reader did the task anyway. Treat the boundary as a description of what the map is good for, not a refusal it can make stick.
- **The load budget is a discipline the map declares, not a fence it enforces.** It is stated inside `catalog.md`, which you can only reach by an action that may already have violated it. The map's real protection is being small.
- **`Open` lines are only as honest as the cartographer writing them.** Nothing forces an uncertainty to be noticed — a confident wrong card will fill the line confidently.
- **Every walk was run by the author.** Six territories, three of them not the author's, but no independent operator. That is a larger gap than the territory count, and it is not closed by adding more territories.

## Licence

MIT.
