# Python implementation

This directory contains the installable implementation of the Adaptive Physics-Informed Battery Digital Twin.

## Setup

```bash
python -m pip install -e .
python -m pip install -e '.[dev]'
```

## Dataset

Download the NASA Ames PCoE battery dataset using:

```bash
python scripts/download_nasa.py --extract
```

The required MATLAB files (`B0005.mat`, `B0006.mat`, `B0007.mat`, and `B0018.mat`) should be available in `data/raw/`. They are excluded from Git; see the repository-level README for source attribution and citation details.

## Run

```bash
python scripts/run_poc.py --smoke-test --output-dir outputs/smoke
python scripts/run_poc.py --data-dir data/raw --output-dir outputs/nasa_poc --epochs 40
```

The smoke test uses synthetic data and is only a pipeline check. For NASA experiments, download the data first. Additional tuning and evidence-generation commands are documented in the root README.

## Evidence boundary

The implementation uses measurements supported by the NASA records: voltage, current, temperature, time, and discharge capacity. It uses capacity-derived SOH/RUL labels and soft degradation/consistency constraints. The SEI-growth PDE is not implemented as a measured physical law in this dataset-based proof of concept.
