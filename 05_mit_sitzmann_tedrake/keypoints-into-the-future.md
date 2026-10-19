---
citation_count: 113
citation_source: "Semantic Scholar, 2026-10-15"
venue: "CoRL 2020"
date: 2020-09-10
research_group: "MIT (Vincent Sitzmann, Russ Tedrake)"
---

# Keypoints into the Future: Self-Supervised Correspondence in Model-Based Reinforcement Learning

- **Authors:** Lucas Manuelli, Yunzhu Li, Pete Florence, **Russ Tedrake**
- **Group role:** Senior author (Tedrake last author). First author Manuelli was a Tedrake PhD student. Yunzhu Li (now Columbia, group 8) is the second author.
- **Date published:** 2020-11 (CoRL 2020, held virtually / Cambridge MA, 16 to 18 November 2020; PMLR vol. 155 published 2021); arXiv 2020-09-10
- **Venue:** Conference on Robot Learning (CoRL) 2020, PMLR 155:693–710
- **Links:** arXiv https://arxiv.org/abs/2009.05085 · PMLR https://proceedings.mlr.press/v155/manuelli21a.html · Project https://sites.google.com/view/keypointsintothefuture
- **Number of citations:** 113 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper learns a latent dynamics (world) model for vision-based manipulation in which the latent state is a set of 3D object keypoints. The keypoints are tracked by a self-supervised dense-correspondence network (Dense Object Nets). An MLP predicts future keypoint positions given actions, and MPPI-based MPC uses this model for closed-loop pushing. It is compared against autoencoder and Transporter latent world models, and is validated on a real Kuka arm with about 10 minutes of interaction data.

## 1. Weights availability
Not released. The project page lists "Code (coming soon)" with no link (checked 2026-10-13), and no checkpoints are linked from the paper, PMLR page or project page.

## 2. Code
No code found. Checked: project page ("Code (coming soon)"), PMLR page, and a web search for a GitHub repository (no match).

## 3. Datasets
- **Simulation:** four author-built pushing tasks (top-down camera, angled camera, occlusions, mugs category-level). Evaluated on 200 initial/goal states per task. Not released.
- **Hardware:** RGBD images from two RealSense D415 cameras for correspondence training, plus about 10 minutes of random-pushing interaction data on a Kuka IIWA for dynamics. Not released.

## 4. Evaluation and downstream tasks
- **Metrics:** final position error (cm) and orientation error (degrees) from the goal pose after closed-loop MPC, averaged over 200 episodes per task.
- **Baselines (Table 2):** Transporter keypoints (2D and 3D), a jointly trained convolutional autoencoder world model, and a ground-truth 3D state oracle (Table 1).
- **Key numbers, position cm / angle deg (Tables 1–2):**
  - Top-down: SDS 1.19/3.88 vs Transporter-3D 2.01/15.95, autoencoder 2.18/14.79; GT 3D 1.14/4.01.
  - Occlusions: SDS 1.46/6.77, WSDS 1.28/5.59 vs Transporter-3D 3.72/20.65, autoencoder 3.08/14.20.
  - Mugs (category): WDS 1.00/13.29 vs Transporter-3D 2.81/61.22, autoencoder 9.05/56.84.
- **Hardware (Sec. 4.4, Fig. 4):** tracks 4 reference keypoint trajectories from a single demonstration with a 2 s MPC horizon (H=10 at 5 Hz). The region of attraction is up to 60° initial-pose perturbation for the easiest trajectory and smaller for a 180° reorientation. Results are given as scatter plots.
- **Downstream tasks:** model-based planning (MPC pushing to goal), category-level generalisation, one-shot imitation via keypoint trajectory tracking.

## 5. Architecture and techniques
- **Encoder:** a pretrained, frozen dense-correspondence network (Dense Object Nets) produces a descriptor image. Reference descriptors are localised as keypoints (2D or 3D) by a parameter-free correspondence function.
- **Latent variants:**
  - DS: 50 random keypoints.
  - SDS: 4 to 5 high-confidence, spatially separated keypoints.
  - WDS: a learnable convex combination of keypoints, trained jointly with the dynamics.
  - WSDS: SDS and WDS combined.
- **Dynamics model:** an MLP with two hidden layers of 500 units, input z_{t−1:t} and action, predicting z_{t+1}. Trained with a multi-step simulation-error loss over horizon H (Eq. 1), Adam lr 1e-4, 1000 epochs. Uses data augmentation exploiting planar translation/rotation invariance (App. 7.1).
- **Planner:** MPPI (it beat random shooting, CEM and gradient shooting in their tests). Cost: distance to goal or reference keypoints.
- **Action:** planar end-effector velocity at 5 Hz.

## 6. Conclusions and future work
- **Authors:** self-supervised correspondence gives interpretable latent states that outperform autoencoder and Transporter latents, especially under occlusion and for category-level generalisation. The method works on hardware and stabilises long-horizon plans from one demonstration. Future work: handling full occlusion via longer-horizon models or higher-level planning (Sec. 3.1).
- **Own observations:**
  - Only planar pushing tasks are evaluated, and the state must be (nearly) fully observable from keypoints.
  - No code or data was released, so the work cannot be reproduced.
  - Hardware results are qualitative or scatter-plot only.

## 7. Sources and verification
- Paper PDF: https://arxiv.org/pdf/2009.05085 (read main text and App. 7)
- PMLR: https://proceedings.mlr.press/v155/manuelli21a.html (found via PMLR v155 index)
- Project page: https://sites.google.com/view/keypointsintothefuture
- Citations: https://api.semanticscholar.org/graph/v1/paper/arXiv:2009.05085
- Not verified: presentation track (oral/poster) at CoRL 2020 (not found).
