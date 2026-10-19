---
citation_count: null
citation_source: "null; Semantic Scholar API returned HTTP 429 on 2026-10-15, prior note value 48 unverified"
venue: "RSS 2026"
date: 2026-02-11
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# RISE: Self-Improving Robot Policy with Compositional World Model

- **Authors:** Jiazhi Yang*†, Kunyang Lin*, Jinwei Li*, Wencong Zhang*, Tianwei Lin, Longyan Wu, Zhizhong Su, Hao Zhao, Ya-Qin Zhang, Li Chen, Ping Luo, Xiangyu Yue♮, **Hongyang Li**♮
  - *Equal contribution. †Project lead. ♮Equal advising (Xiangyu Yue at CUHK, Hongyang Li at HKU).
- **Group role:** Hongyang Li is last author and co-advisor. Affiliations include CUHK, Kinetix AI, HKU, Shanghai Innovation Institute, Horizon Robotics and Tsinghua.
- **Date published:** 2026-07 (RSS 2026, Sydney, 13–17 July). arXiv v1 2026-02-11, v2 2026-04-28. The README dates acceptance to 2026-03-27.
- **Venue:** Robotics: Science and Systems (RSS) 2026, paper 12 (program page roboticsconference.org/program/papers/12; proceedings roboticsproceedings.org/rss22/p012.pdf).
- **Links:**
  - arXiv: https://arxiv.org/abs/2602.11075
  - Project: https://opendrivelab.com/RISE/
  - Code: https://github.com/OpenDriveLab/RISE
  - Weights: https://huggingface.co/OpenDriveLab-org/RISE_Assets
- **Number of citations:** 48 (source: Semantic Scholar API, checked 2026-10-15; Semantic Scholar still lists the venue as arXiv)

## Summary
RISE runs on-policy RL for a VLA (π0.5) entirely "in imagination". Its compositional world model has two parts:

- **Dynamics model:** an action-conditioned, multi-view video model, initialized from Genie Envisioner (GE-base, built on LTX-Video). It is further pre-trained on AgiBot World and Galaxea with an added action encoder and "task-centric batching".
- **Progress value model:** initialized from π0.5 and trained with a progress loss plus TD learning.

**Self-improvement loop:**
1. The rollout policy proposes action chunks.
2. The dynamics model imagines multi-view futures for those actions, with at most 2 chained steps per offline state.
3. The value model turns the imagined futures into chunk-wise advantages.
4. The policy is trained RECAP-style, conditioned on those advantages and mixed with offline data.

The world model is used only during training, so it adds no cost at deployment.

## 1. Weights availability
- **Released:** the pre-trained dynamics model on Hugging Face (OpenDriveLab-org/RISE_Assets, pre-trained on Galaxea Open World and AgiBot World Alpha), together with the training code (2026-03-22). The data and checkpoint are CC BY-NC-SA 4.0. It relies on LTX-Video components from Lightricks.
- **Not confirmed:** release of the task-specific fine-tuned dynamics, value or policy checkpoints.

## 2. Code
- https://github.com/OpenDriveLab/RISE, about 379 stars. The README says Apache-2.0, but the GitHub API reports no detected license file.
- **Modules:** OpenPI-based offline policy and value training, the action-conditioned dynamics model, online RL, and deployment on the AgileX Piper arm. A bug-fix update was pushed on 2026-06-03.

## 3. Datasets
- **Dynamics pre-training:** AgiBot World (Alpha) and the Galaxea Open World dataset, both public.
- **Task-specific data:** expert demonstrations, successful and failed policy rollouts, and DAgger corrections for each of three real tasks. Release was not confirmed.
- **Video benchmark:** Bridge, used for the dynamics-model comparison.

## 4. Evaluation and downstream tasks
**Real robot (Table I):** dual 7-DoF AgileX arms. Values are success rate (%) / score.

