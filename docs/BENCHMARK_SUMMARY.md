# Final benchmark summary

Validated source version: v1.2 paired timing.

| Grid | Candidates | GPU paired operational median | CPU single-thread median | Median speedup |
|---:|---:|---:|---:|---:|
| 40^3 | 320,000 | 2.063 ms | 822.287 ms | 398.7x |
| 56^3 | 878,080 | 4.478 ms | 2,323.822 ms | 518.9x |
| 72^3 | 1,866,240 | 8.444 ms | 4,763.587 ms | 564.1x |

Timing protocol: 25 GPU repetitions and 5 CPU repetitions after warm-up, reporting medians.

The maximum absolute CPU/GPU best-grid MSE difference across the benchmark scales was 5.06e-09.
