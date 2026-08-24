# Examples

One worked map, reproduced whole: the catalog, five cards, and a note on how the busiest one was carved.

**Read this before writing a catalog.** It is calibration, not illustration — the thing being calibrated is how much a card leaves out.

Everything below the territory note is **verbatim tool output**, unedited: the catalog and cards this folder produced when pointed at `nfancurve` on 24 August 2026. Two independent runs of the same commit produced the same five cards, the same two ghosts and the same three collisions. What is hand-written here is the framing — this note, the territory description, and the closing note on carving.

---

## The territory

`nfancurve` — a POSIX shell script that drives Nvidia GPU fan curves on Linux. Six files, one branch, 459 non-blank lines excluding the licence. Public, and written by the author of this folder in 2018. Dormant since a pull request from a stranger merged in January 2023 — nothing about it has stopped working, which is what the Pass 0 test actually asks.

**The later reader:** someone who found this repo, saw the last commit was years ago, and wants to know what they are dealing with before they touch it. Orientation before the work, not the work — the map hands over the seams and does not do the port. In practice, often a model.

It is small on purpose. A map earns nothing on a territory you can read in one sitting *unless* the territory misleads you, and this one does: its most important name means the opposite of what it looks like, and the one file a reader opens before renaming a script is the one file that does not care.

**Map size: 86 non-blank lines against 459.** 19 per cent.

**This map also ships walkable**, at `nfancurve-map/` — the same catalog and the same five cards, as separate files. To test the two-hop walk, open `nfancurve-map/catalog.md` and nothing else. It is reproduced here in full because a worked map belongs in this file; the two are the same text.

## What this map does not demonstrate

Two of the four types in `reference/card-types.md` have no instance here, and the reason is the territory rather than an omission. `nfancurve` is flat — six files, no shelf — so there is no region to card. Its naming collisions are all lexical, so they are catalog lines, which `card-types.md` says is the right treatment.

There is also no `leftover`, and the survey looked: all twelve functions defined in `temp.sh` are called, and all twelve config keys are read. `hyst` appears unread until you notice `temp.sh:129` uses it inside `$(( ))` without a sigil, where a naive grep misses it. A territory with no leftover gets no leftover card. Do not manufacture one to complete the set.

---

# catalog.md

> **Load budget: this catalog, then one card. Stop.** Do not read a second card, the `objects/` folder, or `nfancurve` itself.
>
> **The file wins.** If a card and a real file disagree, the file is right. This settles a conflict; it is not an instruction to go and check one.
>
> **Status words:** **live** has a named reader; **ghost** is a claim the territory does not honour; **leftover** is harmless and unwired. No card here is a leftover.
>
> **Surveyed 2026-08-24** at commit `03fc62fc01608749c06ab15fee5e29dadce7746f`. The map describes that revision only.
>
> **Nothing outside this map is part of the map — including `nfancurve` itself.** If no card answers the question, say so and stop.
>
> **Map location:** beside the territory, not inside it. Its six-file count excludes this map.

## Front door

*“I want to rename `temp.sh`. What breaks?”* → `objects/script-filename.md`

*“Where does the config come from, and what validates it?”* → `objects/config.md`

*“I want to run it as a service.”* → `objects/nfancurve-service.md`

*“I want to run this without a real Nvidia GPU.”* → `objects/nssim-x-flag.md`

*“What does this actually talk to, and how much would I have to replace?”* → `objects/gpu-command-surface.md`

## Names that do not mean what they say

- **`temp.sh` means temperature, not temporary.** It is the whole 294-line program; treating it as disposable misroutes a reader around the program.
- **`tmp`** has four unrelated jobs in `temp.sh`: process count (`:64`), speed-list buffer (`:146`), temperature-list buffer (`:157`), and curve selector (`:175`).
- **`check_already_running` does not exist.** `README.md:25` names it; the function is `kill_already_running` at `temp.sh:63`.

## Cards

| Card | Status | What it decides |
|---|---|---|
| `objects/script-filename.md` | live | What must move with a script rename |
| `objects/config.md` | live | Which settings are sourced and how little is validated |
| `objects/nfancurve-service.md` | ghost | Why the shipped service cannot launch this checkout |
| `objects/nssim-x-flag.md` | ghost | What the undocumented simulation option assumes exists |
| `objects/gpu-command-surface.md` | live | Whether replacing the GPU interface is one change or four |

