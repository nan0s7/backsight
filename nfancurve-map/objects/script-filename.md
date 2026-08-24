# The script's own filename
Status: live
Source: temp.sh:43,63-69,187; USAGE.md:14,18-35; README.md:3,32

What it is: `temp.sh` is a load-bearing name, not merely the script filename. It is hard-coded for duplicate-process detection (`pgrep` at `temp.sh:64,66`), daemon relaunch (`:43`), and the documented invocation and desktop entry.

Why it is shaped that way: The POSIX shell script has no PID or lock file (searched `pidfile`, `lockfile`, `flock`, and `/var/run`: zero matches), so it identifies its own running instance by process name. `$0` is used for config resolution and help, but never for the process lookup.

Hits:
- Renaming it makes `kill_already_running` (`:63-69`, called at `:187`) stop finding running copies. The silent result is concurrent copies issuing fan commands.
- `-D` relaunches `nohup sh temp.sh` (`:43`): a rename makes daemon mode fail or start an old same-named file.
- The desktop-entry examples and run instructions name `temp.sh` (README `:3,32`; USAGE `:14,18-35`).

Does not hit: `nfancurve.service` is the plausible neighbour but contains no `temp.sh` (searched the unit; zero matches); it already launches `/usr/bin/nfancurve`, which this repository does not create. `config` still loads because the script resolves its directory, follows symlinks, then appends `/config` (`temp.sh:22-38`).

Open: Not tested on a live system; `pgrep -c` and `-o` are assumed to be available through the stated `procps` dependency (`temp.sh:62`).
