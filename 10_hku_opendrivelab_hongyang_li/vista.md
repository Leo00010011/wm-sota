---
citation_count: 414
citation_source: "Semantic Scholar, 2026-10-15"
venue: "NeurIPS 2024"
date: 2024-05-27
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# Vista: A Generalizable Driving World Model with High Fidelity and Versatile Controllability

- **Authors:** Shenyuan Gao, Jiazhi Yang, Li Chen, Kashyap Chitta, Yihang Qiu, Andreas Geiger†, Jun Zhang†, **Hongyang Li**†
  - The paper does not define the † mark. It is read here as corresponding or advising authors.
- **Group role:** Hongyang Li is last author (OpenDriveLab and HKU). The first author is at HKUST under Jun Zhang, with co-advising by Geiger (Tübingen).
- **Date published:** 2024-12 (NeurIPS 2024). arXiv v1 2024-05-27, v5 2024-10-28.
- **Venue:** NeurIPS 2024 (arXiv comment, README and bibtex). The presentation type was not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2405.17398
  - Code: https://github.com/OpenDriveLab/Vista
  - Weights: https://huggingface.co/OpenDriveLab/Vista
  - Demo: https://vista-demo.github.io
- **Number of citations:** 414 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
Vista turns Stable Video Diffusion (SVD) into a front-camera driving world model. It predicts 576×1024 video at 10 Hz and supports long rollouts (15 s shown). Its main ingredients are:

- **Latent-replacement conditioning** on up to 3 past frames, which serve as position, velocity and acceleration priors.
- **Two auxiliary losses:**
  - a dynamics-enhancement loss that up-weights regions with motion error;
  - a structure-preservation loss on high-frequency (FFT high-pass) latent features.
- **Action conditioning** in a second phase through LoRA and cross-attention projections. It supports steering angle and speed, trajectory, command, and goal point, learned jointly on OpenDV-YouTube (no actions) and nuScenes.

Vista also defines a reward that needs no ground-truth action: the negative conditional variance of its own predictions over several samples.

## 1. Weights availability
- **v1.0 weights:** `vista.safetensors` on Hugging Face (OpenDriveLab/Vista) and Google Drive, released 2024-06-06. The README warns that an earlier upload had an EMA-merging bug and asks users to re-download.
- **Unreleased:** a planned improved checkpoint (larger batch, more iterations) is still a TODO item.

## 2. Code
- https://github.com/OpenDriveLab/Vista, Apache-2.0, about 908 stars. It is built on Stability AI's generative-models.
- It includes installation, training and sampling docs. Memory-efficient training and an online demo are TODO items.

## 3. Datasets
- **OpenDV-YouTube** (from GenAD): about 1,735 h after manually removing 15 h, with no action labels.
- **nuScenes:** training set, rebalanced by command category, used for action learning.
- **Evaluation:** nuScenes validation (5,369 clips for FID and FVD), Waymo Open (unseen), CODA (corner cases) and OpenDV-YouTube-val.

## 4. Evaluation and downstream tasks
**Fidelity on nuScenes (Table 2):**

| Model | FID ↓ | FVD ↓ |
|---|---|---|
| DriveGAN | 73.4 | 502.3 |
| DriveDreamer | 52.6 | 452.0 |
| WoVoGen | 27.6 | 417.7 |
| Drive-WM | 15.8 | 122.7 |
| GenAD | 15.4 | 184.0 |
| **Vista** | **6.9** | **89.4** |

- **Headline gains:** "+55% FID / +27% FVD" are relative reductions against two different baselines.
  - FID: 6.9 vs GenAD's 15.4, a 55% reduction.
  - FVD: 89.4 vs Drive-WM's 122.7, a 27% reduction.
  - The baseline numbers are copied from each paper, because no baseline model was public.
- **Human evaluation (2AFC):**
  - Setup: 60 scenes from OpenDV-val, nuScenes, Waymo and CODA; 33 participants; 2,640 answers.
  - Comparison: against SVD, I2VGen-XL and DynamiCrafter on visual quality and motion rationality.
  - Result: the abstract says Vista wins "in over 70% of comparisons" against the strongest general video generator.
- **Action control (Table 3):** trajectory difference, measured by an inverse dynamics model over 2 s, with 3 dynamic priors.
  - nuScenes: 1.820 action-free, 1.585 with goal point, 1.593 with command, 0.832 with angle and speed, 0.835 with trajectory, against 0.379 for ground-truth video.
  - Waymo (zero-shot): 2.052 action-free, 1.902 with command, 1.140 with trajectory, against 0.893 for ground-truth video.
  - More priors reduce the error consistently. For example, the nuScenes action-free error drops from 3.785 with one prior to 1.820 with three.
- **Reward:** on 1,500 Waymo cases with perturbed trajectories, the average reward falls as L2 deviation grows (Fig. 10). This is qualitative evidence only; no planning benchmark is run.
- **Ablations:** dynamic priors (Table 3, Fig. 11). The two losses are ablated qualitatively only (Fig. 12). Appendix D also covers LoRA and the action-independence constraint.

## 5. Architecture and techniques
- **Backbone:** SVD UNet, 2.5B parameters in total with 1.6B in the UNet. Clips are 25 frames using the EDM framework, sampled with 50-step DDIM.
- **Phase 1:** full UNet at 576×1024, 20K iterations on 128 A100 GPUs (about 8 days), effective batch size 256, loss weights λ1 = 1.0 and λ2 = 0.1.
- **Phase 2:** UNet frozen, LoRA rank 16 plus zero-initialized cross-attention projections. Training runs 120K iterations at 320×576, then 10K at 576×1024, on 8 A100 GPUs for about 10 days. Only one action mode is active per sample, and OpenDV and nuScenes are sampled 1:1.
- **Rollouts:** autoregressive, re-seeding with the last 3 predicted frames.

## 6. Conclusions and future work
- **Authors:** limitations are compute cost, quality degradation over long horizons or drastic view shifts, failures on ambiguous commands and goal points, and limited training-data scale. The model is front-view only. Future work: scalable architectures (DiT), distillation and faster sampling.
- **Own observation:**
  - FID and FVD gains come from copied baseline numbers at different native resolutions and frame rates, so they are only indicative.
  - Controllability is measured with a learned inverse dynamics model.
  - The reward function is not tested on a closed-loop or NAVSIM planning task in this paper; ReSim later addresses this.

## 7. Sources and verification
- **Read:** full arXiv v5 PDF (main text and appendix C), GitHub README and API metadata, Semantic Scholar API.
- **Not verified:** NeurIPS presentation type, and the exact human-evaluation percentages per baseline (Fig. 7 values could not be reliably matched from the PDF text).