## What is not carded

`LICENCE` is GPLv3 text, not a consumer-facing seam: case-insensitive searches for `licence` and `license` across `temp.sh`, `config`, `nfancurve.service`, `README.md`, and `USAGE.md` found zero references outside the licence.

The fan-curve arithmetic at `temp.sh:114-172` is not carded. `loop_cmds` is defined once and called only at `:278` and `:289`, inside the main loops; it returns no value and affects the rest of the program only by calling `set_speed`. A question about why a particular speed was chosen needs that source directly.

`../nssim` is out of scope: it is a separate territory. Only this repository’s pointer to it is mapped.

This map is for orientation before changing this repository. Questions that ask it to run, repair, or diagnose the program are out of scope by kind.

## Pass 0 file counts

The territory is flat: six tracked files. Lines: `temp.sh` 294; `LICENCE` 674; `README.md` 89; `USAGE.md` 79; `config` 48; `nfancurve.service` 11. It has 459 non-blank lines excluding `LICENCE`.

---

# objects/script-filename.md

```
# The script's own filename
Status: live
Source: temp.sh:43,63-69,187; USAGE.md:14,18-35; README.md:3,32
```

What it is: `temp.sh` is a load-bearing name, not merely the script filename. It is hard-coded for duplicate-process detection (`pgrep` at `temp.sh:64,66`), daemon relaunch (`:43`), and the documented invocation and desktop entry.

Why it is shaped that way: The POSIX shell script has no PID or lock file (searched `pidfile`, `lockfile`, `flock`, and `/var/run`: zero matches), so it identifies its own running instance by process name. `$0` is used for config resolution and help, but never for the process lookup.

Hits:
- Renaming it makes `kill_already_running` (`:63-69`, called at `:187`) stop finding running copies. The silent result is concurrent copies issuing fan commands.
- `-D` relaunches `nohup sh temp.sh` (`:43`): a rename makes daemon mode fail or start an old same-named file.
- The desktop-entry examples and run instructions name `temp.sh` (README `:3,32`; USAGE `:14,18-35`).

Does not hit: `nfancurve.service` is the plausible neighbour but contains no `temp.sh` (searched the unit; zero matches); it already launches `/usr/bin/nfancurve`, which this repository does not create. `config` still loads because the script resolves its directory, follows symlinks, then appends `/config` (`temp.sh:22-38`).

Open: Not tested on a live system; `pgrep -c` and `-o` are assumed to be available through the stated `procps` dependency (`temp.sh:62`).

---

# objects/config.md

```
# config
Status: live
Source: config; temp.sh:193-215,248-267
```

What it is: A 48-line shell file sourced, not parsed, by `. "$conf_file"` (`temp.sh:193`). It supplies twelve values: curves/timing plus `fan2gpu`, `which_curve`, and `default_fan`, which describe the fan-to-GPU layout.

Why it is shaped that way: Sourcing keeps the POSIX loader minimal, but makes this executable shell input rather than a schema-controlled settings file.

Hits:
- A renamed or misspelled key becomes unset; there is no `set -u` or key-name validation.
- `fcurve`/`tcurve` and `fcurve2`/`tcurve2` must remain equal-length pairs, or startup exits at `temp.sh:198-207`; their first temperature must also exceed `min_t`/`min_t2` (`:208-215`).

Does not hit: Command-line options are the wrong neighbour. Only `sleep_time` has an equivalent (`-s`, copied over it at `temp.sh:195`); the other eleven settings have no option path, despite all options being parsed together at `:40-51`.

Open: Documentation beyond the inline comments is thin: searches of both Markdown files found no named documentation for ten keys; `fcurve` and `tcurve` occur only in USAGE’s version-history paragraph.

---

# objects/nfancurve-service.md

```
# nfancurve.service
Status: ghost
Source: nfancurve.service:7; README.md:44-50
```

What it is: A systemd unit whose sole launch command is `/usr/bin/nfancurve -c /etc/nfancurve.conf`.

Why it is shaped that way: It assumes a packaged layout, while this repository supplies a checkout-local `temp.sh` and `config`.

What points at it: `README.md:44-50` tells the reader to install and enable the unit, then only says to ensure the script and config paths are correct. Searches of every tracked file for `/usr/bin` and `/etc/nfancurve` find the unit alone: nothing here installs, builds, links, or otherwise explains those targets.

