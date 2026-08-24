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

### Walk 1 — a 459-line shell utility (published in `examples.md`)

A POSIX script driving GPU fan curves, written by the author in 2018. Dormant since a stranger's pull request merged in January 2023, and still working — which is the test Pass 0 applies, not recency. Public, so nothing is sanitised. Five cards, **97 non-blank lines against 459** — twenty-one per cent of the territory.

Small on purpose. A map earns nothing on a folder you can read in one sitting *unless* the folder misleads you, and this one does in three separate ways:

- **Two ghosts of different kinds.** A systemd unit launching a path the repo never creates, documented right up to the point where it would have to say what to do. And a hardware-simulation flag pointing at a sibling checkout, absent from the script's own help text and from both doc files — discoverable only by reading the `getopts` string.
- **A documented function that does not exist.** `README.md:25` tells a reader without `procps` to comment out `check_already_running`. The function is `kill_already_running`.
- **A seam that is not one.** Every hardware instruction routes through a single variable, which reads as an abstraction layer and is not — the nvidia-settings argument syntax is inline at all four call sites, and device detection recovers a count by string-stripping the tool's English output. Swapping the binary is one line; swapping the interface is four rewrites and a parser.
- **A change that inverts the expected answer.** The rename-versus-systemd pair, worked through under **Wrong neighbour** above. It is the finding that made a six-file repo worth mapping at all.

### Walk 2 — a 413-file knowledge vault (not published)

Run to check `rules.md` was not repo-shaped: prose, conventions and dated captures rather than code, mapped as the corpus a successor inherits.

The nouns came out completely different — a routing table, a status convention encoded by folder membership, a pair of byte-identical entry files with nothing keeping them in step. The rules transferred, which was the question.

It also produced the sharpest single finding of the three walks. The territory encodes project status by which of four folders a file sits in; those four hold eleven files, while the named project folders beside them hold a hundred and five and carry no status at all. The convention governs a tenth of what its name implies.

### Walk 3 — a second shell repo (not published)

A three-script colour-theming utility. Used mainly to test whether the ghost rule generalised, and it is where that rule broke and had to be rewritten: a documented sibling checkout was carded as a ghost, and the card said in its own `Open` line that the rule looked wrong. It was. The rewritten rule is in `rules.md`, Pass 2, *A ghost is a claim the territory does not honour* — the test is now discoverability, not provenance.

### Walk 4 — a third-party repo the author had never opened (not published)

A Python and React assessment tool, 76 tracked files, cloned at a pinned commit and never updated. **The only territory here that is not the author's own**, and so the only test of whether the rules survive ground with no memory attached. Walked four times as the rules changed. Two of the current rules came out of it, and the fourth walk is the closest thing in this project to a controlled experiment.

| Walk | Rules at the time | Cards | Result |
|---|---|---|---|
| 1 | before *card the change* | 5 | All correct, collectively unusable |
| 2 | after it | 1 | First walk to stay inside its own budget here |
| 3 | after entry-file diligence | 1 | Re-derived walk 2 independently, from the live files |
| 4 | reader as a role, triangle cut | 2 | Two doors, each answerable alone |

Walk 1's five cards were each true, and no single one answered the question a reader actually arrived with — every front-door question was a sub-question of one job, so adding a provider meant opening all five. That is where *card the change, not the parts of the change* comes from. Walk 2 dropped to one card and found three things the five-card version had missed. Walk 3 re-derived walk 2's central finding before opening either prior catalog, and declined to card a false architecture claim in the entry file — correctly, as scope — which exposed a contradiction in that rule and got it fixed.

**The fourth walk is why this section exists.** Walk 3 was right to leave the entry file's architecture claim uncarded: under *one reader, one job* it sat off the reader's path. Under *the reader is a role*, it is in scope — and walk 4 carded it as a ghost, with the search behind it. Same territory, same reader's question, same tool. The rule change moved a real finding from correctly-excluded to carded, and produced two cards behind two doors: neither the five-card mush nor the single card only one job could use.

**Its claims were then checked against the source line by line. Six of seven verified exactly**, including the two hardest negatives — a helper that is imported and never called, appearing exactly twice in the whole territory, and a nine-term search of the backend for the documented database, queue and socket layers returning exactly one line, a comment. Pass 1's counting rule fired unprompted, refusing to call a nine-file directory nine providers when it holds a base contract, a factory and seven clients.

