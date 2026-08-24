# The script's own filename
Status:  live
Source:  temp.sh:43,63-69,187 ; USAGE.md:22,32 ; README.md:3,32

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
