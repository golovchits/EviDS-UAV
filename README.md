# EviDS-UAV

**Evidential Deep Learning for Multi-Modal Anti-UAV Detection: A Controlled Ablation**

MSc thesis project (University of Amsterdam, 2025–2026). This repository is the umbrella for three experiment forks that implement and evaluate evidential uncertainty estimation with Dempster-Shafer (DS) belief fusion across different anti-UAV sensing modalities.

---

## Experiments

| ID | Modalities | Task | Fork |
|----|-----------|------|------|
| E1 | Thermal (IR) | Single-object tracking | [EDTC-EviDS](https://github.com/golovchits/EDTC-EviDS) |
| E2 | RGB video + Audio + RF | Multimodal classification | [TRIDENT-EviDS](https://github.com/golovchits/TRIDENT-EviDS) |
| E3 | RGB + IR + Event | Multi-object tracking | [MM-UAV-EviDS](https://github.com/golovchits/MM-UAV-EviDS) |

Each fork is a modified version of the original codebase extended with evidential heads and DS fusion.

---

## Framework overview

The EviDS approach attaches an **evidential head** (Dirichlet parameterisation via softplus) to each modality stream and combines per-modality belief masses using **Dempster's rule of combination**:

```
evidence  e = softplus(logits)
strength  S = sum(e) + K          (K = number of classes)
belief    b = e / S
uncertainty u = K / S
```

Fusion selects DS combination for complementary modalities and a gated uncertainty-weighted average for redundant ones.

---

## Setup

See [`docs/setup.md`](docs/setup.md) for environment and data setup.

Pretrained checkpoints: [`docs/checkpoints.md`](docs/checkpoints.md)

---

## Repository structure

```
EviDS-UAV/           ← this umbrella (coordination + docs)
  docs/
    setup.md         ← environment / data paths
    checkpoints.md   ← model download links
  LICENSE

# Experiment forks (separate repos):
EDTC-EviDS/          ← E1 thermal SOT
TRIDENT-EviDS/       ← E2 multimodal classification
MM-UAV-EviDS/        ← E3 multimodal MOT
```

---

## Citation

If you use this work, please cite the thesis and the original upstream codebases listed in each fork's README.

```bibtex
@mastersthesis{golovchits2026evids,
  author  = {Dmitry Golovchits},
  title   = {Evidential Deep Learning for Multi-Modal Anti-UAV Detection: A Controlled Ablation},
  school  = {University of Amsterdam},
  year    = {2026}
}
```

---

## Upstream acknowledgements

- **EDTC** (E1): [original repository](https://github.com/xuefeng-zhu5/EDTC)
- **TRIDENT** (E2): [original repository](https://github.com/TRIDENT-2025/TRIDENT)
- **MM-UAV-Benchmark** (E3): [original repository](https://github.com/JJGU2291/MM-UAV-Benchmark)
