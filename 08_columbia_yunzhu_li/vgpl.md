---
citation_count: 91
citation_source: "Semantic Scholar, 2026-10-13 (carried over from note; live re-check rate-limited 2026-10-15)"
venue: "ICML 2020"
date: 2020-04-28
research_group: "Columbia (Yunzhu Li)"
---

# Visual Grounding of Learned Physical Models (VGPL)

- **Authors:** **Yunzhu Li**, Toru Lin\*, Kexin Yi\*, Daniel M. Bear, Daniel L. K. Yamins, Jiajun Wu, Joshua B. Tenenbaum, Antonio Torralba (\* equal contribution, alphabetical)
- **Group role:** First and corresponding author. The paper says "Correspondence to: Yunzhu Li". The last author is Torralba, who is not a PI in the summary. Jiajun Wu (07) is a middle author. Because Yunzhu Li is the corresponding author, this paper belongs here under the ownership rule.
- **Date published:** 2020-07 (ICML 2020, held virtually 12 to 18 July 2020). arXiv v1 2020-04-28.
- **Venue:** International Conference on Machine Learning (ICML) 2020, PMLR volume 119 (stated on the paper's first page). Oral or poster status not found.
- **Links:** arXiv https://arxiv.org/abs/2004.13664 · Project http://visual-physics-grounding.csail.mit.edu/ · Code https://github.com/YunzhuLi/VGPL · Visual prior code https://github.com/ToruOwO/VGPL-Visual-Prior · Dynamics prior https://github.com/YunzhuLi/VGPL-Dynamics-Prior
- **Number of citations:** 91 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
VGPL grounds a learned particle-based dynamics model (a DPI-Net "dynamics prior") in images. The pipeline has three parts:
- A visual prior predicts particle positions and object groupings from image sequences.
- An inference module refines the particle positions and estimates object rigidity and global physical parameters (gravity, viscosity, stiffness). It is trained only by back-propagating the future-prediction error through the frozen dynamics prior.
- The grounded model then predicts future particle states.

## 1. Weights availability
- The VGPL README provides evaluation scripts for a "trained model" for the dynamics prior, parameter estimation, position refinement and forward prediction on MassRope and RigidFall. Test data, including cached visual-prior outputs, is on Dropbox:
  - MassRope: https://www.dropbox.com/s/l3fx5onv21ti72p/data_MassRope_valid.zip (2.8 GB).
  - RigidFall: https://www.dropbox.com/s/o3ehs4s4p13kuy6/data_RigidFall_valid.zip (7.35 GB).
- The checkpoint files appear to be in the repository. I did not verify this by listing the files.
- No license file was found (I checked `LICENSE` and `LICENSE.md` on master). FluidCube is not covered by the released scripts.

## 2. Code
- https://github.com/YunzhuLi/VGPL: evaluation code for every stage.
- https://github.com/YunzhuLi/VGPL-Dynamics-Prior: stand-alone dynamics prior with training scripts and Dropbox training data:
  - MassRope: 1.14 GB.
  - RigidFall: 2.9 GB.
- https://github.com/ToruOwO/VGPL-Visual-Prior: the perception module (not inspected).
- No license file in either YunzhuLi repository. Last-update dates not verified (GitHub API rate-limited).

## 3. Datasets
All data was generated with NVIDIA FleX, with RGB renders and particle states and a 90/10 split (Sec. 4.1):
- **RigidFall:** 5,000 sims × 120 steps. Three cubes of 64 particles each, gravity sampled in [−15, −5].
- **FluidCube:** 2,000 × 300 steps. A rigid cube of 48 particles on 300 fluid particles, viscosity in [1, 100].
- **MassRope:** 3,000 × 200 steps. A rigid mass of 81 particles plus a rope of 14 particles, stiffness in [0.25, 1.2].

## 4. Evaluation and downstream tasks
- **Physical parameter estimation:** MAE as a percentage of the parameter range (Table 1).

  | Model | MassRope | RigidFall | FluidCube |
  |---|---|---|---|
  | VGPL | 2.9% | 3.7% | 17.5% |
  | Without rigidness | 3.4% | 7.4% | 22.2% |
  | DensePhysNet | 24.5% | 25.7% | 28.6% |

- **Position refinement (Fig. 2c):** MSE drops after refinement in all environments; MassRope improves more than threefold. The README reports MSE ×10⁴ before and after refinement:
  - MassRope: 1.958 → 0.478.
  - RigidFall: 1.970 → 1.450.
- **Rigidity estimation (Fig. 2a,b):** the mean probability of the correct label is close to 1 for the rope, the mass and the fluid when 10 frames are observed. The floating cube is harder, and its accuracy improves with longer inputs.
- **Future prediction:** particle MSE ×10⁴ at T+1, T+5, T+10 and T+20 (Table 2). The full model is best at long horizons. Ablations (without rigidness, without refinement, without parameter estimation) degrade it, and the rigid bodies "melt" without rigidity estimation. I did not transcribe exact per-cell values because the table columns were scrambled in the PDF text extraction.
- **Downstream tasks:** visual system identification and future prediction only. No control or planning.

## 5. Architecture and techniques
- **Visual prior:** a 4-block CNN (32 to 256 channels) with two MLP heads (hidden size 2048) that output particle positions and grouping probabilities. It is in the style of a point-set generation network and is supervised with ground-truth particles.
- **Dynamics prior:** DPI-Net with edges within d_e = 0.08, 3-layer MLP encoders of width 150 and a rigid-transform head for rigid objects. It uses 4 history steps and is trained on ground truth.
- **Inference module:**
  - Spatial GNN message passing followed by a bidirectional GRU over time.
  - Heads for position refinement, object rigidity (sigmoid) and global parameters (tanh).
  - Trained by an L1 future-prediction loss through the frozen dynamics prior.
- **Training details:** PyTorch with Adam. Learning rates are 1e-4 (visual) and 1e-5 (dynamics and inference). Parameter count not stated.

## 6. Conclusions and future work
- **Authors:** VGPL grounds physical properties from vision through learned visual and dynamics priors. Its particle representation handles rigid, deformable and fluid objects, and it adapts quickly to unknown properties while predicting accurately into the future. No explicit future-work list is given.
- **My own observations:**
  - Everything is in FleX simulation, and the visual prior is supervised with simulator particles, so transfer to real images is untested.
  - The number of particles is fixed per scene.
  - FluidCube parameter error is still high (17.5%).
  - There is no control experiment, so "world model" here means prediction and system identification only.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2004.13664 (v2, full text read).
- READMEs of YunzhuLi/VGPL and YunzhuLi/VGPL-Dynamics-Prior (raw.githubusercontent.com).
- Project page: http://visual-physics-grounding.csail.mit.edu/
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - Presentation type.
  - Exact Table 2 values (text extraction garbled).
  - Repository last-update dates.
  - Whether the checkpoints are committed in the repo.
