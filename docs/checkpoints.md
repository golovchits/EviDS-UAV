# Checkpoints

## E1 — EDTC-EviDS (Thermal SOT)

| Checkpoint | Description | Source |
|-----------|-------------|--------|
| `pretrained_models/yolo_edl_270.pt` | YOLOv5s + evidential head, epoch 270 | [Google Drive — to be added] |
| `pretrained_models/yolo_270.pt` | YOLOv5s baseline (sigmoid), epoch 270 | [Google Drive — to be added] |

Download and place under `EDTC-EviDS/pretrained_models/`.

---

## E2 — TRIDENT-EviDS (Multimodal classification)

| Directory | Description | Source |
|-----------|-------------|--------|
| `save_unimodals/` | Baseline unimodal checkpoints (3 seeds) | [Google Drive — to be added] |
| `save_unimodals_edl/` | EDL unimodal checkpoints (3 seeds) | [Google Drive — to be added] |
| `save_tri-modals/` | DS fusion checkpoints (3 seeds) | [Google Drive — to be added] |

Download and place under `TRIDENT-EviDS/`.

---

## E3 — MM-UAV-EviDS (Multimodal MOT)

Stage 1 (feature extractor) and conditions (a) and (b) use the **original authors' checkpoints**:  
→ [MM-UAV authors' Google Drive](https://drive.google.com/drive/folders/1MaQk8MA_63wHam6Bwr44NYHIg4pPl1qd)

| Condition | Description | Source |
|-----------|-------------|--------|
| Stage 1 | `yolox_s_2_stream` — two-stream feature extractor | Authors' Drive (see above) |
| (a) def-tuning | `yolox_s_2_def-tuning-fusion-head` | Authors' Drive (see above) |
| (b) stn-tuning | `yolox_s_2_stn-tuning-fusion-head` | Authors' Drive (see above) |
| (c) evidential ADFM | `yolox_s_2_evidential_adfm` | [Google Drive — to be added] |
| (d) evidential average | `yolox_s_2_evidential_average` | [Google Drive — to be added] |
| (e) evidential DS | `yolox_s_2_evidential_ds` | [Google Drive — to be added] |

Download and place under `MM-UAV-EviDS/YOLOX_outputs/<condition>/`.
