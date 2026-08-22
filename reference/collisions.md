# Collisions

**Called at Pass 2** by `rules.md`, while readers are being chased. Collisions surface there and are much harder to see afterward, because by Pass 4 you have learned the territory's names and they have stopped looking strange.

This file is the **method** for finding and recording collisions. It is not a list of any one territory's collisions — a produced map carries its own, in its catalog, where a reader hits them on arrival rather than after a second read. A cartographer that only knew one territory's collisions could not be pointed anywhere else.

For a worked instance — four real collisions found in one territory, and where each one ended up — see the catalog and the worked change in `examples.md`.

## What a collision is

A place where a name will cause a reader to act on the wrong thing.

Not a name you dislike. Not an inconsistency. The test is behavioural: **what does a reader do wrong because of this name?** If you cannot state the wrong action, it is not a collision and it does not go in the map.

Collisions are the highest-value thing a cartographer finds, because they are invisible from inside. The person who built the place knows that `temp.sh` is temperature. Everyone else, and every model, gets it wrong on first read and some of them get it wrong on the tenth.

## The four classes

### 1. One name, two jobs

The same word means different things in different parts of the tree. `tmp` holding four unrelated things. `config` for both user settings and build settings. A `status` field that means workflow state in one folder and health in another.

**Cost:** a reader carries the meaning they learned first into a place where it is wrong.

**Record as:** the name, each job, and each path. Do not pick a winner — you are mapping, not renaming.

### 2. Two names, one job

Two files or paths doing the same work, with nothing keeping them in step. Duplicated entry files are the classic case: two documents that must stay identical, no generator, no link, no test.

**Cost:** they drift, silently, and the reader who edits one has no way to know the other exists. This is the class most likely to earn a card rather than a catalog line, because the danger is not confusion at read time but divergence over time.

**Record as:** both paths, whether they currently agree, and what keeps them in step — usually the answer is *nothing*, and that is the finding.

### 3. A name that lies about its content

The name is a real word that means something else here. `temp.sh` means temperature, not temporary. A `notes` folder holding imported third-party material rather than the owner's notes. A folder named for a status that stopped tracking status.

**Cost:** the highest of the four for a model, which reasons from names and has no ambient knowledge to correct with. A human eventually opens the file and is surprised. A model may never open it — it may route around it, or route *to* it, on the strength of the name alone.

**Record as:** what the name says, what is actually there, and what a reader would do on the strength of the name.

### 4. A name that outlived the thing it named

The name was accurate once. The tool was replaced, the process changed, the product was renamed, and the path stayed. A directory named for an application nobody uses. A field named for a workflow that was retired.

**Cost:** subtle and cumulative. It misroutes new readers permanently, and it makes the territory look like it is doing something it stopped doing. It also tends to be load-bearing in ways nobody remembers — the dead name is often embedded in absolute paths elsewhere, which is why these frequently surface again during a worked change.

**Record as:** the name, when it stopped being true (if you can tell), what it names now, and — importantly — whether anything still depends on the literal string. A dead name that nothing depends on is cosmetic. A dead name embedded in a path is structural.

## How to find them

Collisions do not announce themselves. Four moves that surface them reliably:

1. **Compare names against contents, deliberately.** For each candidate noun, ask what a stranger would guess from the name alone, then check. Class 3 falls out of this and nothing else finds it.
2. **Diff things that look like twins.** Any two files with similar names or similar jobs — actually run the comparison rather than assuming. Class 2 is invisible until you do.
3. **Grep the dead names.** When you suspect class 4, search the tree for the literal string. This is how you learn whether the dead name is cosmetic or structural, and it usually turns up dependencies outside the territory.
4. **Notice your own first mistake.** The first thing you got wrong when you opened the folder is a collision, and it is the one you will forget fastest. Write it down in Pass 1 while it is still strange.

## How to record them

Most collisions are **one line in the catalog**, at the point of arrival: *what the product calls it, what the files call it.* The reader needs the translation before they pick a card, so it cannot live inside a card.

A collision earns its own card when the problem is structural rather than lexical — when it is not "you will be confused" but "you will change one of these and not the other."

**Do not editorialise.** No "this should be renamed," no assessment of naming quality. A collision entry says what the name is, what it means, and what a reader will get wrong. The territory's owner had reasons, the reasons are usually historical, and the map's job is to stop the next reader tripping — not to litigate the name.
