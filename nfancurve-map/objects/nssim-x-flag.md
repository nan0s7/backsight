# -x flag / nssim
Status:  ghost
Source:  temp.sh:8,40,49

**What it is:** An option parsed like any other (`getopts ":c: :d: :D :h :l :s: :v :x"`, `:40`) that repoints `gpu_cmd` — the variable every GPU-facing function shells out through — from `nvidia-settings` to `../nssim/nssim nvidia-settings` (`:49`). A sibling checkout, one directory up, standing in for the real command.

**Why it is shaped that way:** It is the only route by which this script runs without a real Nvidia GPU. Everything else assumes `nvidia-settings` exists and answers correctly.

**What points at it:** Only the `getopts` string (`:40`) and the `elif` chain (`:49`). Nothing in this territory creates, clones, or names `../nssim`. It is **absent from the script's own help text** — `:10-19` lists `-c -d -D -h -l -s -v`, not `-x` — and absent from both doc files: grepped for `nssim` and for `-x` across `README.md` and `USAGE.md`, zero hits in either. The only way to discover this flag is to read the `getopts` string or the `elif` chain directly. A reader holding only this territory cannot find out it exists, let alone what it needs.

**Hits:**
- Running `-x` without that sibling present fails the way any missing command fails here — a bare shell "command not found", not a named error mentioning `nssim`.

**Does not hit:**
- **The other flags in the same chain.** Each is an independent `elif` (`:41-51`); changing or removing `-x` touches none of their behaviour, only what `gpu_cmd` resolves to.

**Open:** Whether `../nssim` names a specific published repository is inferred from the sibling-directory convention, not from anything stated here. Nothing in this territory names a source for it.
