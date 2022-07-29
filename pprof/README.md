# pprof

## Setup

*excerpt from https://github.com/google/pprof*

1. `$ go install github.com/google/pprof@latest`

2. Get Google's build tool, [bazel](https://bazel.build/install/ubuntu)

3. Build [perf_data_converter](https://github.com/google/perf_data_converter) with bazel.
```sh
> git clone https://github.com/google/perf_data_converter.git
> cd perf_data_converter
> bazel build src:perf_to_profile
```

4. move perf_to_profile binary to `/usr/local/bin` for ease of use.

`$ sudo cp bazel-perf_data_converter/bazel-out/k8-fastbuild/bin/src/perf_to_profile /usr/local/bin/perf_to_profile`

## Workflow

1. perf record something (Like a kernel build).
```sh
#x86_64 kernel build with cpu cycles
$ perf record -e cycles:pp --freq=256 --output=perf.data --call-graph lbr -- make LLVM=1 -j72
```

2. convert `perf.data` to profile proto using `perf_to_profile`.

`$ perf_to_profile -i perf.data -o profile.data`

3. Feed to pprof.

### Internally within Google
`$ pprof -flame -nodefraction .1 profile.data`

### Externally, must host web server locally
`$ pprof -http=":8000" -nodefraction .1 profile.data`
