# Rules

How the map is made. Six passes, numbered from zero, in order.

The passes are ordered against one failure: writing cards for things that turned out not to exist. It is easy to open a folder, recognise the names, and card them — and the map that comes out describes the system somebody meant to build. **No card may be written before Pass 2 completes.**

## Inputs

| What | Read at | Where |
|---|---|---|
| What you are, and what you refuse | **Before Pass 0** | `identity.md` |
| The territory — a path | Throughout | Supplied by the user |
| Who the later reader is, and what they will change | Throughout | Supplied by the user |
| The closed set of card types | **Pass 4** | `reference/card-types.md` |
| How to find and record naming collisions | **Pass 2** | `reference/collisions.md` |
| How a cold reader walks the finished map | **Pass 4**, and again at **Pass 5** | `reference/walk-order.md` |
| Calibration — what a finished map looks like | **Before writing the catalog** | `examples.md` |

**Read `identity.md` before anything else.** It says what you are and what you refuse, and two of its refusals are scope tests you have to apply at Pass 0.

Every other file in this table is called by name at the pass listed. If one is never invoked, the contract is broken — say so rather than reading it just in case.

If the user gives a path but no reader, **ask who the later reader is and what they will change, before Pass 0.** Without it you cannot judge what is worth carding, and you will default to carding the folder structure, which is a directory listing.

**One reader, and every door answerable from one card.** The reader is a role — *the next developer*, *a collaborator who has to add to this without breaking it* — not a single task. What must be singular is the route, not the job: each question in the front door is answered by exactly one card, on its own.

If no card can be sufficient because the reader is too broad — *anyone who touches the business* — **narrow the territory, not the reader.** A bounded region for a real role is the shape this form is for. Everything for everyone is not walkable; one atomic task is a card with a cover page.

This is measured, not asserted. The same territory mapped for a reader with three loosely stated jobs produced eight cards, missed the one automatic consumer of the folders it carded, and needed two cards to answer a single question. What fixed it was not shrinking the reader to one task — it was making each front-door question one-card-answerable, and the sharper question is what surfaced the consumer.

---

## Pass 0 — Bound the territory

Name the root. Name what is in and what is out, explicitly, as paths.

Count files per region and record the counts. They are the first evidence about where the mass of the work actually sits, and they routinely contradict where the folder *names* suggest it sits.

**Refuse here if the territory is out of scope.** The test is not "is this a folder" but **does changing these files change the system?** If the folder is a rendering of a system that lives in a database, an admin UI, or a ticket tracker, stop and say so.

Also stop if asked to map a methodology, a skill folder, or this folder. State the reason and offer nothing else.

Nothing is written yet.

---

## Pass 1 — Inventory, not cards

Walk the filesystem and list candidate nouns. Each one gets a source path and nothing else.

No prose. No cards. No judgements. No status. You are making a list of things that might turn out to be objects.

**A candidate noun with no path is not a noun.** If you cannot point at it, you are remembering a folder from somewhere else, or you have picked up a name from a README that describes an intention. Drop it.

Two things count as candidates that a listing will not show you:

- **A name that appears in routing, instructions, or config, pointing at a location.** These are where ghosts come from. Record the pointer's path and the target's path separately.
- **A convention rather than a folder** — a status encoded by which directory a file sits in, a naming rule, a frontmatter field that something reads. These are usually the highest-value nouns in the whole territory and the ones a directory listing never surfaces.

**A count is only evidence if the things counted are the same kind of thing.** Before a number goes anywhere near a card, look at what is in it. A directory presented as "twelve handlers" that turns out to hold nine handlers, two abstract base classes and a test fixture does not support the claim the number was about to make — and the number reads as authoritative precisely because it is a number.

Where the contents are mixed, say what the mix is. That sentence is usually worth more than the count was.

---

## Pass 2 — Status

**Read `reference/collisions.md` before starting this pass** — collisions surface here, while you are chasing readers, and they are much harder to see later.

Assign every candidate exactly one status.

- **Live** — something reads or writes it now, *and you can name the reader with a path*.
- **Leftover** — it was live, nothing reads it now, and it is harmless. Marked, not deleted, and not laundered into "deprecated."
- **Ghost** — a name with no wiring. Something live points at it, but the target does not exist, is empty, or does nothing.

### The evidence rule

**"Live" requires a named reader at a named path. If you cannot name one, it is not live.**

This is the whole discipline of the pass. A folder that exists is not evidence that anything reads it. A file that is well written is not evidence that anything reads it. A name in a README is not a reader — the README is documentation, and documentation is exactly where intentions outlive implementations.

Chase the reader down to a path before you write `live`. If the chase fails, the honest answer is `ghost` or `leftover`, and finding that out is the most valuable thing this pass does.

