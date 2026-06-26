# Benchmark methodology — Portfolio v1.2

## Purpose

The demo is intended to show a reusable CUDA architecture for numerical parameter search, not to claim that every workload will receive the same acceleration.

## Dataset and search task

The default run generates five deterministic synthetic series, each with 480 observations. The model is:

```text
y = a * exp(-b * x) + c
```

For a grid with `N` steps per parameter, the engine evaluates `N³` parameter combinations per series. The portfolio script tests `40³`, `56³` and `72³` grids.

## GPU measures

### GPU device fast stage

This value is measured with CUDA events and covers:

```text
evaluation kernel + CUB segmented radix sort
```

It intentionally excludes host-device transfers, one-time allocation/setup, CPU refinement and file generation.

### GPU operational pipeline

This is a paired wall-clock measure using already allocated device buffers. In each repeated invocation, the device-only fast-stage event and the operational wall-clock path are measured together, so the two values are not drawn from separate GPU clock or power states. It covers:

```text
host-to-device input copies
+ evaluation kernel
+ CUB segmented radix sort
+ device-to-host Top-K copies
```

It intentionally excludes one-time allocation/setup, CPU refinement, CSV writing and plotting.

Both measures are repeated after a warm-up. The package exports min/median/max times. Use the **median**, not a single lucky minimum, in external material.

## CPU reference

The included CPU baseline is a single-thread full grid scan with the same search space. It deliberately excludes refinement and file output. It is a correctness and reference baseline, not a claim against every possible CPU implementation.

## Numeric validation

The run compares the best GPU grid MSE per series against the best CPU grid MSE. `validation_max_best_mse_abs_diff` is written to `benchmark.csv` and checked by `validate_benchmark_outputs.py`.

## Safe wording

Use language such as:

> On the tested hardware and synthetic calibration workload, the GPU fast-search stage evaluated and ranked the stated number of parameter candidates. The included single-thread CPU reference required longer on the same workload. Results are workload- and hardware-dependent.

Do **not** write:

- “GPU is always X times faster.”
- “This proves every model can be accelerated by GPU.”
- “End-to-end speedup” without stating whether transfers, refinement and file output are included.
