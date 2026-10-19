---
citation_count: 35
citation_source: "Semantic Scholar, 2026-10-15"
venue: "IEEE ICRA 2024"
date: 2023-09-25
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# MoDem-V2: Visuo-Motor World Models for Real-World Robot Manipulation

- **Authors:** Patrick Lancaster, Nicklas Hansen, Aravind Rajeswaran, Vikash Kumar
- **Group role:** second author (Nicklas Hansen, UCSD PhD student in the Wang/Su group). Neither PI (Wang, Su) is an author. The senior authors are Meta AI researchers who are not PIs of any group in the summary. The paper is kept here because it is a direct extension of the group's TD-MPC/MoDem line and its only academic author is from this group.
- **Date published:** 2024-05 (ICRA 2024, Yokohama, 13–17 May 2024; IEEE DOI 10.1109/ICRA57147.2024.10611121). arXiv v1 2023-09-25; v2 2024-05-12.
- **Venue:** IEEE ICRA 2024
- **Links:** arXiv https://arxiv.org/abs/2309.14236 · DOI https://doi.org/10.1109/ICRA57147.2024.10611121 · Project https://sites.google.com/view/modem-v2 · Code https://github.com/facebookresearch/modemv2
- **Number of citations:** 35 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
MoDem-V2 adapts MoDem (a TD-MPC decoder-free latent world model plus demonstrations) so it can learn contact-rich manipulation directly on a real Franka robot from pixels, proprioception and sparse rewards, without unsafe exploration. Three changes make exploration conservative and uncertainty-aware:
- sampled actions are centred around the behaviour-cloning (BC) policy;
- control is gradually handed over from BC actions to MPC planning;
- planning uses actor-critic ensembles that penalize epistemic uncertainty.

## 1. Weights availability
Not released. The README provides demonstrations (GitHub release on palanc/modem, tag 0.1.0) and training scripts but no trained checkpoints. Checked: GitHub README and project page.

## 2. Code
- https://github.com/facebookresearch/modemv2. The GitHub API reports MIT, but the README says the majority of the code is CC-BY-NC with third-party parts under their own licenses.
- Contains training scripts for the Franka tasks (robohive simulation).
- The repository was archived after the last push on 2024-04-01.

## 3. Datasets
- 10 demonstrations per task (released) for 4 tasks: planar pushing, inclined pushing, bin picking, and in-hand reorientation (D'Manus 10-DoF hand).
- Each task exists in simulation (RoboHive/MuJoCo) and on a real Franka Panda with 3 RealSense D435 cameras.
- Real-robot online data is collected during training and is not released as a dataset.

## 4. Evaluation and downstream tasks
Metrics: success rate over 30 evaluation trials and number of safety violations (torque-limit or >100 N contact-force violations in simulation; hardware faults on the real robot).
- **Simulation (Fig. 4):**
  - MoDem and MoDem-V2 both outperform DAPG (which has privileged state and dense rewards).
  - FERM matches on the pushing tasks but fails on bin picking and reorientation.
  - MoDem-V2 commits far fewer safety violations than MoDem, which degrades early in training through aggressive exploration.
- **Fig. 2:** a torque penalty alone does not prevent MoDem's safety terminations.
- **Ablations (Fig. 5):** ensembles improve sample efficiency and the agency-transfer schedule improves safety; MoDem-V2 combines both.
- **Real world (Fig. 6):** MoDem-V2 clearly beats its initial BC policy on all 4 tasks with ≤2 hours of interaction (planar and inclined pushing 8 min, reorientation 70 min, bin picking 140 min). Original MoDem triggered safety faults within the first two exploration episodes on inclined pushing.

## 5. Architecture and techniques
- MoDem/TD-MPC world model: encoder over stacked multi-view RGB plus proprioception; MLP latent dynamics, reward and terminal Q; policy; joint-embedding prediction plus reward and TD losses.
- Planning (Algorithm 1): with probability 1−α the first actions come from the BC policy and are scored by a BC critic; otherwise MPPI runs with samples from the policy ensemble. α is annealed linearly from 0 to 1 ("agency transfer").
- Trajectory value = w1·mean + w2·std over the ensemble of terminal values, with w2 < 0 (uncertainty penalty).
- Parameter count not stated.

## 6. Conclusions and future work
- **Authors:**
  - MoDem-V2 keeps MoDem's sample efficiency while being much safer, and lets a physical robot learn pushing, picking and in-hand manipulation from about an hour or less of interaction data.
  - Limitations: requires demonstrations; assumes resets to a narrow set of start states.
  - Future work: reuse the learned world model across new objects and goals.
- **Own observations:**
  - Real-world evaluation reports only "best episode success" per task, not learning curves with variance.
  - The world model is task-specific.
  - Licensing is unclear (CC-BY-NC per README vs MIT per GitHub metadata).

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2309.14236 v2 (full text, Figs. 2, 4–6, Algorithm 1); GitHub README and API.
- ICRA 2024 publication confirmed through the IEEE DOI returned by Semantic Scholar (venue "IEEE International Conference on Robotics and Automation").
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Not found: exact real-world success values (they are shown only as bars in Fig. 6).
