# Examples

One worked map, reproduced whole: the catalog, five cards, and a note on how the busiest one was carved.

**Read this before writing a catalog.** It is calibration, not illustration — the thing being calibrated is how much a card leaves out.

---

## The territory

`nfancurve` — a POSIX shell script that drives Nvidia GPU fan curves on Linux. Six files, one branch, 459 non-blank lines excluding the licence. Public, and written by the author of this folder in 2018. Dormant since a pull request from a stranger merged in January 2023 — nothing about it has stopped working, which is what the Pass 0 test actually asks.

**The later reader:** someone who found this repo, saw the last commit was years ago, and wants to know what they are dealing with before they touch it. Orientation before the work, not the work — the map hands over the seams and does not do the port. In practice, often a model.

It is small on purpose. A map earns nothing on a territory you can read in one sitting *unless* the territory misleads you, and this one does: its most important name means the opposite of what it looks like, and the one file a reader opens before renaming a script is the one file that does not care.

**Map size: 97 non-blank lines against 459.** Twenty-one per cent.

**This map also ships walkable**, at `nfancurve-map/` — the same catalog and the same five cards, as separate files. To test the two-hop walk, open `nfancurve-map/catalog.md` and nothing else. It is reproduced here in full because a worked map belongs in this file; the two are the same text.

## What this map does not demonstrate

Two of the four types in `reference/card-types.md` have no instance here, and the reason is the territory rather than an omission. `nfancurve` is flat — six files, no shelf — so there is no region to card. Its naming collisions are all lexical, so they are catalog lines, which `card-types.md` says is the right treatment.

There is also no `leftover`, and the survey looked: all twelve functions defined in `temp.sh` are called, and all twelve config keys are read. `hyst` appears unread until you notice `temp.sh:129` uses it inside `$(( ))` without a sigil, where a naive grep misses it. A territory with no leftover gets no leftover card. Do not manufacture one to complete the set.

---

# catalog.md

> **Load budget: this catalog, then one card. Stop.** Not a second card, not the `objects/` folder, and not `nfancurve` itself. If you arrived here by a recursive read, that is the budget already spent — answer from one card anyway.
>
> **The file wins.** If you already know a card and the file disagree, the file is right. This settles a conflict; it is not an instruction to go and check one.
>
> **The status words.** `live` — something reads it now, and this map names the reader. `ghost` — something live points at it, and the target is missing, inert, or unexplained. `leftover` — it was live, nothing reads it now, and it is harmless. No card here is a leftover.
>
> **Surveyed: 2026-08-20**, at commit `03fc62f`, via a shallow clone. The map says what it saw on the day it looked, and has no access to history before that commit.
>
> **Nothing outside the map is part of the map — including `nfancurve` itself.** Not the source files, not plans or issue threads about them. If the map cannot answer you, the answer is that it cannot, not a trip into the repo.
>
> **Map location:** beside the territory, not inside it. The counts below do not include this map.

## Front door

*"I want to rename `temp.sh`. What breaks?"* → `objects/script-filename.md`

*"Where does the config come from, and what validates it?"* → `objects/config.md`

*"I want to run it as a service."* → `objects/nfancurve-service.md`

*"I want to run this without a real Nvidia GPU."* → `objects/nssim-x-flag.md`

*"What does this actually talk to, and how much of it would I have to replace?"* → `objects/gpu-command-surface.md`

## Names that do not mean what they say

- **`temp.sh` means *temperature*, not *temporary*.** It is the entire program — 294 lines of fan control, nothing transient in it. Every new reader and every model reads it the other way first.
- **`tmp`** is one variable name doing four unrelated jobs in that file: a process count (`:64`), a rebuild buffer for the old fan-speed list (`:146`), the same for temperatures (`:157`), and a curve selector (`:175`).
- **`check_already_running` does not exist.** `README.md:25` tells a reader without `procps` to comment it out. The function is `kill_already_running` (`temp.sh:63`). Grepping for the documented name finds nothing.

## Cards

