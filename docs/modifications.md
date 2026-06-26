# Modifications from Upstream

This document lists the changes made to each baseline codebase.

---

## E1 — EDTC-EviDS

**Upstream:** [xuefeng-zhu5/EDTC](https://github.com/xuefeng-zhu5/EDTC)
**Baseline:** EDTC — YOLOv5s detector + evidential tracker for thermal single-object tracking.

### Evidential detection head

The YOLOv5s detector originally uses a sigmoid head. The fork replaces it with
an evidential Dirichlet head (softplus activation → evidence → α parameters),
trained with the EDL loss (Bayes risk + KL regularisation with annealing).

| File | Change |
|------|--------|
| `yolov5/models/edl.py` | **New.** EDL loss implementation for the detector |
| `yolov5/models/yolo.py` | **Modified.** Output head: sigmoid → softplus + Dirichlet evidence |
| `yolov5/models/common.py` | **Modified.** Supporting changes for evidential output |

### Tracker (unchanged)

The tracker's evidential head (`ScoreDecoder` in `lib/models/uavtrack/score_decoder.py`)
and the detection-tracking switching logic (`lib/test/tracker/uavtrack_eh.py`) are
**kept as-is from upstream** — the fork only modifies the detector stage.

### Experiment configs

| File | Change |
|------|--------|
| `experiments/uavtrack_eh/edl_270.yaml` | **New.** EDL detector + tracker, epoch 270 |
| `experiments/uavtrack_eh/edl_270_taudet.yaml` | **New.** EDL + detector vacuity threshold τ_det |
| `experiments/uavtrack_eh/sigmoid_270.yaml` | **New.** Sigmoid baseline control, epoch 270 |
| `experiments/uavtrack/baseline.yaml` | **Modified.** Updated paths and hyperparameters |
| `experiments/uavtrack_eh/baseline.yaml` | **Modified.** Updated paths and hyperparameters |

### Dataset

| File | Change |
|------|--------|
| `antiuav_train_val_split_stratified.json` | **New.** Stratified train/val split for AntiUAV600 |

### Evaluation

| File | Change |
|------|--------|
| `lib/test/evaluation/antiuavtunedataset.py` | **New.** Evaluation dataset with tuned hyperparameters |
| `lib/test/tracker/device_config.py` | **New.** Device placement configuration |
| `lib/test/analysis/extract_results.py` | **Modified.** Extended result extraction |
| `lib/test/analysis/plot_results.py` | **Modified.** Additional plots (ECE, reliability, stratification) |

### Training infrastructure

| File | Change |
|------|--------|
| `jobs/train_yolo_edl.sh` | **New.** SLURM: train detector with EDL loss |
| `jobs/train_yolo_sigmoid.sh` | **New.** SLURM: train sigmoid baseline |
| `jobs/full_val.sh` | **New.** Full validation pipeline |
| `jobs/full_val_edl.sh` | **New.** Full validation for EDL variants |
| `jobs/compute_edl_metrics.sh` | **New.** Compute ECE, UAUC, reliability metrics |
| `jobs/run_tune_calibration.sh` | **New.** Temperature scaling calibration |
| `install_pytorch17.sh` | **Modified.** Updated for modern PyTorch |

---

## E2 — TRIDENT-EviDS

**Upstream:** [TRIDENT-2025/TRIDENT](https://github.com/TRIDENT-2025/TRIDENT)
**Baseline:** TRIDENT — tri-modal (RGB + Audio + RF) binary UAV classification with GMU and Late Fusion.

### Evidential heads

Each modality's sigmoid classification head is replaced with an evidential Dirichlet head.

| File | Change |
|------|--------|
| `models/edl_head.py` | **New.** Evidential Dirichlet head (softplus → evidence → α, S, u) |
| `models/edl_model.py` | **New.** EDL model wrapper replacing sigmoid head per modality |
| `models/aux_models.py` | **Modified.** Supporting model utilities for EDL |

### DS evidence fusion

The GMU and Late Fusion modules are replaced with Dempster-Shafer belief combination.

| File | Change |
|------|--------|
| `models/ds_fusion.py` | **New.** Iterative DS combination rule with conflict fallback |
| `models/av_fusion.py` | **Modified.** EDL-aware fusion routing |
| `models/get_model.py` | **Modified.** Model factory: instantiate EDL + DS variants |

### Temporal sensor gating

| File | Change |
|------|--------|
| `models/temporal_gate.py` | **New.** Uncertainty-thresholded per-modality gate with hysteresis |

### Training

| File | Change |
|------|--------|
| `edl_unimodal_train.py` | **New.** Train unimodal models with EDL loss |
| `edl_fusion_train.py` | **New.** Train DS fusion with pre-trained unimodal backbones |
| `training/unimodal_train.py` | **Modified.** EDL loss integration |
| `training/multimodal_train.py` | **Modified.** DS fusion training loop |
| `fusion_train.py` | **Modified.** CLI updates for EDL/DS variants |
| `fusion_test.py` | **Modified.** DS fusion inference |
| `unimodal_train.py` | **Modified.** CLI updates |
| `unimodal_test.py` | **Modified.** EDL inference |
| `utils/functions.py` | **Modified.** EDL metric computation |
| `utils/scheduler.py` | **Modified.** Annealing schedule for KL regulariser |

### Analysis and evaluation

| File | Change |
|------|--------|
| `analyse_results_v2.py` | **New.** Full analysis pipeline (accuracy, ECE, UAUC, bootstrap) |
| `compute_ece.py` | **New.** Expected Calibration Error computation |
| `compute_edl_metrics.py` | **New.** UAUC (vacuity/entropy), reliability diagrams |
| `compute_baseline_uauc.py` | **New.** Baseline uncertainty metrics for comparison |
| `calibrate_temperature.py` | **New.** Post-hoc temperature scaling |
| `benchmark_gating.py` | **New.** Gate sweep: 664 hyperparameter configurations |
| `final_canonical_recompute.py` | **New.** Canonical DS fusion recomputation |
| `regenerate_figure.py` | **New.** Regenerate composite figures |
| `save_probs.py` / `save_probs_edl.py` | **New.** Cache inference outputs for analysis |
| `noise_config.yaml` | **New.** Noise injection configuration for robustness testing |

### SLURM scripts

| File | Change |
|------|--------|
| `jobs/run_edl_unimodal.slurm` | **New.** Train unimodal EDL backbones |
| `jobs/run_edl_fusion.slurm` | **New.** Train DS fusion |
| `jobs/run_fusion_baseline.slurm` | **New.** Baseline fusion training |
| `jobs/run_fusion_eval.slurm` | **New.** Evaluate fusion models |
| `jobs/run_unimodal_baseline.slurm` | **New.** Baseline unimodal training |
| `jobs/run_unimodal_eval.slurm` | **New.** Evaluate unimodal models |
| `jobs/run_benchmark_gating.slurm` | **New.** Gate hyperparameter sweep |
| `jobs/run_clean_test.slurm` | **New.** Clean-data test |
| `jobs/run_save_probs_val.slurm` | **New.** Cache validation probabilities |

### Environment

| File | Change |
|------|--------|
| `TRIDENT_env.yml` | **Modified.** Updated dependencies |
| `setup_env.sh` | **New.** One-command environment setup |

---

## E3 — MM-UAV-EviDS

**Upstream:** [JJGU2291/MM-UAV-Benchmark](https://github.com/JJGU2291/MM-UAV-Benchmark)
**Baseline:** MMA-SORT — dual-stream (RGB + IR) detection with deformable-conv alignment
(ADFM/OGAA) and event-camera tracking.

### Evidential detection heads

The sigmoid objectness and classification heads in YOLOX are replaced with evidential
Dirichlet heads (softplus → evidence → α, uncertainty). Both RGB and IR streams
receive independent evidential heads.

| File | Change |
|------|--------|
| `yolox/models/yolo_head_evidential.py` | **New.** Evidential YOLOX head: softplus evidence, Dirichlet α, EDL loss with KL annealing, fp16-safe numerics (fp32 cast + nan_to_num + alpha clamp) |
| `yolox/models/yolo_head.py` | **Modified.** Added imports for evidential head |
| `yolox/models/__init__.py` | **Modified.** Register evidential head |

### DS evidence fusion and decision-level combination

Two fusion strategies are implemented at the decision level (post-head, pre-NMS):

| File | Change |
|------|--------|
| `yolox/models/ds_fusion.py` | **New.** DS belief combination: per-anchor α → belief masses → DS rule → fused α |
| `yolox/models/yolox2_average.py` | **New.** Probability averaging fusion (parameter-free ablation foil) |
| `yolox/models/yolox2_ds.py` | **New.** DS fusion wrapper: RGB + IR heads → DS combine → fused output |

### Fusion backbone variants

| File | Change |
|------|--------|
| `yolox/models/yolo_pafpn2_evidential_adfm.py` | **New.** Condition (c): ADFM alignment + evidential heads |
| `yolox/models/yolo_pafpn2_evidential_average.py` | **New.** Condition (d): decision-level probability averaging |
| `yolox/models/yolo_pafpn2_evidential_ds.py` | **New.** Condition (e): decision-level DS fusion |
| `yolox/models/yolo_pafpn2.py` | **Renamed** from `yolo_pafpn2_base.py` |

### Temporal sensor gating

| File | Change |
|------|--------|
| `yolox/models/temporal_gate.py` | **New.** Streak-triggered per-modality gate (parameter-free: no learnable weights) |

### Experiment configs (one per ablation condition)

| File | Change |
|------|--------|
| `yolox/exps/example/custom/yolox_s_2_evidential_adfm.py` | **New.** Condition (c) config |
| `yolox/exps/example/custom/yolox_s_2_evidential_average.py` | **New.** Condition (d) config |
| `yolox/exps/example/custom/yolox_s_2_evidential_ds.py` | **New.** Condition (e) config |
| `yolox/exps/example/custom/yolox_s_2_evidential_ds_gating.py` | **New.** Condition (f) config |
| `yolox/exps/example/custom/yolox_s_2_def-tuning-fusion-head.py` | **Modified.** Baseline condition (a) |
| `yolox/exps/example/custom/yolox_s_2_stn-tuning-fusion-head.py` | **Modified.** Baseline condition (b) |
| `yolox/exps/example/custom/yolox_s_2_pretrain.py` | **Modified.** Stage 1 pre-training |

### Snellius HPC adaptations

The original code assumes a standard POSIX filesystem with unrestricted CUDA
initialization order. The following changes enable training on Snellius
(Lustre scratch, A100 GPUs). These do not alter algorithmic behaviour.

| File | Change |
|------|--------|
| `yolox/data/datasets/coco2.py` | **Modified.** Tar-based data loading with byte-offset index to bypass Lustre inode limit |
| `yolox/data/data_prefetcher2.py` | **Modified.** Deferred CUDA stream creation (avoids cv2+CUDA init deadlock) |
| `yolox/core/trainer.py` | **Modified.** CUDA init after DataLoader; gradient clipping (max_norm=10.0); optimizer state device fix for resume |
| `yolox/exp/yolox_base2.py` | **Modified.** `pin_memory` conditional on `workers > 0` |
| `yolox/train.py` | **Modified.** `torch.multiprocessing.set_start_method('spawn')` for worker process isolation |
| `extract_images.py` | **New.** Extract annotation-referenced images from tar shards |
| `extract_test_frames.py` | **New.** Extract test frames for tracker evaluation |

### SLURM job scripts

All training, tracking, and evaluation scripts are in `jobs/`.

| File | Change |
|------|--------|
| `jobs/run_stage2_evidential_adfm.slurm` | **New.** Train condition (c) |
| `jobs/run_stage2_evidential_average.slurm` | **New.** Train condition (d) |
| `jobs/run_stage2_evidential_ds.slurm` | **New.** Train condition (e) |
| `jobs/run_stage2_baseline.slurm` | **New.** Train baseline controls |
| `jobs/run_tracker_evidential_adfm.slurm` | **New.** Run tracker for (c) |
| `jobs/run_tracker_evidential_average.slurm` | **New.** Run tracker for (d) |
| `jobs/run_tracker_evidential_ds.slurm` | **New.** Run tracker for (e) |
| `jobs/run_tracker_evidential_ds_gating.slurm` | **New.** Run tracker for (f) |
| `jobs/run_tracker_baseline.slurm` | **New.** Run tracker for (a) |
| `jobs/run_tracker_baseline_stn.slurm` | **New.** Run tracker for (b) |
| `jobs/run_e3_trackeval.slurm` | **New.** Run TrackEval on all conditions |
| `jobs/run_e3_analysis.slurm` | **New.** ECE, UAUC, reliability analysis |
| `jobs/run_e3_analysis_cde.slurm` | **New.** Analysis for conditions (c)/(d)/(e) |
| `jobs/run_e3_analysis_g.slurm` | **New.** Analysis for temperature scaling (g) |
| `jobs/run_temp_scale_full.slurm` | **New.** Full temperature scaling pipeline |
| `jobs/run_adfm_conf010_full.slurm` | **New.** Confidence threshold diagnostic for (c) |
| `jobs/run_adfm_sweep.slurm` | **New.** Confidence threshold sweep |
| `jobs/run_finetune_crops.slurm` | **New.** Crop-level control experiment |
| `jobs/run_crop_extraction.slurm` | **New.** Extract detection crops |
| `jobs/run_seq_cis.slurm` | **New.** Sequence-clustered confidence intervals |
| `jobs/train_yolox_2_stream.sh` | **Moved.** Stage 1 training |
| `jobs/train_yolox_2_stream_def.sh` | **Moved.** Stage 2 training — defconv |
| `jobs/train_yolox_2_stream_stn.sh` | **Moved.** Stage 2 training — STN |

### Analysis tools

| File | Change |
|------|--------|
| `tools/e3_inference_analysis.py` | **New.** ECE, UAUC, per-sequence metrics |
| `tools/fit_temperature.py` | **New.** Temperature scaling on baseline |
| `tools/recompute_sequence_cis.py` | **New.** Bootstrap confidence intervals |
| `tools/extract_detection_crops.py` | **New.** Crop extraction for control experiment |
| `tools/finetune_crops_edl.py` | **New.** Crop-level fine-tuning with EDL |
| `tools/masort-multi2-event.py` | **Modified.** Extended for evidential outputs |
| `tools/create_tars.py` | **New.** Create indexed tar shards for Snellius |

### Tracker

| File | Change |
|------|--------|
| `tracker/ma_sort2_event.py` | **Modified.** Evidential score handling |
| `tracker/ma_sort_event.py` | **Modified.** Event feature extraction updates |
| `tracker/matching.py` | **Modified.** Association with uncertainty |

### Evaluation setup

| File | Change |
|------|--------|
| `setup_eval.sh` | **New.** Copy tracker outputs + GT into evaluation toolkit structure |
