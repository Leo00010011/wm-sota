---
citation_count: 131
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429)"
venue: "CoRL 2023 (PMLR 229)"
date: 2023-06-26
research_group: "Stanford (Jiajun Wu)"
---

# RoboCook: Long-Horizon Elasto-Plastic Object Manipulation with Diverse Tools

- **Authors:** Haochen Shi*, Huazhe Xu*, Samuel Clarke, Yunzhu Li, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author. Yunzhu Li (PI of summary group 8, then a postdoc at Stanford and incoming at UIUC) is second-to-last.
- **Date published:** 2023-11 (CoRL 2023, Atlanta, November 2023). arXiv v1 2023-06-26, v2 2023-10-17.
- **Venue:** 7th Conference on Robot Learning (CoRL 2023), PMLR. The paper footer reads "7th Conference on Robot Learning (CoRL 2023), Atlanta, USA". Listed as a finalist for the CoRL 2023 Best Systems Paper Award (secondary source: a CoRL 2023 summary slide deck). The 08_columbia_yunzhu_li README says it won; I could not confirm that against the official corl2023.org awards page.
- **Links:**
  - arXiv: https://arxiv.org/abs/2306.14447
  - Project: https://hshi74.github.io/robocook/
  - Code: https://github.com/hshi74/robocook
  - Data: https://drive.google.com/drive/folders/1kEw4rnFWnYpkelfucvtJMEYYwA5P_0CK
- **Number of citations:** 131 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
RoboCook learns GNN particle-dynamics world models of how dough deforms under each of 15 tools. It uses only about 20 minutes of real random-interaction data per tool. Three parts combine these models into long-horizon tasks such as making dumplings and shaping letter cookies:
- A PointNet++ tool classifier picks the tool.
- A self-supervised policy network, trained only on data synthesized by the learned dynamics model, chooses tool actions.
- Closed-loop visual feedback corrects execution.

## 1. Weights availability
- No trained checkpoints are released, as far as I could check. The README links real-world data (raw sample, processed dynamics, tool-classification and planning datasets) and 3D tool meshes. No model weights are listed.
- The Google Drive folder was not opened, so checkpoints hidden there cannot be ruled out.

## 2. Code
- **Repo:** https://github.com/hshi74/robocook, MIT License ("Copyright (c) 2023 Haochen Shi").
- **Last commit:** 2024-04-15.
- **Contents:**
  - Perception
  - GNN dynamics training (`scripts/dynamics/run_train.sh`)
  - Tool classifier training
  - Closed-loop control
  - A simulator submodule
- Training and evaluation code are both included.

## 3. Datasets
- **Real-world robot data:** collected by random exploration within each tool's bounded action space, about 20 min per tool.
- **Capture:** point clouds from 4 RGB-D cameras, processed to 300 dough surface particles plus tool particles.
- **Synthetic data:** generated with the learned dynamics model and used to train the policy network.
- **Public:** processed datasets and one raw episode on Google Drive (link above).

## 4. Evaluation and downstream tasks
- **Dumpling making:** a 9-step, multi-tool, long-horizon task. Results are qualitative, with robustness shown under human perturbation (dough deformed, added or replaced). A human comparison and a quantitative tool-classifier evaluation are in the supplement.
- **Alphabet letter cookies (Table 1):** shaping R, O, B, C and K.

  | Method | Chamfer distance (CD) | Earth mover's distance (EMD) | Human recognition | Planning time |
  |---|---|---|---|---|
  | RoboCook | 0.0062 | 0.0042 | 0.90 | 9.3 s |
  | RoboCraft (GNN + L-BFGS) | 0.0066 | 0.0044 | 0.54 | 613.7 s |
  | CEM + GNN | 0.0066 | 0.0045 | 0.52 | 756.0 s |
  | CEM + MPM simulator | 0.0070 | 0.0046 | 0.48 | 1486.7 s |
  | RL + GNN | 0.0077 | 0.0064 | 0.17 | 1867.9 s |

  Human recognition is the accuracy of 100 subjects at identifying the letter. CD of surface normals is also reported in the table.
- **Material generalization:** shaping a "K" from Play-Doh, air-dry clay and model foam without retraining (qualitative, Fig. 6).
- **Downstream tasks covered:** model-based planning and policy learning from the world model, tool selection, and long-horizon manipulation.

## 5. Architecture and techniques
- **Perception:**
  1. Fuse point clouds from 4 cameras.
  2. Color-segment the dough.
  3. Reconstruct a watertight surface (Poisson + MeshFix, or alpha-shape).
  4. Sample inside the mesh using the signed distance function (SDF).
  5. Remove tool-penetrating points.
  6. Poisson-disk sample 300 surface points.
- **Tool particles:** sampled from ground-truth tool meshes placed at the recorded end-effector poses.
- **Dynamics:**
  - GNN over dough and tool particles. Node attributes are position, normal and group (dough or tool).
  - Separate MLP heads predict rigid and non-rigid motion, and their sum is the predicted motion.
  - Loss: 0.5·CD + 0.5·EMD, with multi-step rollout training (s = 2). At inference the model predicts 15 steps.
- **Tool selection:** PointNet++ on the (current, target) point-cloud pair outputs probabilities over 15 tools. The top-3 tools are planned and the best one is executed.
- **Policy:** a PointNet++ self-supervised policy. It formulates the action as multi-bin classification plus regression over parameterized action spaces, e.g. (r, θ, d) for gripping and (x, y, z, θ) for pressing and rolling. Hand-coded planners handle the other tools.

## 6. Conclusions and future work
- **Authors' conclusions:** contributions are tool-aware GNN dynamics, tool selection combined with dynamics, and fast self-supervised policies.
- **Limitations (authors):**
  - Dough occasionally sticks to tools; automatic error correction is suggested as a remedy.
  - Action spaces come from human priors.
  - Subgoals are human-defined, so higher-level task planning is needed.
  - Cables and cloth would need topology estimation.
- **Own observations:**
  - The improvement in CD/EMD over the baselines is small; the large gains are in human recognition and planning speed.
  - The world model is per-tool and per-material-family, with no shared model across tools.
  - Evaluation of the full dumpling task is mostly qualitative.

## 7. Sources and verification
- arXiv PDF v2 https://arxiv.org/pdf/2306.14447: Secs. 1–5, Table 1, Figs. 1–6.
- Repo README and LICENSE: https://github.com/hshi74/robocook
- Semantic Scholar API `paper/arXiv:2306.14447` gives the venue "Conference on Robot Learning".
- Wu's homepage lists "CoRL 2023".
- Not verified: the PMLR volume page URL and presentation type (not opened), and the contents of the Google Drive data folder.
