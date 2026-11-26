# Adaptive Physics-Informed Battery Digital Twin

A research-oriented proof of concept for battery State of Health (SOH) and Remaining Useful Life (RUL) prediction, with a cell-aware evaluation pipeline and replay-based online adaptation.

## Repository layout

```text
battery_twin/   Installable Python package, scripts, and tests
notebooks/      Supplementary executed exploratory notebook
 docs/          Architecture and analysis overview
presentations/  Final project presentation (PDF and PowerPoint)
```

The maintained implementation is the Python package in `battery_twin/`. The notebook is supplementary historical exploration and is not the primary implementation.

## Requirements

- Python 3.10 or newer
- Dependencies declared in `battery_twin/pyproject.toml`

## Install

From the repository root:

```bash
python -m pip install -e ./battery_twin
python -m pip install -e './battery_twin[dev]'
```

## Get the dataset

This project uses the NASA Ames Prognostics Center of Excellence (PCoE) lithium-ion battery aging dataset. The project report cites the NASA Ames Prognostics Data Repository:

<https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/>

A helper downloads the NASA-hosted Battery Data Set archive and can extract the four cells used by this project (B0005, B0006, B0007, and B0018):

```bash
cd battery_twin
python scripts/download_nasa.py --extract
```

The helper stores the archive and extracted files under `battery_twin/data/raw/`. Dataset files are intentionally not committed to this repository. Please review NASA's repository terms and cite Saha, B. & Goebel, K. (2007), *Battery Data Set (NASA Ames Prognostics Data Repository)*, NASA Ames Research Center, when reusing the data.

## Run a smoke test

From the repository root:

```bash
cd battery_twin
python scripts/run_poc.py --smoke-test --output-dir outputs/smoke
```

This uses synthetic data to check that the pipeline executes; it is not a scientific result.

## Run with NASA data

From `battery_twin/` after downloading the dataset:

```bash
python scripts/run_poc.py --data-dir data/raw --output-dir outputs/nasa_poc --epochs 40
```

For the final evidence workflow:

```bash
python scripts/tune.py --data-dir data/raw --output outputs/tuning/grid.json --epochs 40
python scripts/run_evidence.py --data-dir data/raw --grid outputs/tuning/grid.json --output-dir outputs/final_evidence --epochs 4 --seeds 7 17 27
```

These commands generate local outputs under `battery_twin/outputs/`. Generated metrics and figures are not included here; regenerate them from the downloaded data before citing results. In particular, do not treat the numeric claims in older review notes as verified by this repository snapshot.

## Tests

```bash
cd battery_twin
python -m pytest -q
```

## Scope and limitations

The current implementation uses measured terminal signals and capacity-derived labels, with soft degradation/consistency constraints. It does not claim to solve unobserved electrochemical state equations such as SEI growth directly from the NASA measurements. The dataset subset contains four cells, so results should be described as proof-of-concept evidence rather than broad cross-chemistry validation.

## Citation

Saha, B., & Goebel, K. (2007). *Battery Data Set (NASA Ames Prognostics Data Repository)*. NASA Ames Research Center. <https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/>

## License

No license is currently included. Add an appropriate license before publishing if you want others to have explicit permission to reuse the project code.
