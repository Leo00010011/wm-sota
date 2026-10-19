# UC San Diego — Xiaolong Wang & Hao Su (Nicklas Hansen)

- **PIs:** Xiaolong Wang (UCSD ECE; also NVIDIA) and Hao Su (UCSD CSE; also CTO of Hillbot). Key student: Nicklas Hansen (PhD student, co-advised by both; author of the TD-MPC line).
- **Lab / project pages:** https://xiaolonw.github.io · https://cseweb.ucsd.edu/~haosu · https://www.nicklashansen.com
- **Research line:**
  - Decoder-free ("implicit", self-predictive / JEPA-style) latent world models with model-predictive control: TD-MPC → TD-MPC2 → Newt.
  - Extensions to demonstrations, real robots, humanoids, imitation, and transfer.
  - More recently, generative video world models (MMBench2 / hallucination) and a diffusion dynamics model for cloth.

## Included papers (top venue, acceptance confirmed)

Citation counts are from the Semantic Scholar API, checked 2026-10-13.

| Paper | Venue | Year | Citations | Group role |
|---|---|---|---|---|
| [Temporal Difference Learning for Model Predictive Control (TD-MPC)](td-mpc.md) | ICML | 2022 | 537 | Hansen 1st; Wang, Su senior |
| [MoDem: Accelerating Visual Model-Based RL with Demonstrations](modem.md) | ICLR | 2023 | 74 | Hansen 1st; Su, Wang co-authors (last: Rajeswaran, Meta) |
| [On the Feasibility of Cross-Task Transfer with Model-Based RL (XTRA)](xtra.md) | ICLR | 2023 | 19 | Hansen co-1st; Su co-author (last: Z. Tu, UCSD) |
| [Finetuning Offline World Models in the Real World (FOWM)](fowm.md) | CoRL (oral per authors) | 2023 | 50 | Wang senior; Hansen co-1st |
| [TD-MPC2: Scalable, Robust World Models for Continuous Control](td-mpc2.md) | ICLR (spotlight) | 2024 | 706 | Hansen 1st; Su, Wang senior |
| [MoDem-V2: Visuo-Motor World Models for Real-World Robot Manipulation](modem-v2.md) | ICRA | 2024 | 35 | Hansen 2nd; no PI (last: Kumar, Meta) |
| [Hierarchical World Models as Visual Whole-Body Humanoid Controllers (Puppeteer)](puppeteer.md) | ICLR | 2025 | 46 | Hansen 1st; Wang, Su senior |
| [PWM: Policy Learning with Multi-Task World Models](pwm.md) | ICLR | 2025 | 24 | Hansen 3rd; no PI (last: Garg, Georgia Tech) |
| [Multi-Stage Manipulation with Demonstration-Augmented Reward, Policy, and World Model Learning (DEMO³)](demo3.md) | ICML | 2025 | 15 | Su senior; Hansen 2nd |
| [Reward-free World Models for Online Imitation Learning (IQ-MPC)](iq-mpc.md) | ICML | 2025 | 7 | Su senior |
| [Diffusion Dynamics Models with Generative State Estimation for Cloth Manipulation (UniClothDiff)](uniclothdiff.md) | CoRL | 2025 | 31 | Su senior |
| [WorldModelBench: Judging Video Generation Models As World Models](worldmodelbench.md) | NeurIPS (Datasets & Benchmarks) | 2025 | 118 | Wang co-author (last: Yao Lu, NVIDIA) |
| [Learning Massively Multitask World Models for Continuous Control (Newt)](newt.md) | ICLR | 2026 | 14 | Hansen 1st; Su, Wang senior |
| [RoboTALES: Reasoning-Guided Robot Policies via Task-Aligned Simulated Futures](robotales.md) | ECCV | 2026 | 0 | Hansen co-author; no PI (last: Chandraker, UCSD) |
| [Hallucination in World Models is Predictable and Preventable (MMBench2)](mmbench2-hallucination.md) | NeurIPS (accepted; conference Dec 2026) | 2026 | 10 | Hansen 1st; Wang senior |

**Inclusion notes:**
- Four papers (MoDem-V2, PWM, RoboTALES, WorldModelBench) have a senior author outside the group. In none of them is the senior author a PI of another group in the summary, so each is kept here and its group role is stated explicitly.
- Three of these (MoDem-V2, PWM, RoboTALES) have no Wang/Su authorship at all; they are included only through Nicklas Hansen. Drop them if the folder should hold PI-authored papers only.
- RoboTALES is borderline in scope: its video "world model" is plan-conditioned, not action-conditioned.

## Cross-group papers

- None found where Wang, Su or Hansen co-authored a top-venue world-model paper whose senior or corresponding author is a PI of another summary group.
- Puppeteer (ICLR 2025) has Yann LeCun (group 3, NYU) as a co-author, but the last authors are Wang and Su, so it is owned here. The NYU folder should not duplicate it.

## Excluded items

