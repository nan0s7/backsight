# Card types

**Called at Pass 4** by `rules.md`. Four types. The set is closed.

It is closed on purpose. An open set produces a taxonomy — a reader learns the classification scheme instead of the territory, and the cartographer spends its effort deciding which of eleven types something is. If a noun does not fit one of these four, that is evidence it should not be carded, not evidence a fifth type is needed.

Every type uses the same card shape from `rules.md`. The type changes what the card is *for*, not what it looks like.

---

## 1. Region card

**For:** a shelf. A folder, a cluster, a body of files that a reader thinks of as one thing.

Card the shelf, not every book. A region card says what kind of thing lives here, roughly how much of it there is, what reads it, and what moves if the shelf moves.

Use it when the individual files are interchangeable in kind — fifteen notes, a hundred and thirty archived sessions, a directory of templates. The reader does not need to know each one; they need to know what the shelf is and whether their change touches it.

**Never:** enumerate the contents. A region card that lists its files is a directory listing, and the directory already does that better and stays current.

---

## 2. Noun card

**For:** a single object with its own identity — a specific file, a convention, a mechanism.

The most valuable noun cards are usually **not files.** A status encoded by which folder a file sits in, a naming rule, a field something reads — these are real objects that a directory listing cannot show, and they are exactly what a reader breaks by accident, because there is nothing on disk to warn them.

Use it when the thing has behaviour of its own: something reads it, and changing it changes what happens.

**Never:** restate the file. If the card and the source say the same things, the card is a photocopy. The card's job is the relationship — what reads this, what moves — which the source file almost never states about itself.

---

## 3. Ghost card

**For:** a claim the territory makes and does not honour — a pointer to something absent, a pointer nothing explains how to satisfy, or **a description that is simply false**. A fully documented prerequisite is `live`, not a ghost; see the status rules in `rules.md` Pass 2.

The false-description case is the one most often missed, because there is no broken path to trip over. It is also the most expensive: the reader believes the document, builds on it, and nothing fails.

The ghost card's centre of gravity is the **pointer**, not the absence. Anyone can see an empty folder. What the reader cannot see is that three live routing rows send them there, and that is the thing that will cost them.

So a ghost card carries the extra `What points at it` field defined in `rules.md`, and names in it, with paths: what makes the claim, what a reader would expect from it, and what is actually there.

A ghost is a tripwire. A reader who trusts it implements the wrong world — builds against a destination that was never populated, or assumes a step exists because something referred to it.

**Never:** fix it, or recommend fixing it. A ghost card that ends in "this should be cleaned up" has stopped mapping and started consulting. Record it and move on; the repair is the next reader's decision and needs context the map does not have.

---

## 4. Collision card

**For:** a naming problem serious enough that a reader will act on the wrong thing. The four classes and how to find them are in `collisions.md`.

Not every collision earns a card. Most are one line in the catalog — *"Chat is `Incubator`"* — and that is the right treatment, because the reader needs the translation at the moment of arrival, not after a second read.

A collision earns a card when the wrongness is **structural rather than lexical**: two files that must stay in step and have nothing keeping them in step, a name that outlived the thing it named and now misroutes every new reader, one name doing two jobs in different parts of the tree.

**Never:** turn it into a style complaint. The card is about what a reader will do wrong, not about whether the name is good. If you cannot state the wrong action, there is no collision worth carding.

---

## Choosing between them

In order:

1. Does something live point at a thing that is not there? → **Ghost card.**
2. Will a reader act on the wrong thing because of a name? → **Collision card**, if structural; otherwise one catalog line.
3. Is this a shelf of interchangeable things? → **Region card.**
4. Does it have its own behaviour, and does something read it? → **Noun card.**
5. None of these? → **Not a card.** Leave it out. A territory has far more nouns than a good map has cards, and the ones that get cut are the ones nothing reads and nobody will change.
