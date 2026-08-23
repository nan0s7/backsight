# nfancurve.service
Status:  ghost
Source:  nfancurve.service:7 ; README.md:44-50

**What it is:** A systemd unit. Its only `ExecStart` runs `/usr/bin/nfancurve -c /etc/nfancurve.conf`.

**Why it is shaped that way:** A packaged layout — binary on `$PATH`, config under `/etc` — is what a unit conventionally expects. This repo does not ship that way. It ships a script and a config meant to sit together in a checkout, under whatever path you cloned to.

**What points at it:** `README.md:44-50` says to move the unit into place and enable it, and separately says *"Ensure the script and the config paths are correct."* **It never says what correct is.** Grepping every tracked file for `/usr/bin` or `/etc/nfancurve` returns hits in the unit file and nowhere else — no install step, no build, no symlink instruction. Following the README exactly and starting the service fails at the first `ExecStart`.

**Hits:**
- Anyone following the documented setup. The failure is at launch, which at least is loud.

**Does not hit:**
- **`temp.sh` itself.** The script runs correctly by any other invocation. This ghost is specific to the systemd path convention, not a defect in the program — a reader who concludes the script is broken has over-read the card.
- **The rename.** Pointing this unit at a real path does not make the script safe to rename, and renaming the script does not change this unit. The two problems share a filename and nothing else.

**Open:** Whether *"ensure the paths are correct"* means *symlink `/usr/bin/nfancurve` to your checkout yourself* is a reasonable guess and is stated nowhere.
