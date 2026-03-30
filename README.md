![Banner](docs/images/cortexA_banner_image.png)


# CortexA 🧠
**Predicting future neurodegeneration states from 3D MRI using generative diffusion — built for clinical-grade speed.**

> **Status: Work in Progress (WIP).**  
> CortexA is under active development. Expect breaking API changes, shifting model architectures, and evolving inference/serving strategies as performance work progresses.

---

## Overview
**CortexA** forecasts future **neuro-degeneration states** from **3D MRI volumes** using **generative diffusion models**. The focus is twofold:

1. **Clinical-grade AI** (high-resolution volumetric reasoning, robust preprocessing, calibrated outputs)
2. A **highly reactive, “zero-latency” medical UI** (fast enough to feel interactive)

We’re optimizing a massive volumetric inference pipeline on **NVIDIA B200 GPUs** to drive end-to-end processing times down from tens of minutes to **seconds**.

---

## What CortexA Does
- Ingests 3D MRI scans (volumetric neuroimaging)
- Standardizes and preprocesses volumes (spacing, orientation, intensity normalization)
- Runs a **3D latent diffusion** pipeline to model progression trajectories
- Produces:
  - predicted future-state volumes (or latent representations)
  - progression risk indicators / change maps (planned)
  - outputs designed for real-time UI rendering (streaming-friendly)

> Note: This is a research/engineering project and is **not** a medical device. Any clinical usage requires validation, regulatory considerations, and appropriate oversight.

---

## Why This Matters
Current neuroimaging AI often sacrifices resolution or responsiveness:
- downsampling loses clinically relevant detail
- slow inference kills interactivity
- VRAM limits prevent full-fidelity volumetric processing

CortexA is built to keep **resolution high** while making inference **fast enough for a clinician-facing workflow**.

---

## Core Tech Stack
### Languages
- **Python**
- **C++** (performance-critical inference/IO components)

### Libraries
- **MONAI** (medical imaging pipelines, transforms, evaluation)
- **PyTorch** (training + experimentation)
- **TensorRT** (production inference acceleration)

### Frameworks / Models
- **3D Diffusion** (generative forecasting over volumes)
- **3D U-Net** (backbone blocks / denoiser architecture)

### Algorithmic Motifs
- **3D convolutions**
- **Attention mechanisms** (global context over volumetric features)

---

## Data
### Dataset
- **ADNI** (Alzheimer’s Disease Neuroimaging Initiative) open MRI dataset

### Model Notes (High Level)
- MONAI-based preprocessing/feature extraction
- Custom **3D latent diffusion** model for progression modeling
- Target: high-resolution inference without destructive downsampling

---

## Current Performance & Known Limitations (WIP)
- **Inference > 40 minutes** on consumer GPUs
- Requires **heavy downsampling** to fit VRAM limits
- **VRAM bottlenecks** block high-resolution clinical-grade processing
- Goal: leverage **B200 Tensor Cores** + optimized kernels to enable:
  - seconds-level inference
  - real-time-ish UI rendering (progressive outputs, streaming updates)

---

## Roadmap (Suggested)
### Modeling
- [ ] Validate diffusion objective against longitudinal MRI outcomes
- [ ] Calibrate uncertainty (credible intervals / ensemble or diffusion variance)
- [ ] Add change-map outputs (voxelwise deltas) for interpretability

### Inference & Acceleration
- [ ] TensorRT compilation path for denoiser + attention blocks
- [ ] Mixed precision (FP16/BF16) + kernel fusion
- [ ] Latent-space optimizations (smaller latent grids, smarter upsampling)
- [ ] Patch-wise inference with overlap/tiling to control VRAM
- [ ] Streaming inference to UI (progressive refinement)

### Product / UI
- [ ] “Instant preview” mode (quick low-step diffusion, refine on idle)
- [ ] Volume renderer integration (slice view + 3D view)
- [ ] Patient/session management + audit logs (if needed)

### Engineering
- [ ] Deterministic runs + reproducible pipelines
- [ ] Robust IO: NIfTI/DICOM handling (as applicable)
- [ ] Benchmarks: time/VRAM per volume and per resolution

---
