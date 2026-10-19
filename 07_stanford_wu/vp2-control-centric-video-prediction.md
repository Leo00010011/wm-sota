---
citation_count: 34
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live re-check returned HTTP 429 on 2026-10-15)"
venue: "ICLR 2023"
date: 2023-04-26
research_group: "Stanford (Jiajun Wu)"
---

# A Control-Centric Benchmark for Video Prediction (VP²)

- **Authors:** Stephen Tian, Chelsea Finn, **Jiajun Wu**
- **Group role:** senior (last) author. Chelsea Finn (named in summary group 9) is the middle author.
- **Date published:** 2023-05 (ICLR 2023 conference, May 2023; arXiv 2023-04-26)
- **Venue:** ICLR 2023 (the PDF header reads "Published as a conference paper at ICLR 2023"). Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2304.13723
  - Project page: https://s-tian.github.io/projects/vp2/
  - Code: https://github.com/s-tian/vp2
  - Data: https://huggingface.co/datasets/s-tian/VP2 and the Stanford Digital Repository
- **Number of citations:** 34 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
VP² is a benchmark that scores action-conditioned video prediction models (pixel-space world models) by how well they support planning. It does not use perceptual metrics. Each model is plugged into a fixed visual-foresight MPC loop (MPPI planner, given cost functions). Success is measured on 11 simulated manipulation task categories with 310 task instances.

The paper first shows that FVD, LPIPS and SSIM can disagree with control success. It then uses VP² to study model scale, data scale and ensemble-based uncertainty.

## 1. Weights availability
- **Released.** The benchmark download includes pretrained weights for SVG′, FitVid, MCVD and Struct-VRNN, and the classifier cost-function weights for RoboDesk.
- **Download locations:**
  - https://purl.stanford.edu/qf310mj0842 (19.6 GB)
  - Hugging Face mirror (added March 2025): https://huggingface.co/datasets/s-tian/VP2/tree/main/vp2_benchmark
- **Usage:** models are loaded through `vp2/models` wrappers and Hydra configs (see the README).
- **License:** not declared.

## 2. Code
- **Repository:** https://github.com/s-tian/vp2. Last commit 2025-03-03. No LICENSE file (raw `LICENSE` returns 404).
- **Contents:**
  - Environments: robosuite with the iGibson renderer, and RoboDesk.
  - Task instances.
  - MPC agents: MPPI, CEM, CEM-GD, L-BFGS.
  - Cost classifiers.
  - Model interfaces.
  - Experiment commands for the paper's case study.
- **Model training code:** lives in separate repos, e.g., https://github.com/s-tian/svg-prime.

## 3. Datasets
- **robosuite tabletop:** 50K scripted push trajectories, 35 steps each, 256×256 RGB, randomized textures. 5K trajectories are rendered; the rest can be rendered from low-dimensional state.
- **RoboDesk:** 5K noisy scripted trajectories per task, 35K in total.
- **Availability:** public at https://purl.stanford.edu/cw843qn4148 (182 GB) and on Hugging Face (`vp2_training_datasets`).
- **Task instances:**
  - robosuite: 4 push categories × 25 instances (100 instances).
  - RoboDesk: 7 categories (push 3 buttons, open slide or drawer, push upright or flat block off the table).
  - The paper states 310 instances in total.

## 4. Evaluation and downstream tasks
- **Metric:** control success rate under visual-foresight MPC. Each model gets 2 context frames and predicts 10 future frames. A planner using the true simulator gives the upper bound.
- **Metric mismatch (Table 1).** Example on robosuite pushing with FitVid:
  - MSE-trained: FVD 30.7, success 65%.
  - With an LPIPS loss (weight 10): FVD 24.3 (better), but success drops to 35%.
  - How well metrics correlate with success depends on the task.
- **Baselines (Fig. 3):** FitVid, SVG′, MCVD (diffusion, made action-conditioned), Struct-VRNN and MaskViT. Near-simulator performance on "push blue button"; large gaps on robosuite and the other RoboDesk tasks. Results are in a figure, with no table.
- **Model scale (Fig. 4, Table 2):**
  - Models from 2.3M to 325M parameters show no strong trend; full FitVid (302M) may overfit.
  - Prediction time for 10 frames (batch 200) is 5.63 s for full FitVid and 220 s for MCVD.
- **Data scale (Fig. 5):** going from 1K to 50K trajectories helps a little, then plateaus.
- **Ensembles (Fig. 6):** an N = 4 ensemble-disagreement penalty helps on "upright block off table". It is roughly neutral or slightly worse on the other two tasks tested.
- **Downstream task:** model-based planning (visual MPC) for robot manipulation.

## 5. Architecture and techniques
- **Benchmark design:** the only requirement on a model is the call `__call__(context_frames, action_seq) -> predictions`.
- **Planner:** MPPI, with hyperparameters tuned using a perfect model.
- **Costs:** pixel MSE for robosuite. For RoboDesk, MSE plus the logits of a pretrained convolutional task-success classifier.
- **Action spaces:**
  - robosuite: 4-D (end-effector Δxyz plus gripper), sent to an operational-space controller (OSC).
  - RoboDesk: 5-D.
- **Models studied:** variational RNNs (FitVid, SVG′), a masked transformer (MaskViT), a diffusion model (MCVD) and a keypoint model (Struct-VRNN).

## 6. Conclusions and future work
- **Authors' conclusions:**
  - Perceptual metrics can mis-rank models for control, and control-centric evaluation adds a separate axis.
  - Scale helps only to a degree. Uncertainty awareness is a promising direction.
- **Authors' stated limitations:**
  - The tasks are simulated and short-horizon.
  - The evaluation is computationally expensive.
  - Manipulation is only one downstream use of video prediction.
- **Own observations:**
  - The conclusion of the paper says "13 task categories", while Sec. 4 lists 11 (4 + 7). This is an internal inconsistency in the paper.
  - Results depend on the fixed planner and costs, so a model's ranking may change with other controllers.
  - Most quantitative results are reported only in figures.

## 7. Sources and verification
- **Sources read:**
  - arXiv PDF https://arxiv.org/pdf/2304.13723: Secs. 1–7, Tables 1–2, Figs. 1–6.
  - Repo README: https://github.com/s-tian/vp2
  - Semantic Scholar API `paper/arXiv:2304.13723`: venue ICLR.
- **Not verified:** the OpenReview forum page and presentation type (not opened).