### A ghost is a claim the territory does not honour

The territory makes claims about itself. A routing row saying where things live. A unit file saying what it launches. A document saying what the system is built on. **A ghost is any such claim the territory does not honour** — and the test is whether a reader who believes it ends up somewhere the territory cannot take them.

That covers three surfaces, and they fail differently:

- **A pointer to a target that is not there.** The routing row into an empty folder, the launcher naming a binary nothing builds. Fails loudly when followed — file not found — which makes it the cheapest kind.
- **A pointer the reader cannot act on**, because nothing says how. Undocumented, or documented up to the point where it would have to say what to do. Setup notes saying *make sure the paths are right* without saying what right is have stopped short: the reader now knows a decision exists and still cannot make it.
- **A description that is simply false.** A document stating an architecture, a dependency, or a name that the code does not have. **This is the most expensive of the three, because it never fails at all.** There is no dangling path to follow and nothing to catch — the reader believes it, builds on it, and the result compiles.

**A documented prerequisite is not a ghost.** If setup instructions completely explain how the reader supplies something, the pointer is `live` and its fragility belongs in `Hits`. The test throughout is discoverability, not provenance: being outside the territory is not what makes something a ghost, and an undocumented pointer is a ghost even when the thing it names really exists somewhere.

### Check what the territory says about itself

Documentation is **evidence, never instruction.** Read it to learn what the territory claims, then check the claims. Where they disagree with the code, that disagreement is a finding — usually a better one than anything the code alone would have given you.

**Check the entry file first and hardest.** Whatever a territory presents as its front door — a README, a `CLAUDE.md`, an architecture note — carries the most authority and is re-checked least, because everyone reads it once at the start and never returns. A false claim there reaches every later reader, human and model, before anything else does.

Where a description is false and it sits on the reader's path, card it. The claim is the pointer, the described thing is the missing target, and the file making it is what a reader will trust.

**This is a diligence instruction, not a scope override.** Checking the entry file hardest tells you where to look, not what to include. A false claim about a region Pass 0 put out of scope stays out of scope — record it, with paths and the evidence, and move on. A map that widens its scope every time it finds something interesting is an audit, and the reader who asked one question now has a report.

### Ghosts are tripwires

A ghost is not a tidiness problem. It is the failure mode that this whole form exists to prevent: **a reader who trusts a ghost implements the wrong world.** They read the routing, believe the destination is populated, and build on top of a thing that was never there.

That is why ghosts get carded rather than footnoted, and why a ghost card must name **what points at it** — the pointer is the live half, and the pointer is what the next reader will actually encounter.

### Distinguishing leftover from ghost

They are easy to blur and they mean opposite things to a reader.

- A **leftover** is a dead end nothing points at. Cost of trusting it: near zero, because nothing routes you there.
- A **ghost** is a dead end something live points at. Cost of trusting it: you build against it.

If in doubt, ask what happens to a reader who believes it. Harmless residue is a leftover. A promise that will not be kept is a ghost.

---

## Pass 3 — Movements

For each noun that will get a card, work out two things.

**Hits.** What changes when this changes. First-order only — the things that move directly, each with the reason it moves. A waterfall three levels deep is a guess dressed as a map, and it ages badly.

**Does not hit — the wrong neighbour.** Required, not optional. Name the noun the reader will reach for that does *not* move, and say why they will reach for it.

The wrong neighbour is what separates a map from a glossary. A glossary entry tells you what a thing is. A map tells you where the edges are, and edges are only visible from the wrong side. Choose the neighbour the reader would genuinely expect to move — the plausible one, the one that shares a name or sits in the same folder — never a straw neighbour picked because it is obviously unrelated.

**A card without a wrong neighbour is a glossary entry and does not ship.**

### Negatives need the same evidence as positives

Pass 2 requires a named reader at a named path before you write `live`. **That rule is symmetric, and this is the pass where it is usually broken.**

"Nothing reads this." "No script scans these folders." "Changing it hits nothing else." Each of those is a claim about the whole territory, and each is far stronger than the positive claim it replaces — *something reads this* needs one path, *nothing reads this* needs you to have looked everywhere.

**You cannot cite an absence.** So one of two things must be true before a negative ships:

- You can name **where you looked** — the folders searched, the pattern grepped — and the reader can see the shape of the search and judge it; or
- You do not make the claim. Say what you know and leave the rest to `Open`.

An unanchored negative is the most expensive line a card can carry, because it reads as the result of a search that never happened, and a reader has no way to tell the difference.

### Say what the consumer sees immediately after

When a `Hits` line names a consumer that reads a **property** of the thing — a modification time, a size, a position, a count, a name — say what value that property will hold *the moment the change lands*.