**The seventh was false, and it is a regression against walk 3.** The catalog claimed a group of routes did not select or invoke a provider. One of them does — it accepts a provider name and a client is constructed from it downstream. Walk 3 had handled that corner correctly by naming it explicitly; walk 4 flattened it into a negative it had not searched for. It is the fourth defect the negative rule has caught, and the first inside a map built under the current rules.

It also found where the rule did not reach. The same paragraph excluded another region *with* its search attached, one sentence later — so the discipline was live and simply did not cover this. Pass 3 is scoped to nouns that get cards, and Pass 5's negative check said *in every card*; the catalog is not a card, and its *What is not carded* list is nothing but negatives. Both now name the catalog explicitly. Two words and a clause, no new rule.

Not published. The territory belongs to someone else and the findings read as defects; a map is not an audit, but that distinction is easier to hold when the code is your own.

### Refusal test — passed on two territories

- **Pointed at this folder:** refused. A methodology folder is out of scope by `identity.md`.
- **Pointed at a generated archive** — a folder of 138 markdown files, each one produced by a script from session logs that live elsewhere, overwritten whenever the script re-runs: refused at Pass 0 as a rendering. Changing those files does not change the system, and a reader acting on such a map would edit output that is regenerated on the next run.

This second case is the one worth having. It sits *inside* the published territory, it looks exactly like mappable material, and the test that catches it is behavioural rather than cosmetic.

### Cold-read test, round 1 — the vault map, failed once then passed

The bar: a fresh reader, given *only* `catalog.md` and one card, answers "what is this and what moves if I change it" and **stops without requesting more files**.

**Run 1 failed.** On the walk-2 vault map, not the published one. Given its lifecycle card, the reader opened eleven further sources from the live territory and came back with a better answer than the card contained. It was right to: the card claimed nothing scanned a pair of folders, and something did. The claim had been false since the day it was written — nothing decayed, the survey never looked.

Two defects came out of that, and both are now closed:

- **An unanchored negative.** `rules.md` demanded a named reader at a named path before writing `live`, and said nothing about the opposite claim. Pass 3 now requires the same evidence for a negative, and since you cannot cite an absence, it requires naming *where you looked*. Applying it did not merely rescue the claim — it produced a sharper one: the folder has exactly one scanning reader, and five other procedure files that name specific files inside it by path and would never see a new arrival.
- **A catalog header that told the reader to go verify.** That instruction collided with the load budget, and the reader said so unprompted. It was never part of the discipline this map inherited — the rule is *precedence*, deciding who wins when a disagreement surfaces, not an errand. The header now states it that way.

**Run 2 passed.** Same question, same fixture, fresh reader. It opened the catalog and one card, then stopped — and gave its reason: the second card *"would have bought confirmation, not information, at the cost of the stated budget."* It also declined to check the territory, noting the survey date and flagging the numbers as survey-day facts.

The `Does not hit` line did the work. The question contains a false premise — that moving a file between status folders makes sessions treat it as active — and the wrong-neighbour line pre-empted it, which is why one card was enough.

### Cold-read test, round 2 — failed twice on the recarved map

Two fresh readers, neutral working directory, no instructions beyond a user's question and the path to the map. The territory was reachable, deliberately.

**Both blew the budget.** One read the catalog, every card and four territory files; the other abandoned the map entirely and went to the process table. The first got the answer right — including the ghost — but only after reading everything, and it surfaced a `README.md:3` reference the card had missed. The file won; the card is fixed.

Four causes, and the fix for three of them was to delete something:

- **The budget never named the territory.** It forbade "plans, notes and issue threads about this territory" — a list of documents *about* the place, which reads as leave to read the place.
- **"The file wins" sat four lines from the load budget and read as permission to verify.** Restated as precedence, with an explicit *this is not an errand*.
- **`see other-card.md` pointers inside `Does not hit` lines.** A reader treats a pointer as an instruction. They are now forbidden outright rather than merely discouraged.
- **The triangle rule required the wrong neighbour to be carded so the reader could check it** — which is a second card, which is the failure the whole form exists to prevent. It was the only rule here that worked against the one rule, and it is cut. A negative is verified by the search printed inside the card.

### Cold-read test, round 3 — three runs on the five-card map

Neutral working directory, fresh reader each time, the territory not mentioned. One run walked correctly — catalog, one card, no territory — and two did not. What they exposed is worth more than the score.