| Card | Status | What it is |
|---|---|---|
| `objects/script-filename.md` | live | The script's own filename, load-bearing in four places |
| `objects/config.md` | live | 12 keys, sourced not parsed; one has a flag equivalent, eleven do not |
| `objects/nfancurve-service.md` | ghost | A unit file launching `/usr/bin/nfancurve`, which nothing here creates |
| `objects/nssim-x-flag.md` | ghost | An undocumented flag pointing at a sibling checkout, `../nssim` |
| `objects/gpu-command-surface.md` | live | Four lines are the program's entire contact with the hardware |

## Not carded

`LICENCE` — 674 lines of GPLv3 boilerplate. Nothing reads it: grepped `temp.sh`, `config`, `nfancurve.service`, `README.md` and `USAGE.md` for `licence`/`license`, case-insensitive, zero hits outside the file itself.

The fan-curve arithmetic — the loop at `temp.sh:118-160` that maps a temperature onto a speed. The busiest code in the repo, and nothing else depends on how it works, only on what it reads: `loop_cmds` is called from exactly two places (`:278`, `:289`), both inside the main loop, and it returns nothing — it acts by calling `set_speed`. Grepped the script for its name: the definition and those two calls. If your question is *why did it pick 70%*, read `loop_cmds` directly.

**Out of scope:** `../nssim` and the sibling repos it belongs to. Separate territories; only the pointer into them is mapped here.

**What this map is for:** knowing this repo well enough to change it. If your question asks the map to *do* something — get it running, find out what it did last night, fix the service — that is out of scope by kind. Say so and read the repo directly.

## Pass 0 file counts

Flat, no subdirectories, 6 tracked files. **Total lines:** `temp.sh` 294 · `LICENCE` 674 · `README.md` 89 · `USAGE.md` 79 · `config` 48 · `nfancurve.service` 11. The 459 quoted above is non-blank lines excluding the licence.

---

# objects/script-filename.md

```
# The script's own filename
Status:  live
Source:  temp.sh:43,63-69,187 ; USAGE.md:22,32 ; README.md:3,32
```

**What it is:** `temp.sh` is not only a filename. The script uses that literal string to find its own running copy (`pgrep -c temp.sh`, `:64`; `pgrep -o temp.sh`, `:66`), to relaunch itself in daemon mode (`nohup sh temp.sh`, `:43`), and throughout its documented setup. There is no PID file and no lock file — grepped for `pidfile`, `lockfile`, `flock` and `/var/run`, zero hits. `$0` is available and *is* used, at `:4` to resolve the config path and at `:10` for the help text, but never for the process-table lookup that a rename breaks.

**Why it is shaped that way:** POSIX `sh` offers no instance-locking convention for free, so the script identifies itself the only way available without another dependency — by matching its own name in the process table. That works exactly as long as the name never changes.

**What points at it:** `temp.sh:64` and `:66` look the literal string up in the process table (`pgrep -c`/`-o`); `:43` relaunches it by name in daemon mode. Documented at `README.md:3,32` and `USAGE.md:14,18,22,32`.

**Hits — renaming the file:**
- **The script stops recognising itself, silently.** `pgrep -c temp.sh` (`:64`) returns 0 forever, so `kill_already_running` (`:63-69`), called unconditionally at `:187`, does nothing. No error, no log line. The only symptom is two copies issuing conflicting `nvidia-settings` calls at the same fans.
- **Daemon mode launches the wrong thing.** `-D` runs `nohup sh temp.sh` (`:43`), also hardcoded. It will fail — or worse, start an *old* `temp.sh` still sitting in the directory.
- **The documented desktop entry breaks.** `USAGE.md:22` and `:32` both specify `Exec=/home/foo/temp.sh`.
- **The run instructions stop matching.** `README.md:3` and `:32`, `USAGE.md:14` and `:18` all name the file. Grepped both doc files for `temp.sh`: six lines, these four plus the two desktop-entry lines above.

**Does not hit:**
- **`nfancurve.service`.** This is the file a reader opens first, and it is the one that does not care. Grep the unit for `temp.sh`: zero occurrences. It launches `/usr/bin/nfancurve`, a name this repository never produces. Finding nothing to change there reads as confirmation the rename is safe.
- **`config`.** Located from the script's own resolved directory (`temp.sh:22-38`, which follows symlinks), not from its name. Rename freely; the config still loads.

