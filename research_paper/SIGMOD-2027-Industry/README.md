# ForL0 SIGMOD 2027 Industrial Track paper

This directory contains the complete reproducible paper package for the current
SIGMOD 2027 Industrial Track draft.

## Files

- `briskstate_sigmod_industry.tex`: ACM `sigconf` source, including the three
  system diagrams and all evaluation tables.
- `briskstate_sigmod_industry.bib`: bibliography used by the paper.
- `ACM-Reference-Format.bst`: bibliography style retained locally so the paper
  can be compiled in an offline environment.
- `forl0_sigmod_industry.pdf`: Tectonic-compiled review PDF.

## Build

The verified build used Tectonic 0.17.0:

```text
tectonic -X compile briskstate_sigmod_industry.tex --outdir build --keep-logs
```

The verified output is seven pages, uses the ACM proceedings format, and has no
undefined references or citations. The only remaining TeX layout diagnostic is
a 1.44 pt vertical overfull box at the final page break; visual inspection found
no clipping, overlap, or isolated float page.

## Evidence incorporated

The draft is based on the repository implementation plus the final project
design and test reports. It records the following concrete evidence:

- Java control layer, typed JNI layer, and C++ native `StateEngine` hierarchy.
- `StateEngine -> StateTable -> KeyGroup -> Namespace -> SwissTable` ownership.
- ValueState, ListState, MapState, ReducingState, and AggregatingState mappings.
- Eight-way, 192-byte HotSet layout for the optional Kunpeng L0 hot-key cache.
- Copy-on-write checkpoint rules for update, insert, delete, restore, and rescale.
- 184/184 passing tests: 54 Java tests and 130 native tests.
- Intel WordCount: 1.270x mean and 1.414x peak over 36 configurations.
- Intel VTune: Memory Bound 48.1% -> 15.3%, DRAM Bound 30.8% -> 2.2%,
  CPI 0.615 -> 0.375, and Retiring 30.4% -> 50.9%.
- Intel NexMark with normal memory: 509 s -> 343 s aggregate runtime and
  1.39x geometric-mean speedup.
- Kunpeng tight-memory NexMark: q4 4.87x, q19 5.36x, and q20 5.96x.
- Kunpeng L0 on/off ablation, showing that native ownership is the principal
  system-level gain and L0 adds a smaller workload-dependent increment.
- Intel 16/18/20 GB TaskManager memory sweep and all reported per-query times.

## Submission blockers intentionally left explicit

1. The author list is a placeholder. SIGMOD Industrial Track is non-anonymous
   and requires at least one author with a non-academic affiliation. The real
   author order and affiliations must be supplied before submission.
2. Existing materials establish checkpoint/savepoint/restore correctness but do
   not contain matched checkpoint-duration or recovery-time measurements.
3. The available benchmark campaign records one completed run per configuration;
   repeated-run variance and confidence intervals remain to be collected.

These gaps are not replaced by estimated or invented data in the paper.
