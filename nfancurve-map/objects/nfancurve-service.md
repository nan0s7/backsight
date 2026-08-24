# nfancurve.service
Status: ghost
Source: nfancurve.service:7; README.md:44-50

What it is: A systemd unit whose sole launch command is `/usr/bin/nfancurve -c /etc/nfancurve.conf`.

Why it is shaped that way: It assumes a packaged layout, while this repository supplies a checkout-local `temp.sh` and `config`.

What points at it: `README.md:44-50` tells the reader to install and enable the unit, then only says to ensure the script and config paths are correct. Searches of every tracked file for `/usr/bin` and `/etc/nfancurve` find the unit alone: nothing here installs, builds, links, or otherwise explains those targets.

Hits: Following the documented service setup reaches this broken launch path; it fails at `ExecStart` rather than exercising the script.

Does not hit: `temp.sh` is the wrong neighbour. Direct invocation remains separately documented at `README.md:32`; this is a service-layout ghost, not evidence that the program cannot run. Nor does correcting the unit make a script rename safe.

Open: The intended real paths or installation method cannot be derived from this territory.
