---
citation_count: 103
citation_source: "Semantic Scholar, 2026-10-15"
venue: "CoRL 2022"
date: 2022-02-24
research_group: "MIT (Vincent Sitzmann, Russ Tedrake)"
---

# Learning Multi-Object Dynamics with Compositional Neural Radiance Fields

- **Authors:** Danny Driess, Zhiao Huang, Yunzhu Li, **Russ Tedrake**, Marc Toussaint
- **Group role:** Co-author (Tedrake, 4th of 5). The last author is Marc Toussaint (TU Berlin), who is not a PI in the summary, so no other group owns it. It is included here because of Tedrake's authorship and MIT affiliation. Yunzhu Li (then MIT; now Columbia, group 8) is 3rd author.
- **Date published:** 2022-12 (CoRL 2022, Auckland, 14 to 18 December 2022; PMLR vol. 205 published 2023); arXiv v1 2022-02-24
- **Venue:** 6th Conference on Robot Learning (CoRL 2022), PMLR 205:1755–1768
- **Links:** arXiv https://arxiv.org/abs/2202.11855 · PMLR https://proceedings.mlr.press/v205/driess23a.html · Project https://dannydriess.github.io/compnerfdyn/
- **Number of citations:** 103 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper presents an object-centric visual world model. An implicit multi-view object encoder maps posed RGB images and object masks to one latent vector per object. Each latent conditions its own NeRF, and the scene is rendered compositionally. A graph neural network predicts latent dynamics, with the adjacency matrix estimated from the NeRF densities. Actions are rigid-transform interventions on the pusher's latent. The model gives stable long-horizon predictions, generalises to more objects than seen in training, and is used with a latent-space RRT planner for multi-object box sorting.

## 1. Weights availability
Not released. Checked: the project page (only PDF, arXiv and BibTeX links), the PMLR page and the paper. No weights found.

## 2. Code
No code found. The project page and paper link no repository.

## 3. Datasets
- **Simulation (PyBullet-type rigid pushing, App. E.1):**
  - 5,752 training scenes (4 boxes plus a cylinder pusher, average sequence length 17, 4 camera views).
  - Test sets of 312 scenes each with 2, 4 and 8 objects.
  - A deformable-object scenario is also included.
  - Author-generated; not released.
- **Real world (App. F):** Franka Panda pushing a shoe, a giraffe toy, a sand mold and wool. 2,000 random pushes (about 8 h of collection), 4 RealSense side views, colour-threshold masks. The training data needs only image sequences, not the robot actions. Not released.

## 4. Evaluation and downstream tasks
- **Metrics:** image prediction error over 50 rollout steps (1 step = 2 cm of pusher motion), relative image error, centre-of-mass prediction error (from NeRF density), and planner samples to solution.
- **Baselines:** Global NeRF (Li et al. 2021, "3D neural scene representations for visuomotor control"), global 2D CNN autoencoder (visual foresight), CNN decoder instead of NeRF, CNN encoder, GNN with dense adjacency, and an MLP dynamics model.
- **Results (Fig. 3, Fig. 14):**
  - The proposed model has the lowest reconstruction and long-horizon prediction error for 2, 4 (train) and 8 objects.
  - A dense adjacency matrix drifts and becomes useless after a few steps with 2 or 8 objects.
  - Values are given in plots only.
- **Planning, box sorting with latent-space RRT (Table 1, samples to solution for 1/2/4/6 objects):**
  - Full model: 256 / 2,341 / 23,819 / 85,022.
  - Dense-A GNN: no solution or failed execution in every case.
  - Naive control tree with the full model: 24,019 / NS / NS / NS (NS = no solution within 10^5 samples).
- **Real world:** forward prediction from side views beats the CNN-decoder and dense-A baselines, including on scenes with fewer objects than in training (Fig. 17–20). Novel-view renderings are shown.
- **Downstream tasks:** long-horizon prediction, model-based planning (LS-RRT with replanning), generalisation over object count, deformable-object prediction.

## 5. Architecture and techniques
- **Encoder Ω:** an implicit object encoder. 3D query points are projected into each view, image features are bilinearly sampled, masked per object and aggregated across views into a feature volume on a 10×40×40 grid. A volumetric encoder then produces one latent vector per object.
- **Decoder:** a compositional conditional NeRF, one NeRF per latent, composed by density-weighted colour. It is trained end-to-end with an L2 image reconstruction loss from random views.
- **Dynamics:** a GNN in latent space. The adjacency matrix is estimated from predicted NeRF densities (objects that overlap or touch interact) and re-estimated during message passing. A quasi-static assumption leaves non-interacting objects unchanged (Algo. 1 line 12).
- **Actions:** rigid transformations of the pusher applied directly to its implicit latent (T(q)[z]). The GNN predicts the other objects' response, so action labels are not needed for training.
- **Planner:** latent-space RRT using NeRF-derived centre-of-mass estimates for goal costs, with replanning during execution.
- **Paradigm:** deterministic autoencoder plus latent forward model (not generative). Parameter count not stated.

## 6. Conclusions and future work
- **Authors:**
  - Compositional, 3D-grounded latents improve reconstruction, long-term prediction stability and generalisation over object counts, and enable latent-space RRT planning.
  - Limitations: NeRF inference is computationally heavy; object masks are required; finite-size latents limit shapes to the training distribution.
  - Future work: faster NeRFs, slot-attention-style mask-free decomposition, and compositional object representations.
- **Own observations:**
  - Quantitative results are mostly plots. Tasks are tabletop pushing with simple objects, and no code or data was released.
  - The planning evaluation reports sample counts, not success rates over many scenes.

## 7. Sources and verification
- Paper PDF: https://arxiv.org/pdf/2202.11855 (v3; read main text and App. E, F)
- PMLR: https://proceedings.mlr.press/v205/driess23a.html ; project page: https://dannydriess.github.io/compnerfdyn/
- Citations: https://api.semanticscholar.org/graph/v1/paper/arXiv:2202.11855
- Not verified:
  - Oral or poster status at CoRL 2022.
  - The precise simulator name (the paper text I read does not name it in the extracted sections).
