# config
Status: live
Source: config; temp.sh:193-215,248-267

What it is: A 48-line shell file sourced, not parsed, by `. "$conf_file"` (`temp.sh:193`). It supplies twelve values: curves/timing plus `fan2gpu`, `which_curve`, and `default_fan`, which describe the fan-to-GPU layout.

Why it is shaped that way: Sourcing keeps the POSIX loader minimal, but makes this executable shell input rather than a schema-controlled settings file.

Hits:
- A renamed or misspelled key becomes unset; there is no `set -u` or key-name validation.
- `fcurve`/`tcurve` and `fcurve2`/`tcurve2` must remain equal-length pairs, or startup exits at `temp.sh:198-207`; their first temperature must also exceed `min_t`/`min_t2` (`:208-215`).

Does not hit: Command-line options are the wrong neighbour. Only `sleep_time` has an equivalent (`-s`, copied over it at `temp.sh:195`); the other eleven settings have no option path, despite all options being parsed together at `:40-51`.

Open: Documentation beyond the inline comments is thin: searches of both Markdown files found no named documentation for ten keys; `fcurve` and `tcurve` occur only in USAGE’s version-history paragraph.