| Method | Dynamic Brick Sorting | Backpack Packing | Box Closing |
|---|---|---|---|
| π0.5 | 35 / 8.28 | 30 / 4.25 | 35 / 7.50 |
| π0.5 + DAgger | 15 / 6.10 | 50 / 7.00 | 40 / 7.50 |
| π0.5 + PPO | 10 / 7.68 | 35 / 5.88 | 10 / 4.75 |
| π0.5 + DSRL | 10 / 6.65 | 10 / 3.50 | 10 / 7.63 |
| RECAP (re-implemented on π0.5) | 50 / 9.00 | 40 / 6.13 | 60 / 8.13 |
| **RISE** | **85 / 9.78** | **85 / 9.50** | **95 / 9.88** |

- **Headline gains:** the abstract's "+35% / +45% / +35%" are absolute gains over RECAP.
  - For Backpack Packing, the strongest baseline is actually π0.5 + DAgger at 50%, so the gain over the best baseline is +35, not +45.
  - The main text instead says "a 40% margin in Backpack Packing", which is internally inconsistent with the abstract.

**Dynamics model quality (Table V):**

| Setting | Model | PSNR ↑ | LPIPS ↓ | SSIM ↑ | FVD ↓ | EPE ↓ |
|---|---|---|---|---|---|---|
| Real-world tasks | Cosmos | 21.17 | 0.14 | 0.79 | 97.90 | 1.21 |
| Real-world tasks | GE | 21.16 | 0.11 | 0.79 | 85.72 | 1.05 |
| Real-world tasks | RISE | 23.90 | 0.07 | 0.82 | 66.84 | 0.54 |
| Bridge | Cosmos | 21.32 | 0.14 | 0.80 | 73.21 | 1.18 |
| Bridge | GE | 21.47 | 0.12 | 0.79 | 64.55 | 0.96 |
| Bridge | RISE | 23.68 | 0.10 | 0.82 | 45.21 | 0.64 |

The variant without task-centric batching has a better FVD on real-world tasks (61.22) but a worse EPE.

**Ablations (on brick sorting):**
- **Offline data ratio:** 0.6 works best (50% complete success); 0.1 drops to 5%.
- **Online signals:** using both online actions and imagined online states is needed (70% vs 35–40%).
- **Removing components:** removing dynamics pre-training (15%), task-centric batching (40%), progress loss (50%) or TD learning (35%) all hurt, against 70% for the full model.

## 5. Architecture and techniques
- **Dynamics model:** GE-base (LTX-Video DiT backbone) with a lightweight action encoder. Stronger noise is applied to context frames for robustness. The authors report that GE generates 25 multi-view frames in under 2 s, against more than 10 min for Cosmos, roughly a 300× speedup.
- **Value model:** π0.5 backbone. The progress loss is used for the first 10K steps, then TD loss is added for 40K steps.
- **Policy:** π0.5 with flow-matching, advantage-conditioned and binned into N bins, with an EMA rollout policy.
- **Compute:**
  - Dynamics pre-training: 16 H100 GPUs, batch size 512, about 7 days.
  - Task fine-tuning: 8 H100 GPUs, batch size 64, about 3 days.
  - Value model: about 1 day on 8 GPUs.
  - Self-improvement: about 10K steps on 8 GPUs.

## 6. Conclusions and future work
- **Authors:** world models can serve as a practical learning environment for real-world VLA improvement. Limitations: a gap between imagination and reality (implausible transitions in rare states), real data is still required in a sensitive ratio, and the bottleneck shifts from physical cost to compute cost.
- **Own observation:**
  - There are only three tasks, all on one robot platform.
  - The baselines were re-implemented by the authors under a "close compute budget".
  - Trial counts per task are small: success rates move in 5% steps, consistent with about 20 trials.
  - This is a robotics paper, not a driving one, but it follows the same group's Vista and ReSim line of world model as simulator plus reward.

## 7. Sources and verification
- **Read:** full arXiv v2 PDF (main text, including layout-mode tables), GitHub README and dynamics-model docs, GitHub API, Semantic Scholar API, and a web search for the RSS program entry.
- **Not opened:** the RSS proceedings PDF itself.
