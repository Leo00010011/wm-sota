---
citation_count: 16
citation_source: "Semantic Scholar, 2026-10-15 (value from existing note; live re-check returned HTTP 429)"
venue: "ICCV 2025"
date: 2025-04-14
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# Nexus: Decoupled Diffusion Sparks Adaptive Scene Generation (borderline)

- **Authors:** Yunsong Zhou*, Naisheng Ye*, William Ljungbergh, Tianyu Li, Jiazhi Yang, Zetong Yang, Hongzi Zhu, Christoffer Petersson, **Hongyang Li**
  - *Equal contribution. Primary contact: Yunsong Zhou.
- **Group role:** Hongyang Li is last author (OpenDriveLab). Co-authors are from SJTU, Zhejiang University, Zenseact and GAC R&D.
- **Date published:** 2025-10 (ICCV 2025). arXiv v1 2025-04-14.
- **Venue:** ICCV 2025 (CVF open access; the project bibtex says ICCV). The presentation type was not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2504.10485
  - CVF: https://openaccess.thecvf.com/content/ICCV2025/html/Zhou_Decoupled_Diffusion_Sparks_Adaptive_Scene_Generation_ICCV_2025_paper.html
  - Project: https://opendrivelab.com/Nexus
  - Code: https://github.com/OpenDriveLab/Nexus
- **Number of citations:** 16 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
Nexus is a diffusion transformer that generates and simulates multi-agent traffic layouts, meaning agent states over time on a map, not pixels. Its main idea is to decouple noise across tokens:

- Each agent-time token has its own noise level. In training, "noise-masking" lets some tokens (history or goals) stay clean while others are noised.
- At inference, a noise-aware schedule lets the scene be updated with new observations mid-denoising. This makes generation both reactive (like next-token prediction) and goal-directed (like full-sequence diffusion).

The authors also build Nexus-Data, 540 h of adversarial safety-critical scenarios simulated in MetaDrive. They describe the model as "a promising generative world model" and test it as the reactive environment in nuPlan closed-loop evaluation. It is kept as borderline because it is a layout-level traffic world model, not a sensor-level one.

## 1. Weights availability
- Per the README, a checkpoint is released on Hugging Face (the TODO "Nexus & checkpoint" is ticked). The exact HF repo was not opened.
- Code is Apache-2.0; Nexus-Data is CC BY-NC-SA 4.0.

## 2. Code
- https://github.com/OpenDriveLab/Nexus, about 126 stars. It includes training, evaluation and demo docs. A guidance tutorial is still a TODO item.

## 3. Datasets
- nuPlan, Waymo Open (sim-agent validation for unconditional generation), and Nexus-Data. Nexus-Data has 540 h of nuPlan-derived scenes with adversarial attackers (cut-in, sudden braking, collisions), filtered by a validity checklist; only 36.9% of attacks produce valid collisions before filtering.

## 4. Evaluation and downstream tasks
**Generation on nuPlan (Table 1):** 8 s futures from 2 s of history. Rroad = off-road rate (%), Rcol = collision rate (%), Mk = instability.

| Method | ADE ↓ (conditioned) | Rroad ↓ | Rcol ↓ | Mk ↓ | Rcol ↓ (free) | Mk ↓ (free) | Time (s) |
|---|---|---|---|---|---|---|---|
| Diffusion Policy | 7.80 | 13.9 | 14.92 | 12.71 | 16.88 | 9.30 | 6.59 |
| SceneDiffuser | 5.99 | 8.53 | 11.78 | 9.64 | 13.59 | 6.16 | 5.34 |
| GUMP | 1.93 | 7.73 | 7.85 | 16.18 | 10.23 | 14.30 | 5.59 |
| Nexus | 1.28 | 6.89 | 1.62 | 4.63 | 2.61 | 3.23 | 2.79 |
| Nexus-Full | 1.12 | 6.25 | 1.56 | 3.17 | 2.10 | 2.14 | 2.93 |

- **"40% lower displacement error":** matches 1.12 vs GUMP's 1.93, a 42% reduction.

**As a world model for closed-loop evaluation (Table 4, nuPlan Val14, reactive score):** ground truth (Oracle) 82.8, Nexus 73.0, Diffusion Policy 61.6, SceneDiffuser 57.2.

**As a data engine (Table 5):**
- Setup: 3 h of nuPlan logs plus Nexus-synthesized data, used to train a Diffusion-Policy planner.
- Results (reactive score): 48.11 with real data only, 56.46 with 30× synthetic data, 57.86 with 60×. This is the "+20% closed-loop planning" claim.
- 3× synthetic data hurts performance (46.61).

## 5. Architecture and techniques
- **Model:** a DiT over structured agent tokens, with positional embeddings for both physical time and denoising time, conditioned on a global map tensor.
- **Training:** partial noise-masking, with independent per-token noise levels.
- **Inference:** a noise-aware schedule pops denoised tokens and pushes new ones (Fig. 2). Step skipping cuts sampling to 18 steps. Classifier guidance is used for goals and safety-critical attacks.
- **Visuals:** an initial attempt renders Nexus layouts into video with a modified NeRF-based driving reconstruction.

## 6. Conclusions and future work
- **Authors:** Nexus works as both a closed-loop world generator and a data engine. Future work: extending to video generation, end-to-end driving, and continuous online learning.
- **Own observation:**
  - The evaluation is at layout level only.
  - The data-augmentation gain comes from a small 3 h real-data regime with a lightweight planner.
  - The world-model use (Table 4) still trails the ground-truth environment by about 10 points.

## 7. Sources and verification
- **Read:** full arXiv v1 PDF (pdftotext), GitHub README and API metadata, a web search confirming the ICCV 2025 CVF listing, Semantic Scholar API.
- **Not verified:** the HF checkpoint URL and the ICCV presentation type.
