---
citation_count: 497
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; re-check returned HTTP 429)"
venue: "ICLR 2019"
date: 2018-10-03
research_group: "Columbia (Yunzhu Li)"
---

# Learning Particle Dynamics for Manipulating Rigid Bodies, Deformable Objects, and Fluids (DPI-Net)

- **Authors:** **Yunzhu Li**, Jiajun Wu, Russ Tedrake, Joshua B. Tenenbaum, Antonio Torralba
- **Group role:** First author. Yunzhu Li was then an MIT PhD student and is now the Columbia PI. The last author is Torralba, who is not a PI in the summary, so the orchestrator assigned the paper to this folder because it starts the Columbia line of work. Jiajun Wu (07) and Russ Tedrake (05) are middle authors.
- **Date published:** 2019-05 (ICLR 2019, New Orleans, 6 to 9 May 2019). arXiv v1 2018-10-03.
- **Venue:** International Conference on Learning Representations (ICLR) 2019, conference paper. The PDF header reads "Published as a conference paper at ICLR 2019". Oral or poster status was not verified; the project lists a poster.
- **Links:** arXiv https://arxiv.org/abs/1810.01566 · Project http://dpi.csail.mit.edu · Code https://github.com/YunzhuLi/DPI-Net
- **Number of citations:** 497 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
DPI-Net (Dynamic Particle Interaction Networks) is a learned, differentiable, particle-based simulator. It represents rigid bodies, elasto-plastic objects and fluids as particles on a dynamically rebuilt interaction graph and predicts the next particle states. The model is used for gradient-based trajectory optimisation with MPC and for online system identification of unknown physical parameters. This includes a real robot molding deformable foam.

## 1. Weights availability
- The repository README says the evaluation scripts (`scripts/eval_{FluidFall,BoxBath,FluidShake,RiceGrip}.sh`) "use the pretrained checkpoint", so checkpoints appear to be shipped inside the repository.
- No license file was found in the repo (I checked `LICENSE`, `LICENSE.md` and `LICENSE.txt` on master).
- No Hugging Face or Zenodo release.

## 2. Code
- https://github.com/YunzhuLi/DPI-Net (PyTorch). No license file found.
- Contents:
  - Training scripts that generate NVIDIA FleX data on first run, through a PyFleX submodule.
  - Evaluation scripts.
- A 2020-09-08 README update points to https://github.com/YunzhuLi/VGPL-Dynamics-Prior as an improved version with more stable long-horizon rollouts.
- Last-commit date not verified (the GitHub API was rate-limited).

## 3. Datasets
All data is synthetic and generated with NVIDIA FleX (App. C). Each environment uses a 90/10 train/validation split.
- **FluidFall:** 3,000 rollouts × 120 steps, with 189 particles.
- **BoxBath:** 3,000 rollouts × 150 steps, with 960 fluid particles plus a rigid cube of 27 to 150 particles.
- **FluidShake:** 2,000 rollouts × 300 steps.
- **RiceGrip:** 5,000 rollouts × 30 steps, with randomised stiffness and plasticity.
- **Real world:** a Kuka robot with a depth camera (TSDF fusion) molding deformable foam. This is a small set of trials, not a dataset.

## 4. Evaluation and downstream tasks
- **Forward simulation:** rollout MSE (×10⁻²), from Table 1:

  | Model | FluidFall | BoxBath | FluidShake | RiceGrip |
  |---|---|---|---|---|
  | DPI-Net | 0.15 | 2.03 | 1.89 | 0.13 |
  | HRN | 0.21 | 3.62 | 3.58 | 0.17 |
  | Interaction Networks | 2.74 | out of memory | out of memory | out of memory |

  - IN runs out of memory beyond FluidFall.
  - The ablation without hierarchy hurts BoxBath (2.64) and RiceGrip (0.29).
  - The model extrapolates to scenes with twice as many particles (App. B, Fig. 6).
- **Control:**
  - **Metric:** Chamfer distance to the target shape.
  - **Methods compared:** RS+TO (random search plus gradient trajectory optimisation, with MPC) beats random search over the model and PPO (Fig. 5). Adding online identification (RS+TO+ID) is best on RiceGrip with unknown physics.
  - **Sample efficiency:** RL needs over 10M steps on FluidShake vs 600K for the model-based approach, and 10M vs 150K on RiceGrip (Sec. 4.3). The exact bar values in Fig. 5 could not be reliably read from the extracted PDF text.
- **Downstream tasks:**
  - Fluid shaping (FluidShake).
  - Deformable-object gripping and molding in simulation (RiceGrip).
  - Real-world foam molding with online adaptation. The PPO policy did not transfer to the real world.

## 5. Architecture and techniques
- **Model:** a GNN in the style of Propagation Networks. It has:
  - Multi-step message passing within one time step.
  - Dynamic interaction graphs, with neighbours within radius d = 0.08.
  - A hierarchical leaf-to-root, root-to-root and root-to-leaf propagation for long-range effects.
- **Material-specific prediction:**
  - Rigid bodies predict a rigid transform by averaging over particles.
  - Elasto-plastic particles keep a resting position.
  - Fluids use local neighbourhoods only.
- **Layers (App. D):** encoders are MLPs (2×200 for objects, 3×300 for relations) and propagators are 1×200 with residual connections. Adam with learning rate 1e-4, batch size 1.
- **Objective:** supervised next-state regression.
- **Control:** sample 20 action sequences, pick the best under the model, refine with RMSprop through the differentiable model, and run MPC. Unknown attributes are estimated online by gradient descent on prediction error.
- **Size and speed:** parameter count not stated. Speed is about 30 ms per step on a TITAN Xp for RiceGrip.

## 6. Conclusions and future work
- **Authors:** a learned particle dynamics model can approximate interactions across materials and solve complex deformable-object manipulation. Particle-based representations are generalisable and expressive. The work "helps lay the foundation" for robot learning of dynamic scenes with particles. No explicit future-work list is given.
- **My own observations:**
  - The model needs full particle states. In the real world these come from a static mesh reconstruction, not from tracked particles, so perception is the bottleneck.
  - All training data is simulated, and real-world evidence is qualitative and small-scale.
  - Long-horizon stability was later improved in the VGPL-Dynamics-Prior repo, which suggests rollout drift in the original model.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/1810.01566 (v2, full text and appendix read).
- Code: https://github.com/YunzhuLi/DPI-Net (README read via raw.githubusercontent.com).
- Citations: https://api.semanticscholar.org/graph/v1/paper/batch (ArXiv:1810.01566).
- Venue: ICLR header on the PDF, the PI's homepage https://yunzhuli.github.io and the Semantic Scholar venue field.
- **Not verified:**
  - Oral or poster status.
  - Repository last-update date and checkpoint file presence (GitHub API rate-limited).
  - Exact control numbers (bar-chart values garbled in text extraction).