| Item | Reason |
|---|---|
| FACT: Failure-Aware Causal Training for World-Action Models (Peng, Liang, Yan, Hansen, Wang; arXiv 2608.10232) | Claimed as CoRL 2026 by the author homepage and repo description, but no official CoRL 2026 accepted-papers listing or OpenReview decision could be retrieved (the conference is in Nov 2026). Acceptance not confirmed. **Re-check after CoRL publishes its list.** |
| Back to Parsimonious Latents: Learning Task-Centric World Models from Visual Foundations (Fu, Feng, Hansen, Huang; arXiv 2605.25620) | Listed as "NeurIPS 2026" on Hansen's homepage, but **not found** in the NeurIPS 2026 virtual-site paper list (searched by title and by author Biwei Huang). Acceptance not confirmed. Senior author Biwei Huang is not a summary PI. |
| Generalizable Robotic Insertion with World Models (Hansen, …, Su, Wang, …, Narang; arXiv 2609.28258) | IROS 2026, not in the list of top venues. |
| One-Minute Video Generation with Test-Time Training (CVPR 2025; Wang senior) | Text-to-video generation, not action-conditioned or used for control; out of world-model scope. |
| Look Outside the Room: Synthesizing a Consistent Long-Term 3D Scene Video from a Single Image (CVPR 2022; Ren, Wang) | Camera-trajectory-conditioned novel-view video synthesis; borderline and excluded as not framed as a world model for prediction or control. |
| Gaussian-Augmented Physics Simulation and System Identification with Complex Colliders (NeurIPS 2025; Wang senior) | Differentiable physics simulation and system identification, not a learned world model. |
| Co-Design of Soft Gripper with Neural Physics (CoRL 2025; Wang senior) | Neural physics surrogate for hardware design; not a world model for prediction or control. |
| Do Vision-Language Models Have Internal World Models? Towards an Atomic Evaluation (Su co-author) | Only the ICLR 2025 workshop version was found; not a top-venue main-track paper. |
| On Pre-Training for Visuo-Motor Control (ICML 2023), GNFactor (CoRL 2023), Graph Inverse RL (CoRL 2022), Racing benchmark (NeurIPS 2024 D&B), TIPS (ICLR 2026), MyoChallenge 2024 (NeurIPS 2025 D&B) | Not world-model papers. |
| Evaluating Real-World Robot Manipulation Policies in Simulation (SIMPLER, CoRL 2024; Su co-author) | Policy evaluation in hand-built physics simulators, not a learned world model; senior author Ted Xiao (Google DeepMind). |
| Chain-of-Thought Predictive Control (ICML 2024), DexDeform (ICLR 2023), DiffVL (NeurIPS 2023), MovingParts (ICLR 2024) | Hierarchical imitation, differentiable physics, or dynamic-scene reconstruction; not learned world models. |
| Driver-WM (ECCV 2026), co-author "Hao Su" | A different person with the same name (NTU / Chen Lv group), not the UCSD PI. |
| Workshop versions (TD-MPC2 NeurIPS-W 2023, MoDem / XTRA NeurIPS-W 2022, IQ-MPC ICLR-W 2025) | Workshop-only duplicates of the main-venue papers above. |

## Method

- **Summary file:** read Section 1 of `world_models_academic_groups.md` as the starting point.
- **Publication lists:**
  - Nicklas Hansen homepage (full list).
  - Xiaolong Wang selected-publications page.
  - mlanthology author pages for Hao Su and Xiaolong Wang.
  - DBLP was blocked by bot protection.
- **Acceptance checks:**
  - Official virtual-site paper JSON for ICLR 2023–2026, ICML 2023–2026, NeurIPS 2023–2026, CVPR 2024–2026, ICCV 2025 and ECCV 2026; all searched by author name.
  - PMLR volume pages for CoRL 2022–2025 and ICML 2022.
  - IEEE DOI (via Semantic Scholar) for ICRA 2024.
  - OpenReview search API.
- **Paper content:** every included paper was read from its arXiv PDF (pdftotext; main text plus relevant appendix sections). Numbers come from the cited tables and figures.
- **Code and weights:** GitHub API (license, last push) and READMEs; Hugging Face model and dataset cards; project pages.
- **Citations:** Semantic Scholar Graph API batch endpoint, checked **2026-10-13**.

## Open issues

- **FACT (CoRL 2026)** and **TC-WM (claimed NeurIPS 2026)** may become in-scope once official listings confirm them. TC-WM was not in the NeurIPS 2026 list retrieved on 2026-10-13, which conflicts with the claim on Hansen's homepage.
- **MMBench2 / hallucination (NeurIPS 2026):** accepted, but the conference and proceedings are in Dec 2026.
- **Several tables were garbled by PDF text extraction** (FOWM Table 1, MoDem Fig. 1, DEMO³ Fig. 1, RoboTALES Table 1 average row). Only clearly readable numbers are reported.
- **Licenses:**
  - Newt and DEMO³ READMEs say MIT, but GitHub detects no license file.
  - Puppeteer and RoboTALES show "NOASSERTION".
  - MoDem-V2's README says CC-BY-NC while GitHub metadata says MIT.
  - Dataset and weight licenses (MMBench2, TD-MPC2 data, judge weights) are mostly unstated.
- **Not released:** weights for TD-MPC, MoDem, MoDem-V2, FOWM, IQ-MPC, UniClothDiff and DEMO³. IQ-MPC has no public code.
- **Disagreements and nuances relative to the summary file:**
  - The summary lists only TD-MPC, TD-MPC2 and Newt; this folder adds 12 more confirmed top-venue papers.
  - The summary's "one 317M-parameter agent handles 80 tasks" is correct, but that multitask agent is trained offline on 545M transitions from single-task agents, not online.
  - Newt's ICLR 2026 acceptance and "200+ checkpoints" are confirmed.
  - The summary does not mention the group's move to generative video world models (MMBench2, 350M Dreamer-4-style model, NeurIPS 2026).
