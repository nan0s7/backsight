# config
Status:  live
Source:  config ; temp.sh:193

**What it is:** A 48-line shell file **sourced, not parsed** (`. "$conf_file"`, `temp.sh:193`). It defines twelve variables the rest of the script reads by name — nine curve and timing values, and three that describe the machine: `fan2gpu` (`:48`) maps fans to GPUs, `which_curve` (`:38`) assigns a curve per fan, `default_fan` (`:43`) names the one fan used in single-fan mode. Those three are where multi-GPU behaviour is configured. There is no schema — validation is two array-length assertions (`:200`, `:205`) and two ordering assertions (`:209`, `:213`), and nothing else.

**Why it is shaped that way:** Sourcing keeps the loader zero-code and POSIX `sh`. The price is validation: a typo'd key is read as unset rather than rejected by name.

**What points at it:** `temp.sh:193` sources it (`. "$conf_file"`), after `:22-38` resolves its path from the script's own directory. Its twelve variables are then read by name throughout the script.

**Hits:**
- **Renaming a key breaks the script silently.** There is no `set -u`, so an unset variable inside `[ "$x" -eq … ]` fails the comparison instead of erroring with the missing name.
- **`fcurve`/`tcurve` and `fcurve2`/`tcurve2` must stay equal length in pairs** (`:198-207`), or the script exits at startup with a named error. That is the only config validation that exists.

**Does not hit:**
- **The command-line flags** (`-c -d -D -h -l -s -v -x`, `:40`). Exactly one of the twelve keys has a flag equivalent — `sleep_time`, via `-s`, which `:195` uses to overwrite it. The other eleven have no flag path at all. Because the surfaces overlap for the one key most people touch first, a reader will assume they mirror. They do not.

**Open:** Ten of the twelve keys are documented nowhere outside comments in `config` itself — grepped `README.md` and `USAGE.md` for each, zero hits for `min_t`, `min_t2`, `sleep_time`, `hyst`, `force_check`, `fcurve2`, `tcurve2`, `which_curve`, `default_fan`, `fan2gpu`. `fcurve` and `tcurve` appear once each, in a version-history paragraph rather than as documentation. Whether these were never written up or fell out during the `config.txt` → `config.sh` → `config` renames that `USAGE.md` narrates is not determinable from a shallow clone.
