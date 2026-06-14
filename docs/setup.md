# Setup

## Environments

Each experiment has its own conda/venv environment. See the individual fork READMEs for full dependency lists.

| Experiment | Environment file |
|-----------|-----------------|
| E1 — EDTC-EviDS | `environment.yml` (YOLOv5 + custom EDL extensions) |
| E2 — TRIDENT-EviDS | `TRIDENT_env.yml` |
| E3 — MM-UAV-EviDS | `environment.yml` |

---

## Datasets

### E1 — AntiUAV600

Download from [Google Drive](https://drive.google.com/drive/folders/1igFX0yISt4auaH7ijHde1pBfvna8nn3G?usp=sharing) and set `DATA_PATH` in `local.py` accordingly.

### E2 — TRIDENT dataset

Download from the [TRIDENT project](https://github.com/TRIDENT-2025/TRIDENT) and place under `TRIDENT-EviDS/dataset/`.

### E3 — MM-UAV

The MM-UAV dataset (RGB + IR + Event sequences) must be downloaded separately.  
See the [MM-UAV-Benchmark README](https://github.com/JJGU2291/MM-UAV-Benchmark) for instructions.

Set the following environment variables before running SLURM jobs:
```bash
export DATA_DIR=/path/to/MM-UAV-images
export SCRATCH_DIR=/path/to/scratch
```

---

## Running on an HPC cluster (SLURM)

All job scripts are in `jobs/` within each fork. They use parameterised paths and work on any SLURM-based cluster:

```bash
# Required env vars (set these before sbatch)
export CODE_DIR=/path/to/repo        # defaults to repo root auto-detected from script location
export VENV_PATH=/path/to/your/venv  # your conda/venv prefix
export DATA_DIR=/path/to/dataset     # cluster dataset location
```

Submit from the repo root:
```bash
cd /path/to/TRIDENT-EviDS
sbatch jobs/run_edl_fusion.slurm

cd /path/to/MM-UAV-EviDS
sbatch jobs/run_stage2_evidential_ds.slurm
```

---

## Local evaluation

Tracker results (`.txt` files) are committed directly in `MM-UAV-EviDS/YOLOX_outputs/<condition>/track_results_*/`.  
Run evaluation against the MM-UAV-Evaluation-ToolKit separately:

```bash
bash setup_eval.sh   # copies GT + tracker results into eval toolkit structure
```

See `setup_eval.sh` for required `EXTERNAL`, `EVAL_SRC`, and `TRACKER_RESULTS` path variables.
