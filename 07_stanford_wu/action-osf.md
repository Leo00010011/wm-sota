---
citation_count: 14
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; not re-verified on 2026-10-15 because the API returned HTTP 429)"
venue: "CVPR 2023"
date: 2023-06-14
research_group: "Stanford (Jiajun Wu)"
---

# Multi-Object Manipulation via Object-Centric Neural Scattering Functions

- **Authors:** Stephen Tian*, Yancheng Cai*, Hong-Xing Yu, Sergey Zakharov, Katherine Liu, Adrien Gaidon, Yunzhu Li, **Jiajun Wu**. (*equal contribution)
- **Group role:** senior (last) author. Yunzhu Li, PI of summary group 8, is second-to-last.
- **Date published:** 2023-06. CVPR 2023 was held in Vancouver in June 2023; the arXiv preprint appeared 2023-06-14.
- **Venue:** CVPR 2023. DOI 10.1109/CVPR52729.2023.00871. The arXiv comment says "Accepted at CVPR 2023". Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2306.08748
  - Project page: https://s-tian.github.io/projects/actionosf/
- **Number of citations:** 14 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper builds a structured visual world model for multi-object pushing. It has three parts:

- **Object representation.** Each object is a relightable, object-centric neural scattering function (KiloOSF). This allows composed rendering, including shadows, under new lighting.
- **State estimation and dynamics.** Object 6D poses and the light position are recovered by inverting the renderer with CMA-ES. A GNN then predicts future poses conditioned on pusher actions.
- **Planning.** MPPI scores imagined futures by rendering them with the OSFs and comparing them to a goal image (visual-foresight style).

## 1. Weights availability
Not found. The project page has no GitHub, Hugging Face or Drive links (hrefs checked). No checkpoints are mentioned in the paper.

## 2. Code
No code found. The project page links no repository, and I found no repo in my checks.

## 3. Datasets
- **Training data (dynamics model and compositional-NeRF baseline):** 1,000 simulated PyBullet trajectories.
  - Each trajectory has 3 random YCB objects from a 20-object subset.
  - A cylinder pusher takes 50 random actions, with velocity sampled from U(−0.3, 0.3) m/s and each action applied for about 0.2 s.
- **OSF training data:** per-object multi-view images paired with light poses.
- **Real-world data:** images under a single spotlight, used for qualitative lighting and pose estimation only.
- **Release status:** no dataset release found.

## 4. Evaluation and downstream tasks
- **Reconstruction (Fig. 5):** KiloOSF renders lighting changes and shadows better than compositional NeRFs (Driess et al.). This comparison is qualitative.
- **Long-horizon prediction (Fig. 6):** GNN + OSF rendering stays sharp beyond about 10 steps. FitVid (128×128, trained on the same 1,000 trajectories with randomized lighting) degrades. This comparison is qualitative.
- **Model-predictive control (Fig. 7):**
  - Setup: 20 tasks, 3 random objects each, random unknown light, goal at least 0.075 m from the start, 30 MPC steps.
  - Metric: the fraction of trials with final 6D pose error below a threshold.
  - Result: the method beats FitVid + visual MPC and CompNeRF in the 3-object (training) setting.
  - It also wins in the unseen 2-object and 4-object settings. That evaluation supplies object masks at all steps.
  - Results are reported only as curves, with no table.
- **Real world (Fig. 8):** lighting and pose estimation, with PoseCNN initialization. Qualitative only.
- **Downstream task:** visual MPC for multi-object rearrangement.

## 5. Architecture and techniques
- **Rendering:** KiloOSF, a KiloNeRF-style set of many small MLPs per object. Each predicts density and cumulative radiance transfer ρ(x, ω_light, ω_out). Rendering is compositional volume rendering, with shadow mapping.
- **Inverse estimation:** CMA-ES over object poses (position plus quaternion) and light position.
  - Stage 1: masked per-object MSE with the light fixed overhead.
  - Stage 2: whole-scene MSE, jointly optimizing poses and light.
  - Later MPC steps run without masks, with a reduced CMA search range.
- **Dynamics:** a GNN with one node per object (7-D pose) plus the pusher action a ∈ R³.
  - Edges are added dynamically when objects are closer than κ, a threshold derived from point clouds extracted from the OSF densities.
  - Message passing is multi-step. Edge encoders and propagators are shared.
  - Training uses MSE on poses over up to 3 future steps.
- **Planner:** MPPI. The cost is the ℓ2 distance between images rendered from the goal viewpoint and the goal image.

## 6. Conclusions and future work
- **Authors' conclusions:** inverse rendering with relightable OSFs plus GNN dynamics enables MPC under harsh, unseen lighting and with new object counts. It outperforms pixel-space video prediction.
- **Limitations stated by the authors:**
  - One KiloOSF must be trained per object.
  - The lighting model is simple.
  - Only rigid objects are handled.
- **Own observations:**
  - Control results come entirely from simulation (PyBullet). Real-world use is limited to perception.
  - There is no code or weight release.
  - The quantitative control comparison is plot-only.

## 7. Sources and verification
- **Paper:** arXiv PDF v1, https://arxiv.org/pdf/2306.08748. Read Secs. 1–6, Algorithm 1 and Figs. 5–8.
- **Project page:** https://s-tian.github.io/projects/actionosf/ (checked for code links; none found).
- **Semantic Scholar API** (`paper/arXiv:2306.08748`): venue CVPR, plus the DOI above.
- **Not verified:** CVPR proceedings page numbers.
