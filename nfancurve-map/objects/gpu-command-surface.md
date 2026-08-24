# gpu_cmd — the GPU command surface
Status:  live
Source:  temp.sh:8,49 ; temp.sh:72,75,80,85 ; temp.sh:224,232 ; USAGE.md:53,71,77

**What it is:** Every instruction this program sends to hardware goes through one variable, `gpu_cmd`, set to `nvidia-settings` at `:8` and repointed nowhere else except by `-x` (`:49`). It is invoked at exactly four lines — `:72` reads core temperature, `:75` runs a generic query, `:80` takes fan control, `:85` sets a speed — and each appends `$display`, which is empty by default (`:4`) and set only by `-d` as `-c <string>` (`:42`). Grepped the whole script for other external interfaces: `basename`, `dirname`, `pwd`, `ls` and `cd` for path resolution (`:10,25-38`) and `pgrep` for self-identification (`:64,66`). No `nvidia-smi`, no `/sys`, no `/proc`. Four lines are the entire hardware surface of 294.

**Why it is shaped that way:** One variable in front of every call is what makes `-x` possible at all — swap the binary, get a fake GPU. That was the design intent, and it is the reason a reader reads `gpu_cmd` as an abstraction layer.

**Hits — changing what this talks to:**
- **All four call sites move together, and the variable does not help you.** `gpu_cmd` swaps a *command name*, not a *protocol*. The nvidia-settings syntax is written inline at each site — `-q=[gpu:N]/GPUCoreTemp -t`, `-a [gpu:N]/GPUFanControlState=`, `-a [fan:N]/GPUTargetFanSpeed=`. Anything that does not already speak that argument language needs four rewrites, not one assignment. `-x` works only because `nssim` mimics the syntax.
- **Device detection parses English prose, not data.** `:224` and `:232` call `get_query` and recover a count by stripping suffixes: `${num_fans%* Fan on*}`, then `${num_fans%* Fans on*}`, and the same for `GPU`/`GPUs`. A reworded or localised response yields an empty string, and the script exits with *"No Fans detected"* (`:225-226`) — which reads as a hardware fault and is a parsing failure.
- **`$display` is unquoted at all four sites**, deliberately: it carries two words (`-c :0`). Quoting it while tidying breaks every call at once.

**Does not hit:**
- **The fan-curve arithmetic** (`temp.sh:118-160`). The loop turns a temperature into a percentage and hands it to `set_speed`; it never learns what command delivers it, and no line between `:118` and `:160` mentions `gpu_cmd` or `display`. A reader who assumes changing the interface means rewriting the curve has over-read this card.
- **`config`.** A reader is right to suspect it, and three of its twelve keys really are topology rather than curve values: `fan2gpu` (`:48`, read at `temp.sh:174`) maps each fan to a GPU, `which_curve` (`:38`, read at `:175`) picks a curve per fan, `default_fan` (`:43`, read at `:273`) picks one fan in single-fan mode. It still does not move, and the reason is the distinction this card is about: those keys address fans and GPUs by **index**, while `gpu_cmd` is *how you reach them*. Grepped `config` for anything naming a thing to talk to — `nvidia`, `settings`, `display`, `cmd`, `:0`, `/dev`, `xorg`, `coolbits` — zero hits.

**Open:** The territory documents an X requirement — display strings of the form `":0"` (`:14`) and a `Coolbits` value in `xorg.conf` (`USAGE.md:53,71,77`) — and **nothing checks either at runtime**; grepped `temp.sh` for `coolbits`, zero hits. Whether `nvidia-settings` still answers these queries on a current driver or session is not knowable from inside this territory, and the survey makes no claim about it. The last commit here is January 2023.
