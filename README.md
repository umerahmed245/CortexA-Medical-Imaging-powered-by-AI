![Banner](docs/images/cortexA_banner_image.png)

# CortexA ⚪

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

## Roadmap (Suggested)

### **Modeling**
*   **Validate diffusion objective against longitudinal MRI outcomes:** Evaluate the model's ability to track disease-specific trajectories, such as **hippocampal shrinkage** or **ventricular enlargement**. Use metrics like **SSIM** (image similarity) and **MAE** (volumetric accuracy) to compare predicted follow-up scans against actual longitudinal data.
*   **Calibrate uncertainty via Latent Average Stabilization (LAS):** Improve spatiotemporal consistency by repeating the inference process multiple times and **averaging the results** to estimate a theoretical mean progression, reducing irregularities caused by random noise.
*   **Add change-map outputs for interpretability:** Integrate **Class Activation Maps (CAM)** or voxelwise deviation maps to visualize areas of significant longitudinal change, such as **cortical thinning** or lesion growth, providing clinicians with localized evidence for the model's predictions.

### **Inference & Acceleration**
*   **TensorRT compilation for hybrid architectures:** Focus optimization on **Transformer blocks** (self-attention/cross-attention) while maintaining a hybrid CNN-Transformer structure to ensure high-resolution spatial details are not lost during upsampling.
*   **Latent-space optimizations:** Utilize **AutoencoderKL** to compress 3D volumes (e.g., up to **170x compression**) before diffusion, significantly increasing inference throughput. 
*   **Patch-wise inference with overlap:** Implement a **sliding-window manner** for inference to manage the high VRAM demands of 3D medical scans, aggregating patch probabilities to obtain a final hard prediction.
*   **Streaming inference via next-scale prediction:** Transition from sequential next-token prediction to **parallel next-scale prediction**. This allows for **coarse-to-fine synthesis**, providing a usable "global" anatomy preview almost instantly while refining localized details in the background.

### **Product / UI**
*   **“Instant preview” mode:** Use the **hierarchical nature of next-scale prediction** to display initial low-resolution structural volumes immediately, aligning with how radiologists read scans (global anatomy before local details).
*   **Standardized Volume Renderer:** Ensure integration of the three primary planes—**Sagittal, Axial, and Coronal**—using the **neuraxis** as an orientation reference to help users navigate structures like the basal ganglia or hippocampus.
*   **Compliance and Security:** Implement **HIPAA-compliant encryption** for data transmissions and a **patient management system** that separates personally identifiable information in metadata files from clinical image data.

### **Engineering**
*   **BIDS Standardization:** Adopt the **Brain Imaging Data Structure (BIDS)** as the core organizational standard to ensure your pipeline is interoperable and reusable across different research and clinical sites.
*   **Robust IO (DICOM to NIfTI):** Build a pipeline that handles the idiosyncratic nature of **source DICOM data** (e.g., Siemens vs. Philips) and converts it into the **NIfTI format**, which is more efficient for day-to-day deep learning analysis.
*   **AID-RT Documentation & Benchmarking:** Generate a domain-specific **model card** (following AID-RT standards) that explicitly lists hardware/software requirements, **expected inference times**, and the **environmental impact** (carbon emissions) of your model.

---