Hits: Following the documented service setup reaches this broken launch path; it fails at `ExecStart` rather than exercising the script.

Does not hit: `temp.sh` is the wrong neighbour. Direct invocation remains separately documented at `README.md:32`; this is a service-layout ghost, not evidence that the program cannot run. Nor does correcting the unit make a script rename safe.

Open: The intended real paths or installation method cannot be derived from this territory.

---

# objects/nssim-x-flag.md

```
# -x flag / nssim
Status: ghost
Source: temp.sh:8,40,49
```

What it is: `-x` redirects `gpu_cmd` from `nvidia-settings` to `../nssim/nssim nvidia-settings`, a sibling tool expected to mimic the real command.

Why it is shaped that way: It is the only in-repository route around real Nvidia hardware; the normal path sets `gpu_cmd` to `nvidia-settings` at `temp.sh:8`.

What points at it: The only pointers are the `getopts` specification (`temp.sh:40`) and its `elif` at `:49`. `-x` is absent from the help text (`:10-19`), and searches of `README.md` and `USAGE.md` for `nssim` and `-x` have zero matches. No tracked file creates, clones, or describes `../nssim`.

Hits: Running `-x` without that sibling leaves the shell trying to invoke a missing command, without a named `nssim` error from this program.

Does not hit: The adjacent flags are the wrong neighbour: each is an independent `elif` from `temp.sh:41-51`; altering `-x` alone only changes the command assigned to `gpu_cmd`.

Open: The identity and source of the presumed sibling repository are not stated in this territory.

---

# objects/gpu-command-surface.md

```
# gpu_cmd — the GPU command surface
Status: live
Source: temp.sh:8,49,72,75,80,85,224,232; USAGE.md:53,71,77
```

What it is: Every hardware-facing action calls `$gpu_cmd`: temperature query (`temp.sh:72`), general query (`:75`), fan-control state (`:80`), and fan speed (`:85`). `-d` supplies its display argument (`:42`).

Why it is shaped that way: One command variable permits the hidden `-x` simulation route, but the call syntax remains written at each of the four sites.

Hits:
- Replacing the GPU interface requires four protocol rewrites, not a one-line command swap: the inline calls use distinct `nvidia-settings` query and assignment syntax.
- Device detection parses the English `Fan(s) on` and `GPU(s) on` suffixes from query output (`:224-237`). A changed or localised response becomes an empty count and reports “No Fans detected” (`:225-226`).
- `$display` is intentionally unquoted because it carries `-c <string>`; quoting it as one argument changes all four calls.

Does not hit: Fan-curve arithmetic at `temp.sh:114-172` is the wrong neighbour. It maps temperatures to a percentage and calls `set_speed`, but has no `gpu_cmd` or `display` reference. `config` likewise stores only numeric/topology indexes: searches there for `nvidia`, `settings`, `display`, `cmd`, `:0`, `/dev`, `xorg`, and `coolbits` return zero hits.

Open: The territory documents X/CoolBits setup but `temp.sh` does not check it (searched `coolbits`: zero hits); current-driver behaviour cannot be established from this 2023 revision.

---

# Why the change is one card, not four

`script-filename.md` is the whole change. Renaming `temp.sh` lands in four places and gets one card, not one per place — Pass 4, *card the change, not the parts of the change*. Carved by module it would have been a card for `kill_already_running`, a card for `-D`, a card for the desktop entry and a card for the docs: four cards that are individually true, and not one of them answers the question the reader arrived with.

The other four cards are not here to prop up that one's negatives. Each answers its own arrival — *where do the settings come from*, *how do I run this as a service*, *how do I run it without the hardware*, *what would I have to replace* — and each is answerable from itself alone. A `Does not hit` line is verified by the search printed inside it, never by sending the reader to a second card; a map that offers somewhere to go will find that they go.

**The asymmetry is the payload, and it is the reverse of what a reader expects.** The service unit is the first thing anyone opens before renaming a script — and it is the one file that does not care, *because it was already pointing somewhere else entirely*. Meanwhile the two things that do break are both inside the script, both silent, and both only visible as symptoms much later: a duplicate process fighting over your fans, and a daemon flag that quietly stopped working.

A map confined to *what depends on this file* would have found the four hits. What makes the negative useful is knowing that the obvious dependency is already a ghost.
