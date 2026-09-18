# DCC-Incomplete: Dynamic Conflict Clustering with Lightweight Coordinated Repair for Distributed Constraint Satisfaction

This repository contains the implementation, benchmark instances, experimental results, and figures associated with the paper:

**Dynamic Conflict Clustering with Lightweight Coordinated Repair for Distributed Constraint Satisfaction**

DCC-Incomplete is an incomplete repair-based solver for Distributed Constraint Satisfaction Problems (DCSPs). It uses dynamic conflict clustering, structured coordination, strategic repair mechanisms, and a lightweight memory-guided restart mechanism to reduce unnecessary communication and improve recovery from stagnation.

## Repository Structure

```text
DCC-Incomplete Repository/
│
├── Benchmarks/
│   ├── final_four_level_benchmark/
│   │   ├── exported_instances/
│   │   ├── exported_instances_manifest.csv
│   │   └── final_four_level_benchmark_manifest.csv
│   │
│   └── pydcop_yaml/
│       ├── case_0001.yaml
│       ├── ...
│       └── case_0072.yaml
│
├── results/
│   ├── dcc/
│   ├── external_baselines/
│   │   ├── raw_executions/
│   │   ├── execution_summaries/
│   │   ├── paper_results/
│   │   ├── gdba_runtime_metrics/
│   │   └── all_10_executions_raw.csv
│   └── stopping_limit_sensitivity/
│
├── figures/
│   ├── architecture/
│   ├── functional_validation/
│   └── external_comparison/
│
├── notebooks/
│   └── DCC_Incomplete_Reproducible_Experiments.ipynb
│
├── README.md
├── requirements.txt
├── .gitignore
└── LICENSE
```

## Algorithms

The repository contains results for five algorithm configurations:

- **DCC-Incomplete-random**: DCC-Incomplete using random local restart for stagnation recovery.
- **DCC-Incomplete**: the proposed solver using memory-guided restart.
- **DSA**: Distributed Stochastic Algorithm.
- **MGM**: Maximum Gain Message algorithm.
- **GDBA**: Generalized Distributed Breakout Algorithm.

DSA, MGM, and GDBA are evaluated using their implementations in the pyDCOP framework.

## Benchmark

The experiments use satisfiable distributed graph-colouring instances with planted satisfying assignments.

The benchmark contains **72 instances**, generated from:

- 4 graph families: `S` (Sparse), `M` (Medium), `D` (Dense), and `XD` (Extreme-dense stress test)
- 3 graph sizes: 20, 50, and 100 variables
- 3 graph seeds
- 2 initial-assignment seeds

Therefore:

```text
4 families × 3 sizes × 3 graph seeds × 2 initial-assignment seeds
= 72 benchmark instances
```

The exact benchmark definitions are provided in:

```text
Benchmarks/final_four_level_benchmark/
```

The corresponding pyDCOP representations are provided in:

```text
Benchmarks/pydcop_yaml/
```

## Experimental Protocol

The DCC-Incomplete variants are evaluated on the fixed set of 72 benchmark instances.

To account for run-to-run variability in the external pyDCOP baselines, **DSA, MGM, and GDBA are each executed 10 complete times over the same 72 benchmark instances**.

For each complete baseline execution:

1. Success rate is calculated over the corresponding benchmark cases.
2. Runtime, iterations/cycles, messages, messages per variable, messages per constraint, and final cost are summarised using the median.
3. The final reported baseline value is obtained by taking the arithmetic mean of the 10 execution-level summaries.

Thus, each external baseline contains:

```text
72 instances × 10 executions = 720 algorithm-instance runs
```

and the three external baselines together contain:

```text
720 × 3 = 2160 algorithm-instance runs
```

The DCC-Incomplete results retain the fixed benchmark evaluation and are not aggregated over 10 repeated executions.

## Stopping Conditions

DSA and MGM use a stopping budget of 150 cycles where applicable.

GDBA is evaluated using a strict 10-second timeout because the pyDCOP implementation used in this study does not support `stop_cycle` for GDBA.

The DCC-Incomplete stopping parameters, including those used for the XD stress-test experiments, are defined directly in the reproducibility notebook.

## Reproducing the Experiments

The complete experimental workflow is provided in:

```text
notebooks/DCC_Incomplete_Reproducible_Experiments.ipynb
```

To reproduce the experiments:

1. Clone or download this repository.
2. Create a Python environment and install the packages listed in `requirements.txt`.
3. Open the notebook in Jupyter Notebook or JupyterLab.
4. Restart the kernel.
5. Run all cells from top to bottom.

The notebook:

- constructs and validates the benchmark;
- exports the 72 benchmark instances;
- runs the DCC-Incomplete variants;
- prepares the pyDCOP benchmark representations;
- executes DSA, MGM, and GDBA;
- performs the 10 complete baseline executions;
- generates execution-level and final aggregated results;
- creates the paper-facing comparison tables;
- generates the comparison figures.

## Results

### DCC-Incomplete Results

The fixed DCC results are stored in:

```text
results/dcc/
```

This includes raw and summary results for the complete four-family benchmark, the main S/M/D comparison, the XD stress-test family, and the random-restart and memory-guided comparison.

### External Baseline Results

The 10-execution baseline results are stored in:

```text
results/external_baselines/
```

The individual executions are available in:

```text
results/external_baselines/raw_executions/
```

with:

```text
execution_01.csv
...
execution_10.csv
```

Execution-level summaries are stored in:

```text
results/external_baselines/execution_summaries/
```

The final aggregated values used for the paper are stored in:

```text
results/external_baselines/paper_results/
```

The complete raw dataset across the 10 executions is also provided as:

```text
results/external_baselines/all_10_executions_raw.csv
```

### Stopping-Limit Sensitivity

The extended stopping-budget experiments for DCC-Incomplete-random are stored in:

```text
results/stopping_limit_sensitivity/
```

These experiments investigate whether increasing the iteration and restart limits alone can provide results comparable with the memory-guided restart mechanism.

## Figures

Figures used in the paper are organised as:

```text
figures/architecture/
figures/functional_validation/
figures/external_comparison/
```

The external-comparison figures are generated from the final 10-execution baseline evaluation.

## Reproducibility Notes

The benchmark graph seeds and initial-assignment seeds are fixed so that the same benchmark instances can be reconstructed.

The external baseline repetitions do not change the benchmark instances; they repeat the algorithms on the same fixed benchmark in order to account for stochastic run-to-run variability.

Runtime values may vary slightly across machines because they depend on hardware, operating system, Python environment, and framework overhead.

## Software

The implementation uses Python and Jupyter Notebook.

External comparison algorithms are executed using **pyDCOP**.

Exact package information is provided in:

```text
requirements.txt
```

## Citation

If you use this implementation, benchmark, or experimental results, please cite the associated paper:

> R. Moghaddas et al., *Dynamic Conflict Clustering with Lightweight Coordinated Repair for Distributed Constraint Satisfaction*.

The complete publication details will be added after publication.

## License

See the `LICENSE` file for the repository licence.
