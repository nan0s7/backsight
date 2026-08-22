# Walk order

**Called at Pass 4** by `rules.md` when writing the catalog, and again at **Pass 5** to verify the finished map.

This file describes how the *produced* map is walked. It is the standard the map is built to meet, which is why it is read before writing the catalog rather than after.

## The walk

```
catalog (front door)  ->  one card  ->  stop
```

**Three steps, two reads, and the third step is stopping.** A reader who does not stop has not been served by the map; they have been given a folder with a table of contents.

**Catalog — read one.** Small, stable, points at everything, stores almost nothing. It resolves name collisions in place, because a reader who cannot translate a name cannot pick a card. Its **front door** is the part that routes: the reader arrives with a question, not a category, and the front door converts the question they actually have into the name of one card. Phrase it as their question — *"what is a live commitment in here and what is finished?"* — not as a heading like *Overview*.

**One card — read two.** The card answers what the thing is and what moves if they change it, on its own.

**Stop.** With the answer, without having opened the folder.

## The two-hop rule

**Arrival to answer is two reads: the catalog, then one card.** That is the budget.

Two hops is not a stylistic preference. It is what makes the map cheaper than the territory. A map that takes five reads to answer a question has been beaten by grep.

If a card needs a second card to make sense, the two-hop rule is broken and the fix is structural — move the content into one card or split the noun differently. Do not add a "read this first" note. That is a third hop wearing a hat.

## The load budget

**The map must state its own budget, in the catalog, as an instruction:** read the catalog, then one card, and stop.

State it because the default behaviour of both humans and models, faced with a folder of small files, is to read all of them. The budget has to be written down or it does not exist.

**Never write, and never imply:** load everything, read the whole `objects/` folder, add every file to the project, ingest the map before starting. A cartographer whose map says any of these has failed its one rule. The catalog is small on purpose — that is the design, not a gap to be filled by reading more.

## How a model walks it, specifically

The later reader is usually a model with no memory of the place, and it differs from a human reader in ways the map has to absorb.

- **It cannot ask a follow-up.** Anything the map leaves implicit is simply lost. This is why `Open` is a required line: an unstated uncertainty reads as a stated fact.
- **It will believe the map.** A human notices when a folder does not match its description. A model reading the map instead of the tree has nothing to notice with. Hence the disagreement rule in the catalog header — the file wins — which tells it which way to resolve a conflict before it hits one.
- **It defaults to reading everything.** Left to itself it will open the whole map, spend its context on the catalog's neighbours, and arrive at the actual work with nothing left. The budget is aimed squarely at this.
- **It reasons from names.** A name that lies is more expensive for a model than for a human, because the human has ambient memory of the place and the model has only the name. This is why collisions are resolved in the catalog, at the point of arrival.

## When the question matches no card

**Say so and stop.**

Do not read more cards hoping one turns out to be relevant. Do not synthesise an answer from adjacent cards — an answer assembled from two cards about neighbouring things is a guess with citations attached, and it is indistinguishable from a real answer at the point of use.

The honest response names what the map does cover and what it would take to answer the question: usually reading the territory directly at a stated path.

**A map that covers four things well and says so is more useful than one that gestures at everything.** Coverage was never the promise. The promise is that where the map speaks, it is right, and where it does not, it says nothing.
