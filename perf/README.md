# Profiling a build of the Linux kernel with LLVM via ``perf``

## One Time Setup

1. Install perf.
  - `sudo apt install perf`
2. Fetch linux kernel sources.
```sh
cd /tmp
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.19.12.tar.xz
tar xf linux-5.19.12.tar.xz
cd linux-5.19.12
```
  - Check kernel.org for latest release.
3. Install dependencies for kernel build.
  - `sudo apt install flex bison clang lld python3 perl make`
4. Check access to performance monitoring and observability operations.
  - `cat /proc/sys/kernel/perf_event_paranoid`
  - If this value is 1 or 2, then: `sudo sh -c "echo 0 > /proc/sys/kernel/perf_event_paranoid"`
5. Check that kernel addresses may be symbolicated.
  - `cat /proc/sys/kernel/kptr_restrict`
  - If this value is 1, then: `sudo sh -c "echo 0 > /proc/sys/kernel/kptr_restrict"`

## Profile a build

```sh
make LLVM=1 -j$(nproc) defconfig
perf record -e cycles:pp --freq=128 --call-graph lbr -- make LLVM=1 -j$(nproc)
```
TODO: issues on AMD until...zen3?

## Display results

```sh
perf report --no-children --sort=dso,symbol
```

### Errors

If you observe the error
```
Error:
Invalid event (cycles:ppu) in per-thread mode, enable system wide with '-a'.
```
and happen to be on an AMD ZEN2 or older kernel, try dropping `:pp` from the
`perf record` command, and switching from `--call-graph lbr` to
`--call-graph fp`. This will generally require you to recompile your binary
with frame pointers. LLVM can be profiled, but can only report hottest
functions without meaningful context.

Patches in flight to fix this:
- https://lore.kernel.org/lkml/166155216401.401.5809694678609694438.tip-bot2@tip-bot2/
- https://lore.kernel.org/lkml/20220829113347.295-1-ravi.bangoria@amd.com/