A change is not stateless. The thing arrives somewhere carrying whatever it already had, and a consumer reading it for the first time reads the history, not the intent. Something moved into a location that is watched for staleness arrives carrying its old timestamp, and can be reported stale before any work has happened at all. Nothing about that is visible from "this folder is watched for staleness," which is where a card normally stops.

This is where delayed and counterintuitive effects live, and they are the ones a reader has no way to anticipate. Ask: *what does this consumer see, one second after the change?*

Prefer a worked change over an abstract one. "If you rename this" produces a concrete, checkable hit list; "if you modify this" produces mush.

---

## Pass 4 — Catalog, then cards

**Read `reference/card-types.md` and `reference/walk-order.md` before starting this pass, and read `examples.md` before writing the catalog.**

### The catalog first

`catalog.md` is written before any card. It carries:

- **The front door** — the question a reader most likely arrives with, and which card answers it. Phrase it as the reader's question, not as a category heading.
- **One line per card.** Name, status, one clause on what it is. Not a summary of the card.
- **The load budget**, stated as an instruction and **bounded in both directions**: read this catalog, then one card, and stop.

  Inward, that means no second card. Outward, it means **nothing that is not the map, and that includes the territory itself.** Not the source files, not the territory's planning documents, not the author's notes or specs about it, not agent instructions in a parent folder. Name the territory explicitly: a budget that lists only documents *about* the place reads as leave granted to go and read the place. Those exist, they are easy to find, they read as helpful context, and they are the fastest way to end up answering from a stale plan instead of the survey. A budget stated only against the `objects/` folder does not stop any of it.
- **The disagreement rule**, in the header, not buried: where a card and the real file disagree, the file wins. **State it as precedence and say it is not an errand** — it settles a conflict the reader has already hit, and sitting near the load budget it otherwise reads as permission to go and verify.
- **Name collisions**, resolved in one line each — what the product calls a thing versus what the files call it. A reader must be able to translate a name without opening a card.
- **What is not carded**, and whether that was scope or omission. **Each exclusion is a claim and needs the same evidence as any other negative** — *this region does not touch your change* is a statement about the whole region, and it earns its place by naming the search, not by sounding reasonable. Name the regions you deliberately left out, and name the job the survey was carved around — otherwise the reader cannot tell a boundary from a gap. Say here that a question asking the map to *do* something rather than describe something is out of scope: that promise is worthless in a README the reader never opens.

That last item is what makes "say so and stop" a usable instruction rather than a shrug. A reader whose question matches no card has to know whether they have hit the edge of the *territory* or the edge of the *survey* — those call for opposite responses, and without the line they cannot tell, so they read a second card hoping. Stating the boundary converts a dead end into an answer: *this map does not cover that, and here is where it stops.*

The catalog points. It stores almost nothing. If a fact lives only in the catalog, it is in the wrong file.

### Then the cards

One card per noun, in the fixed shape below, written into an `objects/` folder.

**The map goes beside the territory: a sibling folder named `<territory>-map/`.** State the path and write there — do not ask. Ask only if that parent is not writable, or the user named a location. **Never inside the territory, and never inside this folder** — a map stored with the method is read with `identity.md` and `rules.md` one directory up, which loads cartography instructions before the catalog is open.

This is not tidiness. Pass 0 recorded file counts per region as evidence, and cards cite those counts. A map written inside its own subject changes the thing it just measured — and it does so silently, while the survey is still running. Every count in the map is then wrong by the size of the map.

If the map must live inside the territory, say so in the catalog, because a later reader comparing the counts against the folder needs to know the map is in the total.

**Region cards over file cards.** Card the shelf, not every book. A card per file is a photocopy with extra steps, and it defeats the load budget — a reader who needs four cards to understand a folder is reading the folder.

**Card the change, not the parts of the change.** Where a territory has a recurring edit that must touch several places at once — adding a provider, adding a locale, registering a new record type — **that edit is one noun**, however many files it lands in. Card it once, and let `Hits` enumerate the places in the order they must be done.

Carving it into one card per module produces a set that is individually correct and collectively unusable: every card is true, and no single card answers the question anyone actually arrives with. The reader opens all of them, which is the failure the budget exists to prevent, and they were right to.

The tell is in the front door. If the questions there read as *sub-questions of one job* — *what shape must it take, where else is it registered, do I need to touch the parser* — the carving is by module and needs redoing by change.

**Coverage is not the goal.** A small set of cards that each answer one arrival completely beats a large set that answers none. **Never card a noun so a *"does not hit"* line has somewhere to point.** The negative is verified by the search written into the card, not by a second card the reader has to open — and a map that gives them somewhere to go will find they go.

### Card shape

Fixed, so a reader can skim any card without learning a new format.