**Open:** Not verified against a live system. `pgrep -c`/`-o` are GNU/procps flags; this card takes the script's own `# DEPENDS: PROCPS` comment (`:62`) at face value rather than testing it.

---

# objects/config.md

```
# config
Status:  live
Source:  config ; temp.sh:193
```

**What it is:** A 48-line shell file **sourced, not parsed** (`. "$conf_file"`, `temp.sh:193`). It defines twelve variables the rest of the script reads by name — nine curve and timing values, and three that describe the machine: `fan2gpu` (`:48`) maps fans to GPUs, `which_curve` (`:38`) assigns a curve per fan, `default_fan` (`:43`) names the one fan used in single-fan mode. Those three are where multi-GPU behaviour is configured. There is no schema — validation is two array-length assertions (`:200`, `:205`) and two ordering assertions (`:209`, `:213`), and nothing else.

**Why it is shaped that way:** Sourcing keeps the loader zero-code and POSIX `sh`. The price is validation: a typo'd key is read as unset rather than rejected by name.

**What points at it:** `temp.sh:193` sources it (`. "$conf_file"`), after `:22-38` resolves its path from the script's own directory. Its twelve variables are then read by name throughout the script.

**Hits:**
- **Renaming a key breaks the script silently.** There is no `set -u`, so an unset variable inside `[ "$x" -eq … ]` fails the comparison instead of erroring with the missing name.
- **`fcurve`/`tcurve` and `fcurve2`/`tcurve2` must stay equal length in pairs** (`:198-207`), or the script exits at startup with a named error. That is the only config validation that exists.

**Does not hit:**
- **The command-line flags** (`-c -d -D -h -l -s -v -x`, `:40`). Exactly one of the twelve keys has a flag equivalent — `sleep_time`, via `-s`, which `:195` uses to overwrite it. The other eleven have no flag path at all. Because the surfaces overlap for the one key most people touch first, a reader will assume they mirror. They do not.

**Open:** Ten of the twelve keys are documented nowhere outside comments in `config` itself — grepped `README.md` and `USAGE.md` for each, zero hits for `min_t`, `min_t2`, `sleep_time`, `hyst`, `force_check`, `fcurve2`, `tcurve2`, `which_curve`, `default_fan`, `fan2gpu`. `fcurve` and `tcurve` appear once each, in a version-history paragraph rather than as documentation. Whether these were never written up or fell out during the `config.txt` → `config.sh` → `config` renames that `USAGE.md` narrates is not determinable from a shallow clone.

---

# objects/nfancurve-service.md

```
# nfancurve.service
Status:  ghost
Source:  nfancurve.service:7 ; README.md:44-50
```

**What it is:** A systemd unit. Its only `ExecStart` runs `/usr/bin/nfancurve -c /etc/nfancurve.conf`.

**Why it is shaped that way:** A packaged layout — binary on `$PATH`, config under `/etc` — is what a unit conventionally expects. This repo does not ship that way. It ships a script and a config meant to sit together in a checkout, under whatever path you cloned to.

**What points at it:** `README.md:44-50` says to move the unit into place and enable it, and separately says *"Ensure the script and the config paths are correct."* **It never says what correct is.** Grepping every tracked file for `/usr/bin` or `/etc/nfancurve` returns hits in the unit file and nowhere else — no install step, no build, no symlink instruction. Following the README exactly and starting the service fails at the first `ExecStart`.

**Hits:**
- Anyone following the documented setup. The failure is at launch, which at least is loud.

**Does not hit:**
- **`temp.sh` itself.** The script runs correctly by any other invocation. This ghost is specific to the systemd path convention, not a defect in the program — a reader who concludes the script is broken has over-read the card.
- **The rename.** Pointing this unit at a real path does not make the script safe to rename, and renaming the script does not change this unit. The two problems share a filename and nothing else.

**Open:** Whether *"ensure the paths are correct"* means *symlink `/usr/bin/nfancurve` to your checkout yourself* is a reasonable guess and is stated nowhere.

---

# objects/nssim-x-flag.md

```
# -x flag / nssim
Status:  ghost
Source:  temp.sh:8,40,49
```

