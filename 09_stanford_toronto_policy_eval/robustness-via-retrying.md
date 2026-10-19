---
citation_count: 73
citation_source: "Semantic Scholar, 2026-10-15"
venue: "CoRL 2018"
date: 2018-10-06
research_group: "Stanford / Toronto (policy evaluation)"
---

# Robustness via Retrying: Closed-Loop Robotic Manipulation with Self-Supervised Learning

- **Authors:** Frederik Ebert, Sudeep Dasari, Alex X. Lee, Sergey Levine, **Chelsea Finn** (UC Berkeley)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2018-10 (CoRL 2018, Zürich, venue date); arXiv 2018-10-06
- **Venue:** CoRL 2018
- **Links:** arXiv https://arxiv.org/abs/1810.03043 · project page https://sites.google.com/view/robustness-via-retrying · code https://github.com/febert/robustness_via_retrying
- **Number of citations:** 73 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
The paper extends visual MPC (planning with an action-conditioned video prediction model) to longer tasks by adding a self-supervised image-registration network that keeps track of the designated object pixels relative to the start and goal images at every step. The robot can therefore re-plan and keep retrying after mispredictions. It also adds multi-camera planning and a grasp reflex so prehensile and non-prehensile strategies emerge from 160 robot hours of autonomous data.

## 1. Weights availability
Not released. The repository README says "Instructions to install and reproduce experiments coming soon!"; no checkpoints found.

## 2. Code
https://github.com/febert/robustness_via_retrying, 16 stars, last push 2018-11-07, no license reported (GitHub API, checked 2026-10-15). Copied code base (data collection, experiments, MuJoCo models, `python_visual_mpc`, TensorFlow data); no install or reproduction instructions.

## 3. Datasets
- 20,000 pushing trajectories and 15,000 trajectories with gripper control on Sawyer robots, 150 training objects (autonomously collected). Linked from the project page as part of the Berkeley robotic interaction datasets: https://sites.google.com/berkeley.edu/robotic-interaction-datasets
- Evaluation: 20 long-distance and 15 short-distance pushing tasks, 20 relocation tasks with grasping, novel objects.

## 4. Evaluation and downstream tasks
- Pushing success (Table 1, object within 15 px of goal on 48x64 images): long-distance 66% (registration) vs 45% (OpenCV MIL tracker) vs 20% (predictor propagation); short-distance 83% for all.
- Combined grasping and pushing (Fig. 9): comparable to the supervised OpenCV tracker without human supervision.
- Appendix: additional simulated comparisons (not read in detail).
- Downstream task: closed-loop visual MPC object relocation on a real robot.

## 5. Architecture and techniques
- World model: transformation-based action-conditioned video prediction (SNA architecture of Ebert et al. 2017), predicting future frames and designated-pixel distributions.
- Registration network: fully convolutional, trained self-supervised with photometric and smoothness losses on the same data (curriculum on temporal distance up to 8 steps, 60k iterations).
- Cost: expected pixel distance to goal, weighted by registration confidence from photometric error (Eq. 5); multiple camera views combined.
- Planner: CEM with MPC re-planning at every step.

## 6. Conclusions and future work
- Authors: learned registration closes the control loop and substantially improves long tasks; grasp reflex enables prehensile skills. Future: multi-object relocation with a higher-level planner.
- Own observations: the paper's contribution is the planning cost, not the world model itself; the video model is from prior work. Code release is incomplete.

## 7. Sources and verification
- Paper: arXiv 1810.03043v1 PDF (comment "accepted at CoRL 2018"), main text read.
- Code: GitHub API and README; project page via WebFetch (2026-10-15).
- Citations: Semantic Scholar API, 2026-10-15.
