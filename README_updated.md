# DCC-Incomplete

This repository contains the implementation, benchmark material, experimental results, and figures associated with the study:

**Dynamic Conflict Clustering with Lightweight Coordinated Repair for Distributed Constraint Satisfaction**

DCC-Incomplete is an incomplete repair-based solver for Distributed Constraint Satisfaction Problems (DCSPs). The proposed method combines dynamic conflict clustering, structured coordination, strategic repair behaviour, and a lightweight memory-guided restart mechanism.

## Repository Contents

The repository currently contains:

```text
DCC-Incomplete/
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
├── DCC-Incomplete-With Comparison Results in 10 Times Execution(1).ipynb
├── benchmarks.zip
├── figures.zip
└── results.zip
```

### Main notebook

`DCC-Incomplete-With Comparison Results in 10 Times Execution(1).ipynb`

This is the main reproducibility notebook. It contains the DCC-Incomplete implementation, functional validation experiments, benchmark generation, pyDCOP baseline experiments, result aggregation, and paper-facing figure generation.

### `benchmarks.zip`

Contains the fixed benchmark material used in the experiments, including the 72 graph-colouring benchmark instances and the corresponding pyDCOP YAML representations.

### `results.zip`

Contains the experimental result files used for the paper, including DCC-Incomplete results, repeated external-baseline executions, aggregated baseline summaries, and stopping-limit sensitivity results.

### `figures.zip`

Contains the figures used in the paper, including functional-validation figures and the final external-comparison plots.

## Benchmark

The experiments use satisfiable distributed graph-colouring instances with planted satisfying assignments.

The benchmark contains **72 instances** generated from:

- 4 graph families:
  - `S` — sparse
  - `M` — medium density
  - `D` — dense
  - `XD` — extreme-dense stress test
- 3 problem sizes:
  - 20 variables
  - 50 variables
  - 100 variables
- 3 graph seeds
- 2 initial-assignment seeds

Therefore:

```text
4 families × 3 sizes × 3 graph seeds × 2 initial-assignment seeds
= 72 benchmark instances
```

The use of fixed graph seeds and initial-assignment seeds allows the same benchmark instances and starting conditions to be reproduced.

## Algorithms

The experiments include five algorithm configurations:

- **DCC-Incomplete-random** — DCC-Incomplete using random local restart for stagnation recovery.
- **DCC-Incomplete** — the proposed version using memory-guided restart.
- **DSA** — Distributed Stochastic Algorithm.
- **MGM** — Maximum Gain Message algorithm.
- **GDBA** — Generalized Distributed Breakout Algorithm.

DSA, MGM, and GDBA are executed using the pyDCOP framework.

## External-Baseline Repetition Protocol

The DCC-Incomplete variants retain their fixed 72-instance benchmark results.

To account for run-to-run variability in the external baselines, DSA, MGM, and GDBA are each executed **10 complete times over the same set of 72 benchmark instances**.

Within each complete baseline execution:

1. Success rate is calculated over the corresponding benchmark cases.
2. Runtime, iterations/cycles, messages, messages per variable, messages per constraint, and final cost are summarised using the median.
3. The final paper-facing baseline value is obtained by taking the arithmetic mean of the 10 execution-level summaries.

Thus, for each external baseline:

```text
72 benchmark instances × 10 complete executions
= 720 algorithm-instance runs
```

The three external baselines together therefore contain:

```text
720 × 3 = 2160 algorithm-instance runs
```

## Platform Support

**The current reproducibility notebook has been developed and tested on Windows.**

The pyDCOP execution cells use the Windows command interface (`cmd /c`) and the Windows pyDCOP launcher (`pydcop.BAT`). The notebook should therefore currently be treated as a **Windows-supported reproducibility workflow**.

The successfully executed reference environment recorded in the notebook used:

```text
Python 3.10.8
pyDCOP 0.1.2a1
```

Other package requirements are listed in `requirements.txt`.

## Software Requirements

### 1. Install Python

Install Python 3 before installing the project dependencies.

The reference execution used Python 3.10.8.

### 2. Install the required Python packages

From a command prompt opened in the repository directory, run:

```bash
pip install -r requirements.txt
```

The requirements file includes the main packages used by the notebook:

- NumPy
- pandas
- Matplotlib
- IPython
- Jupyter
- pyDCOP

### 3. Start Jupyter from the repository directory

For the current notebook structure, it is recommended to start Jupyter while the repository directory is the current working directory.

For example:

```bash
cd path\to\DCC-Incomplete
jupyter notebook
```

Then open:

```text
DCC-Incomplete-With Comparison Results in 10 Times Execution(1).ipynb
```

This is important because the notebook uses relative paths based on the current working directory.

## Running the Notebook

For a full reproduction:

1. Download or clone this repository.
2. Install Python and the packages in `requirements.txt`.
3. Start Jupyter from the repository root directory.
4. Open `DCC-Incomplete-With Comparison Results in 10 Times Execution(1).ipynb`.
5. Restart the kernel.
6. Run all cells from top to bottom.

The notebook uses the current working directory as the project root and creates its working/output directories automatically.

During execution, directories such as the following are created:

```text
figures/
final_four_level_benchmark/
pydcop_external_comparison/
final_paper1_comparison/
```

These are runtime/output directories generated by the notebook and are separate from the archived `benchmarks.zip`, `figures.zip`, and `results.zip` files included in the repository.

## Main Experimental Outputs

A successful complete execution produces, among other outputs:

```text
figures/
```

Functional-validation figures.

```text
final_four_level_benchmark/
```

The fixed four-family benchmark representation and DCC-Incomplete result files.

```text
pydcop_external_comparison/
```

pyDCOP YAML files, baseline outputs, repeated baseline executions, and associated result material.

```text
final_paper1_comparison/
```

Final paper-facing comparison tables and plots produced from the experimental results.

## Stopping Conditions

DCC-Incomplete, DCC-Incomplete-random, DSA, and MGM use the stopping budgets defined in the notebook and experimental setup, with a main iteration/cycle budget of 150 where applicable.

GDBA is evaluated using a strict 10-second timeout because the pyDCOP implementation used in this study does not support a cycle-based stopping condition for GDBA.

Additional stopping-limit sensitivity experiments for DCC-Incomplete-random are included in the archived result material.

## Reproducibility Notes

- The benchmark instances use fixed graph seeds and initial-assignment seeds.
- The same benchmark instances are used across the compared methods where applicable.
- The external baseline algorithms are repeated 10 complete times over the fixed 72-instance benchmark.
- Runtime values may vary slightly across machines because they depend on hardware, operating system, Python environment, and framework overhead.
- The notebook includes a compatibility patch used for running the installed pyDCOP implementation with NumPy 2.x.
- The notebook contains previously generated cell outputs from the reference Windows execution. Paths visible inside those saved outputs may therefore show the original local execution directory. The executable notebook code itself uses the current working directory as its project root.

## Archived Material

The ZIP archives are included so that the benchmark material, paper-facing figures, and final result files can be downloaded directly without rerunning the complete experiment.

To inspect these materials independently of the notebook, extract:

```text
benchmarks.zip
figures.zip
results.zip
```

The notebook does not require these archives to be extracted in order to create its own runtime/output directories during a fresh complete execution.

## Citation

If you use this implementation, benchmark, or experimental material, please cite the associated paper:

> R. Moghaddas et al., *Dynamic Conflict Clustering with Lightweight Coordinated Repair for Distributed Constraint Satisfaction*.

Complete publication details will be added after publication.

## Licence

This repository is provided for academic review, research transparency, and reproducibility purposes subject to the terms in the `LICENSE` file.
