# Case Study - GPU-Accelerated Nonlinear Parameter Search

## Problem
Nonlinear calibration and brute-force parameter search can become slow when a model must be evaluated across hundreds of thousands or millions of candidate parameter combinations.

## Approach
A neutral CUDA pipeline was built around four stages:

1. Generate or load multiple time series.
2. Evaluate the full parameter grid on the GPU.
3. Segment and rank candidates per series, retaining Top-K candidates.
4. Refine shortlisted candidates on the CPU and export benchmark and fit-quality outputs.

## Verified result
On an NVIDIA GeForce GTX 1660 Ti, the paired GPU operational path processed 1,866,240 nonlinear candidates in 8.444 ms. The included single-thread CPU full-grid reference took 4.764 s. The median speedup was 564.1x across the timed workload.

The paired path includes host-to-device transfer, GPU grid evaluation, segmented Top-K selection, and Top-K device-to-host transfer. It excludes one-time allocation, CPU refinement, file output, and chart rendering.

## Validation
The CPU and GPU best-grid results agreed to within a maximum MSE difference of 5.06e-09 across the three benchmark scales. The subsequent Top-K refinement reduced MSE by 15.6% on average across the five synthetic series.

## Why it matters
This pattern is reusable where a client has a costly numerical loop, parameter sweep, calibration routine, simulation, or model-selection problem. The objective function and parameter schema can be adapted while preserving the same high-throughput search, ranking, validation, and reporting structure.
