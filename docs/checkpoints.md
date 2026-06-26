# Checkpoints

All EviDS checkpoints are in a single Google Drive folder:  
→ **[EviDS-UAV Checkpoints on Google Drive](https://drive.google.com/drive/folders/1WNrKHYwXNDiX4JuhEb1rKxSO24fy-KWJ?usp=sharing)**

Each experiment fork also has its own `CHECKPOINTS.md` with the specific file listing.

---

## E1 — EDTC-EviDS (Thermal SOT)

| File | Description |
|------|-------------|
| `pretrained_models/yolo_edl_270.pt` | YOLOv5s + evidential head, epoch 270 |
| `pretrained_models/yolo_sigmoid_270.pt` | YOLOv5s baseline (sigmoid), epoch 270 |

Place under `EDTC-EviDS/pretrained_models/`.

---

## E2 — TRIDENT-EviDS (Multimodal classification)

| Directory | Description |
|-----------|-------------|
| `save_unimodals/` | Baseline unimodal checkpoints (3 seeds × 3 modalities) |
| `save_unimodals_edl/` | EDL unimodal checkpoints (3 seeds × 3 modalities) |
| `save_tri-modals/` | DS fusion checkpoints (3 seeds) |

Place under `TRIDENT-EviDS/`.

---

## E3 — MM-UAV-EviDS (Multimodal MOT)

Stage 1 (feature extractor) and conditions (a) and (b) use the **original authors' checkpoints**:  
→ [MM-UAV authors' Google Drive](https://drive.google.com/drive/folders/1MaQk8MA_63wHam6Bwr44NYHIg4pPl1qd)

| Condition | Directory |
|-----------|-----------|
| Stage 1 | `yolox_s_2_stream` — two-stream feature extractor |
| (a) def-tuning | `yolox_s_2_def-tuning-fusion-head` |
| (b) stn-tuning | `yolox_s_2_stn-tuning-fusion-head` |

EviDS conditions (c)–(e) are in the main EviDS Drive folder above:

| Condition | Directory |
|-----------|-----------|
| (c) evidential ADFM | `yolox_s_2_evidential_adfm` |
| (d) evidential average | `yolox_s_2_evidential_average` |
| (e) evidential DS | `yolox_s_2_evidential_ds` |

Place under `MM-UAV-EviDS/YOLOX_outputs/<condition>/`.