**What it is:** An option parsed like any other (`getopts ":c: :d: :D :h :l :s: :v :x"`, `:40`) that repoints `gpu_cmd` — the variable every GPU-facing function shells out through — from `nvidia-settings` to `../nssim/nssim nvidia-settings` (`:49`). A sibling checkout, one directory up, standing in for the real command.

**Why it is shaped that way:** It is the only route by which this script runs without a real Nvidia GPU. Everything else assumes `nvidia-settings` exists and answers correctly.

**What points at it:** Only the `getopts` string (`:40`) and the `elif` chain (`:49`). Nothing in this territory creates, clones, or names `../nssim`. It is **absent from the script's own help text** — `:10-19` lists `-c -d -D -h -l -s -v`, not `-x` — and absent from both doc files: grepped for `nssim` and for `-x` across `README.md` and `USAGE.md`, zero hits in either. The only way to discover this flag is to read the `getopts` string or the `elif` chain directly. A reader holding only this territory cannot find out it exists, let alone what it needs.

**Hits:**
- Running `-x` without that sibling present fails the way any missing command fails here — a bare shell "command not found", not a named error mentioning `nssim`.

**Does not hit:**
- **The other flags in the same chain.** Each is an independent `elif` (`:41-51`); changing or removing `-x` touches none of their behaviour, only what `gpu_cmd` resolves to.

**Open:** Whether `../nssim` names a specific published repository is inferred from the sibling-directory convention, not from anything stated here. Nothing in this territory names a source for it.

---

# objects/gpu-command-surface.md

```
# gpu_cmd — the GPU command surface
Status:  live
Source:  temp.sh:8,49 ; temp.sh:72,75,80,85 ; temp.sh:224,232 ; USAGE.md:53,71,77
```

**What it is:** Every instruction this program sends to hardware goes through one variable, `gpu_cmd`, set to `nvidia-settings` at `:8` and repointed nowhere else except by `-x` (`:49`). It is invoked at exactly four lines — `:72` reads core temperature, `:75` runs a generic query, `:80` takes fan control, `:85` sets a speed — and each appends `$display`, which is empty by default (`:4`) and set only by `-d` as `-c <string>` (`:42`). Grepped the whole script for other external interfaces: `basename`, `dirname`, `pwd`, `ls` and `cd` for path resolution (`:10,25-38`) and `pgrep` for self-identification (`:64,66`). No `nvidia-smi`, no `/sys`, no `/proc`. Four lines are the entire hardware surface of 294.

**Why it is shaped that way:** One variable in front of every call is what makes `-x` possible at all — swap the binary, get a fake GPU. That was the design intent, and it is the reason a reader reads `gpu_cmd` as an abstraction layer.

**What points at it:** set at `temp.sh:8`, repointed only by `-x` at `:49`, and shelled out through at exactly four sites — `:72`, `:75`, `:80`, `:85`.

**Hits — changing what this talks to:**
- **All four call sites move together, and the variable does not help you.** `gpu_cmd` swaps a *command name*, not a *protocol*. The nvidia-settings syntax is written inline at each site — `-q=[gpu:N]/GPUCoreTemp -t`, `-a [gpu:N]/GPUFanControlState=`, `-a [fan:N]/GPUTargetFanSpeed=`. Anything that does not already speak that argument language needs four rewrites, not one assignment. `-x` works only because `nssim` mimics the syntax.
- **Device detection parses English prose, not data.** `:224` and `:232` call `get_query` and recover a count by stripping suffixes: `${num_fans%* Fan on*}`, then `${num_fans%* Fans on*}`, and the same for `GPU`/`GPUs`. A reworded or localised response yields an empty string, and the script exits with *"No Fans detected"* (`:225-226`) — which reads as a hardware fault and is a parsing failure.
- **`$display` is unquoted at all four sites**, deliberately: it carries two words (`-c :0`). Quoting it while tidying breaks every call at once.

