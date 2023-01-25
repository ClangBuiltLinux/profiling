# Building the Linux kernel with Profile Data

Performance of the Linux kernel may be improved by providing data collected at
runtime of a previous build of the kernel.

There are two distinct methods of collecting such profile data; sampling and
instrumentation. They have different trade-offs and requirements.

The collected data is typically post-processed before being fed back into the
compiler for a successive build.

This document will touch on the tradeoffs, prerequisite software and hardware
requirements (if any) for either approaches, and relevant KCONFIGS or command
line flags to use.

## Trade-offs between sampling vs instrumentation

TODO

## Sampling

Sampling generally relies on hardware performance counters for low overhead and
accurate profile data.

### Software dependencies

- Linux perf (optional)
- Android simpleperf (optional)
- TODO: compiler versions > X.0, Y.0
- TODO: profile post processors

TODO: links for the above

### Hardware dependencies

- Intel x86 (TODO: starting from what uArch?)
- AMD Zen 4
- Aarch64???
- PPC???

TODO: how to check if your hardware is capable.

### KConfigs

For the training build, do XYZ (TODO).
For the optimized build, do ABC (TODO).

## Instrumentation

Instrumentation requires that the training build be rebuilt with additional
instructions inserted by the compiler to measure the frequency that branches
are taken.

### Software dependencies

TODO

### Hardware dependencies

- N/A!

### KConfigs

TODO


---
META NOTE TO FELLOW AUTHORS (this section to be deleted)
This is a draft of documentation we plan to submit to the upstream linux kernel
docs proper.  Please keep a list of authors/editors that have contributed to
this doc, and I will put their names on the commit message when submitting this
upstream.

The upstream kernel expects restructured text; this is markdown. I plan to use
pandoc to convert it (and hate writing rst).

https://pandoc.org/demos.html

Contributors:
Nick Desaulniers <ndesaulniers@google.com>