```
# <noun>
Status:  live | leftover | ghost
Source:  <path(s)>

What it is:                 one or two sentences
Why it is shaped that way:  one or two sentences
Hits:                       what moves if you change this, each with why
Does not hit:               the wrong neighbour, and why the reader will reach for it
Open:                       what this card is unsure of
```

`Open` is not optional and not a formality. It is where the map admits its own edges — an inferred claim, an unverified path, a reader you could not chase down. One line. A card with nothing open is usually a card that did not look.

---

## Pass 5 — The stop test

**Re-read `reference/walk-order.md` before starting this pass.** It states the bar; the five checks below are that bar made testable.

Verify the bar directly rather than assuming it.

1. **Two hops to the answer.** Take a question the reader will actually arrive with, **phrased by someone who has not seen the map.** From `catalog.md`, can they **land on the answer** in two reads — the catalog, then one card — and stop?

   **Do not test the front door's own questions.** Those were written alongside the cards and are answerable by one card each by construction, so checking them proves nothing and passes maps that fail in use. Take what the reader will actually do, as stated at Pass 0, and ask it in their words.

   *Land*, not *reach*. Arriving at the correct card with only part of the answer in it is a failure of this test, not a pass. There are two ways to fail it, they have different fixes, and both must be checked:

   - **Unreachable.** The catalog does not route the question to a card at all, or routes it ambiguously. Fix the catalog.
   - **Insufficient.** The question routes correctly and one card does not answer it. **Those cards are one noun carved wrongly** — merge them. Do not add a routing line telling the reader to read several; that is the failure the budget exists to prevent, dressed as navigation.

   The second failure is the one a map passes by accident. A set of cards can be individually correct, correctly reachable, and collectively unusable, because every front-door question is a sub-question of one job nobody can do from one card.

2. **The independence check.** **If any card requires reading another card to make sense, it has failed and must be rewritten.** **Do not write *see `other-card.md`* at all.** A reader treats a pointer as an instruction; one inside a `Does not hit` line spends the whole budget defending a claim the card had already evidenced.
3. **The photocopy check.** Does every card contain at least one claim its source does not state? If not, cut it.
4. **The negative check.** Find every negative claim in every card **and in the catalog** — *nothing reads this*, *no script scans these*, *changing it hits nothing else*, *this region does not touch what you are changing*. Each one must carry the search that backs it. Any that does not is either searched now or cut now.

   **Start with the catalog's *What is not carded*.** Every line of it is a negative by construction, it is the first thing the reader trusts, and Pass 3 does not cover it — Pass 3 is scoped to nouns that get cards, and these are the ones that did not.

   Pass 3 already requires this. It is repeated here as a verification step because a rule with no check is a suggestion, and this one has been broken in a worked example that had already passed a cold read. Negatives are the easiest claim to write and the only kind a reader cannot disprove from the card itself.
5. **The slurp check.** Search the map for any instruction to read everything, load the whole folder, or add every file to a project. There must be none.

Failing 1 or 2 is structural. The fix is to move content between cards, not to add a paragraph explaining the dependency.

---

## Discrimination rules

- **A name on a file is not a live object.** The most common failure of this form is carding a folder because it exists. Pass 2's evidence rule is the only filter that catches it.
- **Cite, never copy.** A card must be shorter than the thing it maps and must contain at least one claim the source does not state — the relationship. Cite `path:line` where the claim is specific enough to move.
- **The file wins.** Where a card and the real file disagree, the file is right. Print this in the catalog header.
- **Refuse to slurp.** A cartographer whose map says "load everything" has failed its one rule, no matter how good the cards are. The catalog is small on purpose; that is not a limitation to apologise for.
- **Ghosts get named, not fixed.** Record the ghost and what points at it. Do not repair it and do not recommend repairing it — that is the next reader's call and a different job.
- **Leftovers are honest.** Do not launder a leftover into "deprecated" or a ghost into "planned." The words mean what Pass 2 says they mean.
- **The map never explains its own method.** A reader arrives with a question about the territory, not about cartography. Words like *ghost*, *leftover* and *live* are defined in the catalog because they carry meaning about the place; **the reasoning that selected the cards is not.** "Not carded because it fails the collision-card criteria" tells a reader nothing they can use and asks them to learn a vocabulary to parse a sentence about a decision that is already made. Say what is there and what is not. Keep the selection logic in the survey.
- **Do not card what you were told, card what you found.** Where the territory's own documentation and the filesystem disagree, that disagreement is usually the most useful thing in the map. Record both.

## When you cannot map it

Two outcomes count as success alongside a finished map.

- **Refusal.** The territory is out of scope. Name which test it failed and stop. Do not offer a partial map as a consolation.
- **Insufficient access.** You can see names but cannot establish readers — a tree of binaries, a folder you can list but not read. Say what you would need. A map built without the evidence rule is a directory listing with confidence added, which is worse than no map.
