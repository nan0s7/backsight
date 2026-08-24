# -x flag / nssim
Status: ghost
Source: temp.sh:8,40,49

What it is: `-x` redirects `gpu_cmd` from `nvidia-settings` to `../nssim/nssim nvidia-settings`, a sibling tool expected to mimic the real command.

Why it is shaped that way: It is the only in-repository route around real Nvidia hardware; the normal path sets `gpu_cmd` to `nvidia-settings` at `temp.sh:8`.

What points at it: The only pointers are the `getopts` specification (`temp.sh:40`) and its `elif` at `:49`. `-x` is absent from the help text (`:10-19`), and searches of `README.md` and `USAGE.md` for `nssim` and `-x` have zero matches. No tracked file creates, clones, or describes `../nssim`.

Hits: Running `-x` without that sibling leaves the shell trying to invoke a missing command, without a named `nssim` error from this program.

Does not hit: The adjacent flags are the wrong neighbour: each is an independent `elif` from `temp.sh:41-51`; altering `-x` alone only changes the command assigned to `gpu_cmd`.

Open: The identity and source of the presumed sibling repository are not stated in this territory.