**All three ran a recursive text search across the map folder before reading anything.** Not "opened a second card hoping" — a tooling reflex that swept every card in one action. That is a limit this form cannot fully close: **the load budget is stated inside `catalog.md`, which you can only reach by an action that may already have violated it.** There is no doorplate on the outside of the building. The budget is now the catalog's first line, which is the most a document inside the folder can do.

The honest reframe is that the budget is a discipline the map declares, not a fence it enforces — and **the map's real protection is being small.** In the worst run the reader swept all 97 lines of it instead of reading 459 lines of territory. Total budget failure still cost about a fifth of the source.

**Two findings against the map itself:**

- **A task-shaped question defeats it, and probably always will.** The catalog names *"find out what it did last night"* as an out-of-scope example. Asked exactly that, the reader went and found out — territory, process table, system log. A map cannot out-instruct the person asking it. The claim that it refuses tasks is now written as a limit below, not as a behaviour.
- **The run that obeyed the budget gave a wrong answer, because a card was wrong.** Asked what to change for a second GPU, it answered from one card and said `config` was not involved. `config:48` is `fan2gpu`, the fan-to-GPU map, read at `temp.sh:174`. The card had asserted config held no device setting on the strength of a grep for interface words that never looked for topology words. An under-searched negative, propagated verbatim into user-facing advice. Corrected, and it is the third defect the negative rule has caught.

**The map has not been re-run since that correction.** That is the honest state.

### Slurp test — clean

The folder was searched for any instruction to load everything, read the whole map, or add every file to a project. There are none, in either the method files or the worked map. Every occurrence of that language is a prohibition.

## What it cannot know

Stated plainly, because the limits are the point.

- **The cold-read test passed on the second attempt, not the first.** The failure is written up in full under **round 1** above, and it is what produced the rule about negatives in `rules.md`. **n=1 on the passing run**, one question, one card. It has not been run across every card or every question shape.
- **The published map has failed more cold reads than it has passed.** Five runs across rounds 2 and 3; one clean walk. Every failure is written up above with what it changed — the triangle rule is gone because of round 2, the budget sits first in the catalog because of round 3.
- **The evidence base is five territories, and one of them was not the author's.** A third-party repo they had never opened, cloned at a pinned commit: two of the current rules came out of it, and its map was then checked against the source line by line — **six of seven claims verified exactly, and the seventh was false**, written up above as a regression. That is the sharpest measurement in this repo, and it is the one taken on ground with no memory attached. **What it is not is an independent operator.** Every walk was run by the author. That is a larger gap than the territory count, and it is not closed by adding more territories.
- **This is a map, not an assistant, and it cannot enforce that.** It answers *what is here* and *what moves if I change it*. Asked instead to get something working, it is the wrong instrument — but saying so in the catalog does not stop a reader, and testing showed it does not: given a task the catalog names as out of scope, the reader did the task. Treat the boundary as a description of what the map is good for, not a refusal it can make stick.
- **The folder is better evidenced than its output.** Maps produced by this method have been checked closely; whether those maps then get *used well* rests on a handful of sessions. One walked cleanly. One found no matching card and read a second card hoping. One drifted outward into the author's own planning notes. The last two were caused by gaps since closed — a catalog that never said what it had left out, and a load budget bounded only against reading more cards — but the usage record is thin and is not improved by asserting otherwise.
- **`Open` lines are only as honest as the cartographer writing them.** Nothing in the method forces an uncertainty to be noticed — the line is required, but a confident wrong card will fill it confidently.
- **The map does not track the territory after the survey.** A `live` status is a claim about the day it was written, and the catalog carries that date so a reader can weigh it. Nothing re-verifies it afterward, and nothing needs to: **re-running the cartographer is the maintenance path**, and it costs one session rather than one excavation. What the map owes you is an honest survey and its date. Deciding whether that date is fresh enough to act on is yours.
- **`Hits` is first-order only, by design.** Deeper waterfalls are guesses dressed as maps. This is a deliberate limit and it means the map will understate the blast radius of a large change.
- **It cannot map what it cannot read.** Binaries, encrypted stores, and anything behind an API are invisible, and the map will not mention their absence unless something inside the territory points at them.
- **A map is not an audit.** It says what is there, not whether it is any good. If you want to know why something failed, this is the wrong instrument.

## Licence

MIT.
