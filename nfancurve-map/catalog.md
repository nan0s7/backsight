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
