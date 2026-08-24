# gpu_cmd — the GPU command surface
Status: live
Source: temp.sh:8,49,72,75,80,85,224,232; USAGE.md:53,71,77

What it is: Every hardware-facing action calls `$gpu_cmd`: temperature query (`temp.sh:72`), general query (`:75`), fan-control state (`:80`), and fan speed (`:85`). `-d` supplies its display argument (`:42`).

Why it is shaped that way: One command variable permits the hidden `-x` simulation route, but the call syntax remains written at each of the four sites.

Hits:
- Replacing the GPU interface requires four protocol rewrites, not a one-line command swap: the inline calls use distinct `nvidia-settings` query and assignment syntax.
- Device detection parses the English `Fan(s) on` and `GPU(s) on` suffixes from query output (`:224-237`). A changed or localised response becomes an empty count and reports “No Fans detected” (`:225-226`).
- `$display` is intentionally unquoted because it carries `-c <string>`; quoting it as one argument changes all four calls.

Does not hit: Fan-curve arithmetic at `temp.sh:114-172` is the wrong neighbour. It maps temperatures to a percentage and calls `set_speed`, but has no `gpu_cmd` or `display` reference. `config` likewise stores only numeric/topology indexes: searches there for `nvidia`, `settings`, `display`, `cmd`, `:0`, `/dev`, `xorg`, and `coolbits` return zero hits.

Open: The territory documents X/CoolBits setup but `temp.sh` does not check it (searched `coolbits`: zero hits); current-driver behaviour cannot be established from this 2023 revision.
