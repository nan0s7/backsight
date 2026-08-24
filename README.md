# Backsight

A folder-based cartographer that walks a body of work still in force, and leaves behind a **catalog and a small set of cards** that a later reader can enter at one point, answer one question, and leave.

*A backsight is the reading a surveyor takes back to an already-established point, to orient themselves before measuring anything new. One sighting against something known, then you proceed. That is what this leaves behind, and it is the whole budget: the catalog, then one card, then go.*

That later reader is usually **a model with no memory of the place**. It cannot ask a follow-up. It will believe what the map says. The whole design follows from that.

It tells you **what the nouns are, how they move, what else moves if you touch one, and what is live, leftover, or a ghost.** It does not tell you why anything failed, or what to fix.

**Front door.** Using it: [How to use it](#how-to-use-it) · [What to feed it](#what-to-feed-it) · [How a cold model should walk it](#how-a-cold-model-should-walk-it). Everything from [How it was tested](#how-it-was-tested) down is evidence and limits — read it to decide how far to trust this, skip it to just run it.

## The one rule

**Load the catalog, then one card. Never the whole `objects/` folder.**

This is the rule the produced map states about itself, and it is the reason the form exists. A map you have to read whole is a folder with extra steps, and it costs more than the territory it was supposed to save you from reading.

Both humans and models, handed a folder of small tidy files, will read all of them. So the budget is written into the catalog as an instruction, not left as an assumption.

## The three statuses

Every noun gets exactly one, and the words mean what they say. They are not softened.

| Status | Test | What it costs a reader who trusts it |
|---|---|---|
| **live** | Something reads or writes it now, **and you can name the reader with a path** | Nothing. This is the map working. |
| **leftover** | It was live, nothing reads it now, and it is harmless | Near zero — nothing routes you there |
| **ghost** | A name with no wiring: something live points at it, and the target is missing, empty, or inert | **You implement the wrong world** |

**The evidence rule is the whole discipline: "live" requires a named reader at a named path.** A folder that exists is not evidence anything reads it. A name in a README is not a reader — documentation is precisely where intentions outlive implementations.

Ghosts are the payload. A ghost card is built around **what points at it**, because the pointer is the live half and the pointer is what the next reader will actually hit.

## The words this uses

Six terms do the work. They are not standard vocabulary, so they are defined once here.

**Territory** — the body of work being mapped. A folder where the files *are* the system, not a folder that describes one.

**Noun** — a thing in the territory worth knowing about before you change something. Usually a file, a folder, or a convention. The most valuable ones are often not files at all: a status encoded by which directory something sits in, a naming rule, a field something reads. A directory listing cannot show you those, which is exactly why they get broken by accident.

**Card** — one page about one noun. Fixed shape, short, and always shorter than the thing it describes. A card that only repeats its source is a photocopy and gets cut.

**Catalog** — the entry page. It points at every card and stores almost nothing. You read it, pick one card, and stop.

**Hit** — *what else moves when you change this.* If you rename this file, edit this rule, delete this folder — what breaks, what silently changes behaviour, what stops matching. Each hit says **why** it moves, not just that it does. Hits are first-order only: the things that move directly. Chains three steps deep are guesses.

**Wrong neighbour** — the *"does not hit"* line, and the one that makes this a map instead of a glossary. It names the thing you would reasonably assume also moves, and does not. It is the more useful half, because you already suspect the hits; nobody warns you about the safe-looking neighbour you were about to edit for no reason, or the one you assumed was covered and isn't.

A worked pair, from the map in `examples.md`: the script finds its own running copy by matching its own filename. Renaming it **hits** that self-recognition — silently, two copies then issuing conflicting fan commands as the only symptom. It **does not hit** the systemd unit, which is the first file anyone opens before renaming a script, *because that unit was already pointing at a path this repo never produces.* Finding nothing to change there reads as confirmation the rename is safe. That is the kind of thing a map is for.

## How to use it

1. **Drop this folder into a Claude project.** It is the method, not a map. The one rule above governs what comes *out* — the catalog and the cards — not what goes in here.
2. **Give it a path and a reader.** *"Map `~/code/thing` for the next developer, who has never seen it."* The reader is a role, not a task. It will ask if you leave it out, and it will narrow the territory rather than the reader if nothing can be answered from one card.
3. **You don't need to say where the map goes.** It writes a sibling folder next to the territory — point it at `~/code/thing` and you get `~/code/thing-map/`. Name a location only if you want a different one.
4. **Read what comes back the way it tells you to:** the catalog, then one card.

Step 2 as one message, if you want something to paste:

```
Read identity.md, then follow rules.md.
Map <path> for <reader>, who will <what they change>.
```

The cartographer reads `identity.md` first, which hands off to `rules.md`, which calls the rest at the passes that need them. Nothing in that chain reads this page.

## What to feed it

Two things:

1. **A path** — a folder-based body of work where the files *are* the system. A repo, a knowledge vault, a client delivery folder, an SOP pack.
2. **Who the later reader is, and what they will change.**

It needs both, and it will ask for the second. Without a reader it cannot judge what is worth carding, and it defaults to carding the folder structure — which is a directory listing with confidence added.

## What it refuses

Refusal is a successful outcome here, not a failure to try.

- **Territories the folder only renders.** If the system actually lives in a SaaS admin panel, a database, or a ticket tracker and the folder is an export, it says so and stops. The test is *does changing these files change the system.*
- **Methodologies, skill folders, and itself.** A map of a mapping system teaches a reader nothing about a place.
- **Territories it cannot gather evidence in.** If it can see names but cannot establish readers, it says what it would need rather than producing a listing with statuses guessed on top.

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

**`Does not hit` is required** — the wrong neighbour, defined above. A card without one is a glossary entry and does not ship.

`Open` is also required. It costs one line, and it is the only place the map admits what the survey was unsure of — which a reader who cannot ask a follow-up has no other way to learn.

## How a cold model should walk it

```
catalog (front door)  ->  one card  ->  stop
```

**Two reads, and stopping is a step.** The front door is a section of the catalog, not a separate file.

**If the question matches no card: say so and stop.** Do not read more cards hoping one becomes relevant, and do not assemble an answer from two cards about neighbouring things — that is a guess with citations attached, and at the point of use it is indistinguishable from a real answer.

## What's in here

| File | Job | Called at |
|---|---|---|
| `identity.md` | Who the cartographer is; what it refuses | Before Pass 0 |
| `rules.md` | The procedure — six passes, discrimination rules, card shape | The spine |
| `reference/collisions.md` | Method for finding collisions — *this* territory's own are in the catalog in `examples.md`, and why | Pass 2 |
| `reference/card-types.md` | The closed set of four card types | Pass 4 |
| `reference/walk-order.md` | How the finished map gets walked | Pass 4, and Pass 5 |
| `examples.md` | One worked map — catalog, five cards, a note on carving | Before writing the catalog |
| `nfancurve-map/` | The worked map as walkable files, so the two-hop walk can be tested | Never read by the procedure |
| `README.md` | Human-facing orientation | Never required by the procedure |
| `LICENSE` | MIT | Distribution only |

Each file the procedure reads is invoked by name at the point listed: `identity.md` hands off to `rules.md`, and `rules.md` opens with an Inputs table calling `identity.md`, `examples.md` and the three reference files at the passes shown. A reference file nothing calls is either dead or a missing step in the procedure.

### Why `reference/collisions.md` holds a method, not this territory's collisions

The competition's folder shape reads as though `reference/` should hold the collisions found in the territory being mapped. It holds **the method** instead — the four classes, how to find them, how to record them.

A cartographer that only knew one territory's collisions could not be pointed anywhere else. The collisions belong to the *produced map*, in its catalog, where a reader hits them on arrival. This territory's actual collisions are in `examples.md`, where a worked map belongs.

### What the procedure does not read

`README.md` is documentation for a person deciding whether to use this. **Nothing in the procedure reads it.** Delete it and the cartographer runs identically.

The same goes for `nfancurve-map/`. It is **output, not method** — the product of one walk, shipped as separate files so a stranger can test the two-hop walk without running anything. It is named the way Pass 4 names any produced map, `<territory>-map/`, because that is what it is. `rules.md` never opens it, and `examples.md` already carries the same text as calibration.

**So if you are dropping this folder into a project to actually use it, leave out `README.md`, `LICENSE` and `nfancurve-map/`.** Four files and `reference/` are the whole method.

Where this page restates something, it is summarising for orientation. **`identity.md` and `rules.md` are authoritative.** If they disagree with this page, they win and this page is the bug.

### Walk the map from somewhere neutral

Put the map beside the territory, and walk it from a session that is not rooted inside either.

An assistant started inside a folder with its own `CLAUDE.md`, `AGENTS.md` or routing table will load those first, before it ever opens the catalog — and then it is walking the map with the territory's own instructions already in context, which is the one condition the map is built to avoid. A routing table in particular is designed to send a reader toward related material, which is exactly the outward drift the load budget forbids.

This has been observed, not theorised. A map walked from inside such a folder pulled in a planning document and a project note about the territory, neither of them part of the map, and produced a question built on them rather than on the card.

### No outside context is required

The method runs from the copied folder plus the two user-supplied inputs. Personal profiles, memory files, task routers and the author's other context are neither inputs nor fallbacks.

## The method

Built on Interpretable Context Methodology — folder structure as agent architecture, plain markdown, one file one job, reference material held structurally apart from working input. Van Clief & McDermott, *Interpretable Context Methodology: Folder Structure as Agent Architecture*.

**The first version of this folder was generated with the [ICM Architect skill](https://github.com/RinDig/icm-architect)**, told that the repeating unit is a folder someone will change — which is how it picks the system map form rather than a pipeline. The walks below are what reshaped it from there.

The pipeline lives inside `rules.md` rather than in numbered stage folders, and `README.md` (human) and `identity.md` (agent) split the entry-file job. Both are departures from ICM's defaults, taken because the competition prescribes a flat five-item folder and the folder itself is the product — nothing is instantiated from it.

## How it was tested

### Six territories

| Territory | Shape | Size | Cards | Map |
|---|---|---|---|---|
| A GPU fan-curve utility | POSIX shell | 6 files, 459 lines | 5 | 86 lines — 19% |
| A knowledge vault | prose, captures, conventions | 413 files | 4 | not published |
| A colour-theming utility | three shell scripts | 3 files | 2 | not published |
| An assessment tool | Python + React | 76 files | 1 | 21 lines |
| A macOS app | Swift + SwiftPM | 123 files | 6 | 97 lines |
| A constructed-language site | Docusaurus, 950 markdown | 1,038 files | 3 | **49 lines** |

Only the first is published, in `examples.md`. Two are the author's own, three are not, and the assessment tool was cloned at a pinned commit and never opened by hand — the only test of whether the rules survive ground with no memory attached.

**The compression is the result.** Ninety-five per cent of a 1,038-file site fits in three cards, because the shelf a collaborator actually needs is not the 895 files: `static/words.csv` is the authoritative lexicon at 874 records, and `docs/words/` is a build product that `scripts/build.zx.mjs` deletes and regenerates. A correction typed into the shelf is gone on the next build. That is the kind of thing a directory listing cannot show anyone, and it is what the form is for.

### What repeated runs disagree about

Eight consecutive runs were made against a copy of this folder that never received the rule changes being tested — a mistake that produced the only controlled measurement here. Same folder, same territory, same reader, eight times.

**The territory findings were identical every time**: the same five nouns, the same two ghosts, the same three collisions. What forked was the writing — one phrasing convention landed once in eight, another twice. **The runs disagree about the method, not about the place.**

Where that leaves the rules is the useful part. Rules naming a mechanical, checkable output land immediately and hold. Rules about phrasing under judgement fork roughly half the time however they are worded, ordered or enforced — and adding enforcement did not move them.

So the last rule added was written as an inventory line, with a prediction recorded in the commit before it was run: agent-instruction files — `CLAUDE.md`, `AGENTS.md`, `.cursorrules` — become candidate nouns by name, because they read as scaffolding and get skipped, and because a file called `CLAUDE.md` is read as addressed to the reader rather than as a claim to check. Two territories with known answers, both predicted, both confirmed:

- **The macOS app** ships `AGENTS.md` and `CLAUDE.md` byte-identical at 216 lines, with no generator, link or test between them, and nothing in the repository referencing either name. Two names, one job, nothing keeping them in step.
- **The assessment tool**'s entry file documents a Supabase database, a Redis queue and WebSocket streaming. A search of the backend and frontend for all three returns a comment saying job storage is in memory. Recorded, with the search, as out of scope rather than carded — the reader asked about providers.

Both maps also came back *smaller* than the runs before them.

### Three findings that changed the rules

- **The ghost rule was wrong, and a card said so in its own `Open` line.** On the colour-theming utility, a documented sibling checkout was carded as a ghost. The test is now discoverability rather than provenance: a documented prerequisite is `live`, and its fragility belongs in `Hits`.
- **A rule change moved a real finding from correctly-excluded to carded.** The assessment tool was walked four times as the rules changed. Under *one reader, one job* a false architecture claim in its entry file sat off the reader's path and was rightly left out. Under *the reader is a role* it is in scope, and the next walk carded it with the search behind it. Same territory, same question, same tool.
- **The negative rule has now caught four defects, three of them in this folder's own maps.** You cannot cite an absence, so a negative must name where you looked. The sharpest catch: a card claimed `config` held no device setting, on the strength of a grep for interface words that never searched for topology words. `config:48` is the fan-to-GPU map. That answer had already reached a reader.

### Refusal test — passed on three territories

- **Pointed at this folder:** refused. A methodology folder is out of scope by `identity.md`.
- **Pointed at a sibling method folder** — a diagnostician built to the same discipline: refused, naming the clause it failed and citing the two files that identify the territory as a methodology. No partial map and no offer to proceed.
- **Pointed at a generated archive** — a folder of 138 markdown files, each one produced by a script from session logs that live elsewhere, overwritten whenever the script re-runs: refused at Pass 0 as a rendering. Changing those files does not change the system, and a reader acting on such a map would edit output that is regenerated on the next run.

The generated archive is the case worth having. It sits *inside* the published territory, it looks exactly like mappable material, and the test that catches it is behavioural rather than cosmetic.

### Cold reads — the bar, and what failing it bought

The bar: a fresh reader, given *only* `catalog.md` and one card, answers "what is this and what moves if I change it" and **stops without requesting more files**. Four cold reads across two rounds, one pass and three failures. Every failure produced a deletion rather than an addition.

- **A card claimed nothing scanned a pair of folders, and something did.** The reader opened eleven further sources and came back with a better answer than the card contained. It was right to. The claim had been false since the day it was written — nothing decayed, the survey never looked. Pass 3 now demands the same evidence for a negative as for a positive, and since you cannot cite an absence, it demands naming *where you looked*.
- **The catalog header told the reader to go and verify.** *The file wins* sat four lines from the load budget and read as permission. It is precedence — who wins when a disagreement surfaces — not an errand, and it now says so.
- **The load budget never named the territory.** It forbade plans, notes and issue threads *about* the place, which reads as leave to read the place itself.
- **`see other-card.md` pointers inside `Does not hit` lines.** A reader treats a pointer as an instruction. Forbidden outright now, not discouraged.
- **The triangle rule required the wrong neighbour to be carded so a reader could check it** — which is a second card, which is the failure the whole form exists to prevent. It was the only rule here pointed against the one rule. Cut, not patched.

The passing read is worth as much. It stopped at a single card and gave its reason: the second *"would have bought confirmation, not information, at the cost of the stated budget."* The `Does not hit` line did that work — the question carried a false premise, and the wrong-neighbour line pre-empted it, which is why one card was enough.

### Where the load budget actually fails — three more cold reads

Neutral working directory, fresh reader each time, the territory not mentioned. One run walked correctly — catalog, one card, no territory — and two did not. What they exposed is worth more than the score.

**All three ran a recursive text search across the map folder before reading anything.** Not "opened a second card hoping" — a tooling reflex that swept every card in one action. That is a limit this form cannot fully close: **the load budget is stated inside `catalog.md`, which you can only reach by an action that may already have violated it.** There is no doorplate on the outside of the building. The budget is now the catalog's first line, which is the most a document inside the folder can do.

The honest reframe is that the budget is a discipline the map declares, not a fence it enforces — and **the map's real protection is being small.** In the worst run the reader swept all 86 lines of it instead of reading 459 lines of territory. Total budget failure still cost about a fifth of the source.

**Two findings against the map itself:**

- **A task-shaped question defeats it, and probably always will.** The catalog names *"find out what it did last night"* as an out-of-scope example. Asked exactly that, the reader went and found out — territory, process table, system log. A map cannot out-instruct the person asking it. The claim that it refuses tasks is now written as a limit below, not as a behaviour.
- **The run that obeyed the budget gave a wrong answer, because a card was wrong.** Asked what to change for a second GPU, it answered from one card and said `config` was not involved. `config:48` is `fan2gpu`, the fan-to-GPU map, read at `temp.sh:174`. The card had asserted config held no device setting on the strength of a grep for interface words that never looked for topology words. An under-searched negative, propagated verbatim into user-facing advice. Corrected, and it is the third defect the negative rule has caught.

**The published map has since been replaced by tool output.** Two runs of the current folder, in separate sessions, produced the same five cards, the same two ghosts and the same three collisions; `examples.md` and `nfancurve-map/` now carry that output verbatim rather than a hand-edited version of it. The reason is that runs copy this file — the second reproduced its front door character for character. A worked example the tool cannot itself produce sets a bar it will miss in front of a stranger.

### Slurp test — clean

The folder was searched for any instruction to load everything, read the whole map, or add every file to a project. There are none, in either the method files or the worked map. Every occurrence of that language is a prohibition.

## What it cannot know

Stated plainly, because the limits are the point.

- **The cold-read test passed on the second attempt, not the first.** The failure is written up in full under **Cold reads** above, and it is what produced the rule about negatives in `rules.md`. **n=1 on the passing run**, one question, one card. It has not been run across every card or every question shape.
- **The published map has failed more cold reads than it has passed.** Seven runs, two clean walks. Every failure is written up above with what it changed — the triangle rule is gone because a reader was sent to a second card to check a negative, and the load budget sits first in the catalog because three readers swept the folder before reading a word of it.
- **The evidence base is six territories, and three of them were not the author's.** The most-walked of those is a third-party repo cloned at a pinned commit and never opened by hand: two of the current rules came out of it, and its map was then checked against the source line by line — **six of seven claims verified exactly, and the seventh was false**, written up above as a regression. That is the sharpest measurement in this repo, and it is the one taken on ground with no memory attached. **What it is not is an independent operator.** Every walk was run by the author. That is a larger gap than the territory count, and it is not closed by adding more territories.
- **This is a map, not an assistant, and it cannot enforce that.** It answers *what is here* and *what moves if I change it*. Asked instead to get something working, it is the wrong instrument — but saying so in the catalog does not stop a reader, and testing showed it does not: given a task the catalog names as out of scope, the reader did the task. Treat the boundary as a description of what the map is good for, not a refusal it can make stick.
- **The folder is better evidenced than its output.** Maps produced by this method have been checked closely; whether those maps then get *used well* rests on a handful of sessions. One walked cleanly. One found no matching card and read a second card hoping. One drifted outward into the author's own planning notes. The last two were caused by gaps since closed — a catalog that never said what it had left out, and a load budget bounded only against reading more cards — but the usage record is thin and is not improved by asserting otherwise.
- **`Open` lines are only as honest as the cartographer writing them.** Nothing in the method forces an uncertainty to be noticed — the line is required, but a confident wrong card will fill it confidently.
- **The map does not track the territory after the survey.** A `live` status is a claim about the day it was written, and the catalog carries that date so a reader can weigh it. Nothing re-verifies it afterward, and nothing needs to: **re-running the cartographer is the maintenance path**, and it costs one session rather than one excavation. What the map owes you is an honest survey and its date. Deciding whether that date is fresh enough to act on is yours.
- **`Hits` is first-order only, by design.** Deeper waterfalls are guesses dressed as maps. This is a deliberate limit and it means the map will understate the blast radius of a large change.
- **It cannot map what it cannot read.** Binaries, encrypted stores, and anything behind an API are invisible, and the map will not mention their absence unless something inside the territory points at them.
- **A map is not an audit.** It says what is there, not whether it is any good. If you want to know why something failed, this is the wrong instrument.

## Licence

MIT.
