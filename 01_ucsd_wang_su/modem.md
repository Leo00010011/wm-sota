---
citation_count: 74
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429)"
venue: "ICLR 2023"
date: 2022-12-12
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# MoDem: Accelerating Visual Model-Based Reinforcement Learning with Demonstrations

- **Authors:** Nicklas Hansen, Yixin Lin, **Hao Su**, **Xiaolong Wang**, Vikash Kumar, Aravind Rajeswaran
- **Group role:** first author (Hansen, during a Meta AI internship); both PIs are co-authors. The last author (Rajeswaran, Meta AI) is not a PI of any group in the summary, so the paper is kept in this folder.
- **Date published:** 2023-05 (ICLR 2023, Kigali, 1–5 May 2023). arXiv 2022-12-12.
- **Venue:** ICLR 2023, poster ("Accept: poster" in the ICLR 2023 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2212.05698 · OpenReview https://openreview.net/forum?id=JdTnc9gjVfJ · Project https://nicklashansen.github.io/modemrl · Code https://github.com/facebookresearch/modem
- **Number of citations:** 74 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
MoDem speeds up visual model-based RL (a TD-MPC latent world model with MPPI planning) by using a handful of demonstrations in three phases:
1. Behaviour-cloning pretraining of the encoder and policy.
2. "Seeding": world model and critic are trained on rollouts of the pretrained policy instead of random data.
3. Interactive finetuning that oversamples the demonstrations (annealed from 75% to 25%), with image-shift augmentation throughout.

With 5 demonstrations and 100K interactions, it solves sparse-reward visual manipulation tasks that the baselines cannot.

## 1. Weights availability
Not released. The repository provides code and demonstrations (GitHub release v.0.1.0) but no trained checkpoints. Checked: GitHub README, project page.

## 2. Code
- https://github.com/facebookresearch/modem, MIT license.
- Contains training code for Adroit, Meta-World and DMControl.
- Last push 2022-12-12; unmaintained since release.

## 3. Datasets
- **Simulators (public), 224×224 RGB plus proprioception:**
  - Adroit: 3 dexterous-hand tasks, sparse reward.
  - Meta-World: 15 medium/hard/very-hard tasks, sparse reward.
  - DMControl: 3 locomotion tasks, dense reward.
- **Demonstrations:** 5 per task. Adroit uses human teleoperation demos from Rajeswaran et al. 2018. Demos are released on the GitHub releases page.

## 4. Evaluation and downstream tasks
Metrics: success rate (Adroit, Meta-World), normalized return (DMControl), all at 100K interactions; 5 seeds.
- **Main results (Fig. 1, Fig. 4):**
  - Adroit: 53% average success at 100K steps; all baselines fail.
  - Meta-World: 81% average success, and all 15 tasks solved; FERM solves a subset.
  - DMControl: MoDem is also best.
- **Baselines:** BC+R3M, DAPG (state-based oracle), FERM, and TD-MPC with and without demos.
- **Relative gains:** the abstract states 160–250% higher relative success (38–50% absolute) than baselines. The conclusion states 250–350%, which is inconsistent with the abstract.
- **Ablations (Figs. 6–8):**
  - All three phases matter; naively adding demos to TD-MPC helps little.
  - Works even with 1 demo.
  - Augmentation is essential.
  - Oversampling ratio and number of seeding episodes matter less.
  - Human demos converge to the same performance as near-optimal policy demos.
  - Frozen R3M features are less sample-efficient than an end-to-end encoder.

## 5. Architecture and techniques
- TD-MPC world model: conv encoder; MLP latent dynamics, reward, Q and policy; latent consistency, reward and TD losses (Eq. 1); MPPI planning with a policy prior.
- Additions: BC loss (Eq. 2), seeding-phase training, demo oversampling coefficient κ (Eqs. 3–4), random-shift augmentation.
- Parameter count not stated in the main text.

## 6. Conclusions and future work
- **Authors:** naively appending demos does not accelerate MBRL. The three-phase recipe does, and data augmentation and end-to-end representations matter. No explicit future-work list beyond this.
- **Own observations:**
  - Simulation only (the real-robot follow-up is MoDem-V2).
  - High memory requirement (README: ≥80 GB RAM).
  - The relative-gain figures are inconsistent between abstract and conclusion.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2212.05698 v1 (main text, Table 1, Figs. 1–8); GitHub README and API.
- ICLR 2023 acceptance confirmed in the ICLR 2023 virtual-site data ("Accept: poster").
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: the per-baseline mapping of the bar values in Fig. 1 (extracted text is ambiguous); only MoDem's own values are reported.