**Does not hit:**
- **The fan-curve arithmetic** (`temp.sh:118-160`). The loop turns a temperature into a percentage and hands it to `set_speed`; it never learns what command delivers it, and no line between `:118` and `:160` mentions `gpu_cmd` or `display`. A reader who assumes changing the interface means rewriting the curve has over-read this card.
- **`config`.** A reader is right to suspect it, and three of its twelve keys really are topology rather than curve values: `fan2gpu` (`:48`, read at `temp.sh:174`) maps each fan to a GPU, `which_curve` (`:38`, read at `:175`) picks a curve per fan, `default_fan` (`:43`, read at `:273`) picks one fan in single-fan mode. It still does not move, and the reason is the distinction this card is about: those keys address fans and GPUs by **index**, while `gpu_cmd` is *how you reach them*. Grepped `config` for anything naming a thing to talk to — `nvidia`, `settings`, `display`, `cmd`, `:0`, `/dev`, `xorg`, `coolbits` — zero hits.

**Open:** The territory documents an X requirement — display strings of the form `":0"` (`:14`) and a `Coolbits` value in `xorg.conf` (`USAGE.md:53,71,77`) — and **nothing checks either at runtime**; grepped `temp.sh` for `coolbits`, zero hits. Whether `nvidia-settings` still answers these queries on a current driver or session is not knowable from inside this territory, and the survey makes no claim about it. The last commit here is January 2023.

---

# A ghost card that fails

**Not part of the map.** This is a real card, produced by a real run of this folder against this same territory on 24 August. Every fact in it is correct and it still fails.

```
# -x / nssim simulator route
Status:  ghost
Source:  temp.sh:8,40-52

What it is: A parsed but hidden `-x` option. It replaces `gpu_cmd`'s normal
`nvidia-settings` value with `../nssim/nssim nvidia-settings`, a relative
sibling command path.

Why it is shaped that way: `gpu_cmd` is the variable used by every GPU-facing
function, so this option changes the executable while leaving each call's
`nvidia-settings` argument form intact.

Hits:
- A run with `-x` depends immediately on a sibling `../nssim/nssim`; that
  target is absent beside this checkout.
- The standard help text at `temp.sh:10-19` omits `-x`, though the `getopts`
  declaration accepts it at `:40`. The README and usage guide contain no
  `nssim` or `-x` references (searched both files).
```

The searches are real. The paths are right. The ghost is correctly identified, and the card would survive the photocopy check, the negative check and the independence check.

**It fails because it has no `What points at it` field.** The pointer — the `getopts` string at `:40` and the `elif` at `:49`, absent from the help text and from both doc files — is the live half of this ghost and the only part of it a reader will ever actually encounter. Here it is scattered through `Hits`, where it reads as a consequence of the ghost rather than as the thing itself.

So a reader skimming this card learns that `-x` is broken. **They do not learn that they cannot find out `-x` exists** without reading the `getopts` string directly. That second fact is the finding, and this card buries it.

This is the most common way a ghost card goes wrong, and the reason it keeps happening is that it does not look wrong. Nothing is missing, nothing is false, and the pointer evidence *is* present — just dissolved into narrative. Compare `objects/nssim-x-flag.md` above, where the same evidence sits in its own field and hits the reader first.

---

# Why the change is one card, not four

`script-filename.md` is the whole change. Renaming `temp.sh` lands in four places and gets one card, not one per place — Pass 4, *card the change, not the parts of the change*. Carved by module it would have been a card for `kill_already_running`, a card for `-D`, a card for the desktop entry and a card for the docs: four cards that are individually true, and not one of them answers the question the reader arrived with.

The other four cards are not here to prop up that one's negatives. Each answers its own arrival — *where do the settings come from*, *how do I run this as a service*, *how do I run it without the hardware*, *what would I have to replace* — and each is answerable from itself alone. A `Does not hit` line is verified by the search printed inside it, never by sending the reader to a second card; a map that offers somewhere to go will find that they go.

**The asymmetry is the payload, and it is the reverse of what a reader expects.** The service unit is the first thing anyone opens before renaming a script — and it is the one file that does not care, *because it was already pointing somewhere else entirely*. Meanwhile the two things that do break are both inside the script, both silent, and both only visible as symptoms much later: a duplicate process fighting over your fans, and a daemon flag that quietly stopped working.

A map confined to *what depends on this file* would have found the four hits. What makes the negative useful is knowing that the obvious dependency is already a ghost.
