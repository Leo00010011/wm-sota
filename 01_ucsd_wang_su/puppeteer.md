---
citation_count: 46
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICLR 2025"
date: 2024-05-28
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Hierarchical World Models as Visual Whole-Body Humanoid Controllers

- **Authors:** Nicklas Hansen, Jyothir S V, Vlad Sobal, Yann LeCun, **Xiaolong Wang**, **Hao Su** (Wang and Su marked "equal advising")
- **Group role:** first author (Hansen) and senior authors (Wang, Su). NYU co-authors include Yann LeCun (PI of group 3 in the summary), but the senior/last authors are the UCSD PIs, so the paper belongs here.
- **Date published:** 2025-04 (ICLR 2025, Singapore, 24–28 April 2025). arXiv v1 2024-05-28; v3 2025-05-14.
- **Venue:** ICLR 2025, poster ("Accept (Poster)" in the ICLR 2025 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2405.18418 · OpenReview https://openreview.net/forum?id=7wuJMvK639 · ICLR https://iclr.cc/virtual/2025/poster/30793 · Project https://www.nicklashansen.com/rlpuppeteer · Code https://github.com/nicklashansen/puppeteer
- **Number of citations:** 46 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Puppeteer is a two-level hierarchy of TD-MPC2 world models for visual whole-body control of a 56-DoF simulated humanoid:
- **Low level:** a proprioceptive tracking world model, pretrained once on retargeted CMU MoCap, turns end-effector reference commands into joint actions.
- **High level:** a "puppeteer" world model plans those commands from proprioception plus RGB, using task rewards.

The authors describe it as a hierarchical JEPA-style world model. Its task returns are close to TD-MPC2, and human raters strongly prefer its motion.

## 1. Weights availability
- Released: low-level tracking and high-level task checkpoints in a Google Drive folder, https://drive.google.com/drive/folders/1cgt9HzquO5mcB71Krv0C0mD10scfMquO (linked from the README).
- Load example: `python evaluate.py task=corridor low_level_fp=/path/to/tracking.pt checkpoint=/path/to/corridor-1.pt`.
- No separate license for the weights; the repo README says MIT.

## 2. Code
- https://github.com/nicklashansen/puppeteer. The README says MIT; the GitHub API reports "NOASSERTION".
- Contains training and evaluation for both levels, plus the new task suite.
- Last push 2025-09-18.

## 3. Datasets
- **Pretraining:** the MoCapAct offline dataset (noisy rollouts of clip-tracking policies; 836 CMU MoCap clips retargeted to the CMU Humanoid; public, MIT license), mixed 50/50 with online interaction.
- **Downstream:** a new task suite of 8 tasks on DMControl/MuJoCo (public).
  - 5 visual tasks: corridor, hurdles, walls, gaps, stairs.
  - 3 proprioceptive tasks: stand, walk, run.
  - Inputs: a 212-d proprioceptive state plus 64×64 third-person RGB.

## 4. Evaluation and downstream tasks
Metrics: episode return, a human-preference user study, and "naturalness" proxies (episode length, torso height).
- **Return (Fig. 5, 3M steps, 10 seeds):** comparable to TD-MPC2 on all tasks except stairs (where TD-MPC2 "rolls" up the stairs). SAC and DreamerV3 achieve no meaningful performance; hierarchical DreamerV3 on top of the TD-MPC2 low level is poor.
- **User study, n=51 (Fig. 6):** about 97.8% of responses prefer Puppeteer's motions. The text says 4% rate them "equally natural" and 0% prefer TD-MPC2.
- **Naturalness on gaps (Table 1):**

  | Metric | Puppeteer | TD-MPC2 |
  |---|---|---|
  | Episode length @1M steps | 115.9±5.2 | 66.9±9.8 |
  | Episode length at convergence | 159.3±5.9 | 181.6±28.1 |
  | Torso height (cm) | 96.0±0.2 | 85.9±4.7 |

- **Ablations (Fig. 8):** offline+online pretraining data and more MoCap clips improve tracking; planning at both levels is critical; pretraining the high level helps.
- **Zero-shot generalization (Fig. 9):** non-trivial performance on gaps up to 3× longer than seen in training.
- **Compute:** about 12 days for tracking and about 4 days per downstream task on one RTX 3090.

## 5. Architecture and techniques
- Both levels are 5M-parameter TD-MPC2 world models with identical components: encoder, latent dynamics, reward, Q-ensemble, policy prior, plus a new termination head D (binary cross-entropy).
- Planning handles predicted termination with soft truncation, i.e. cumulative discounting of termination probabilities in rollouts.
- Commands are relative end-effector (head, hands, feet) positions; both levels run at the same frequency (k=1).
- Training: decoder-free joint-embedding prediction, reward and TD losses (self-predictive, JEPA-style); MPPI planning at both levels. The high level is trained with the low level frozen.

## 6. Conclusions and future work
- **Authors:**
  - The method produces natural, human-like visual whole-body control in a fully data-driven way.
  - Limitations they state: tasks mainly test visuo-locomotion, and only one level is pretrained in most experiments. How to pretrain and transfer the full hierarchy is open.
- **Own observations:**
  - Simulation only (DMControl CMU Humanoid); no real humanoid.
  - "Naturalness" depends on the MoCap prior and is measured mostly by preference.
  - Long training (12 days for tracking).
  - The visual input is low resolution (64×64).

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2405.18418 v3 (main text, Table 1, Figs. 5–9); GitHub README and API.
- ICLR 2025 acceptance confirmed in the ICLR 2025 virtual-site data.
- Citation count: Semantic Scholar batch API, 2026-10-13.
- The exact split in the human-preference figure (97.8% / 1.8% / 0.4%) is read from the extracted figure text and does not fully match the prose (4% "equal"). Reported as given.
