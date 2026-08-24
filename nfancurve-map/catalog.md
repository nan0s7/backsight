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
