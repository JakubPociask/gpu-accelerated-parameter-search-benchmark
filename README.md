# GPU Batch Optimization Engine

A portfolio case study and benchmark evidence package for GPU-accelerated nonlinear parameter search and calibration.

## What it demonstrates

- GPU evaluation of large parameter grids
- Segmented Top-K selection with CUB
- CPU refinement of shortlisted candidates
- Reproducible CPU-vs-GPU benchmark reporting
- Structured benchmark evidence, CSV outputs and figures

The repository uses deterministic synthetic curves. It does not include proprietary financial models, production code, or private data.

## Verified benchmark environment

- GPU: NVIDIA GeForce GTX 1660 Ti
- Compute capability: 7.5
- Build target: `sm_75`
- Dataset: 5 synthetic series x 480 observations
- Timing protocol: 25 GPU repetitions and 5 CPU repetitions after warm-up; medians reported
- CPU reference: single-thread full-grid implementation used for the benchmark

## Results

| Grid | Total candidates | CPU median | GPU paired operational path median | Speedup |
|---:|---:|---:|---:|---:|
| 40^3 | 320,000 | 0.822287 s | 0.002063 s | 398.7x |
| 56^3 | 878,080 | 2.323822 s | 0.004478 s | 518.9x |
| 72^3 | 1,866,240 | 4.763587 s | 0.008444 s | 564.1x |

The paired GPU operational path includes host-to-device transfer, fast grid evaluation, segmented Top-K selection, and Top-K device-to-host transfer. It excludes one-time allocation, CPU refinement, CSV writing, and figure rendering.

Across the three benchmark scales, the maximum absolute difference between the CPU and GPU best-grid MSE was `5.06e-09`.

## Why Top-K refinement matters

The fast stage screens the entire grid on the GPU. The CPU then refines shortlisted candidates. Across the five synthetic series, refinement reduced MSE by an average of 15.6%. In one series, the best refined solution originated from fast rank 1 rather than rank 0, which is why retaining a Top-K set is more robust than trusting one grid point.

## Technical demo availability

This public repository intentionally contains benchmark evidence, methodology, figures and sample outputs only.

The runnable source code, build scripts and executable binaries are not distributed publicly. A live technical walkthrough and runnable demonstration can be reviewed privately with qualified clients during a project discussion or paid discovery phase.

## Notes

Performance depends on hardware, compiler, model complexity, data layout, and workload size. These numbers are a benchmark of this included synthetic workload and should not be interpreted as a general guarantee.

## Copyright and use

Copyright (c) 2026 Jakub Pociask. All rights reserved.

This repository is shared for portfolio and evaluation purposes. Commercial reuse, redistribution, modification, or incorporation into another product requires prior written permission. See [COPYRIGHT_AND_USE.md](COPYRIGHT_AND_USE.md).

## Contact

For private technical demos, commercial licensing, consulting, or GPU-accelerated numerical optimization projects:

**Jakub Pociask**
