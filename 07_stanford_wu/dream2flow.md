---
citation_count: 42
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live re-check blocked by HTTP 429)"
venue: "IEEE ICRA 2026"
date: 2025-12-31
research_group: "Stanford (Jiajun Wu)"
---

# Dream2Flow: Bridging Video Generation and Open-World Manipulation with 3D Object Flow

- **Authors:** Karthik Dharmarajan, Wenlong Huang, **Jiajun Wu**, Li Fei-Fei*, Ruohan Zhang*
  - *Equal advising.
  - Corresponding author: Wenlong Huang (per the paper; the repo README marks Huang as corresponding).
- **Group role:** co-author (3rd of 5). Last authors are Li Fei-Fei and Ruohan Zhang (Stanford SVL); neither is a PI in the summary file. It is kept here as a Wu co-authored Stanford SVL paper.
- **Date published:** 2026-06 (ICRA 2026; arXiv 2025-12-31). The month is assumed from the ICRA 2026 conference dates; the IEEE DOI confirms the venue.
- **Venue:** IEEE ICRA 2026, DOI 10.1109/ICRA57385.2026.11697043 (Semantic Scholar). Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2512.24766
  - Project: https://dream2flow.github.io/
  - Code: https://github.com/KDharmarajanDev/Dream2Flow
- **Number of citations:** 42 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Dream2Flow uses off-the-shelf image-to-video generators as zero-shot visual world models for manipulation. The pipeline:

1. The video model imagines a human performing the task in the robot's scene.
2. 3D object flow (tracked object points lifted to 3D) is extracted from the generated video.
3. Robot actions are obtained by making the object follow that flow. This uses either trajectory optimization with a dynamics model (a learned particle model in simulation, a rigid-grasp model on the real robot) or RL with a flow-matching reward.

This separates what should change in the world from how the robot embodiment achieves it.

## 1. Weights availability
- No trained model weights are released. The method relies on third-party models:
  - Video generation: Veo 3 (API), Wan2.1, Kling 2.1.
  - Perception: SpatialTrackerV2, CoTracker3, Grounding DINO, SAM 2, AnyGrasp, HaMeR.
- The learned Push-T particle dynamics model was not found as a release.
- Scene data is on Hugging Face: https://huggingface.co/datasets/kdharmarajan123/Dream2Flow

## 2. Code
- **Repository:** https://github.com/KDharmarajanDev/Dream2Flow
  - Library for 3D object flow extraction (`create_3d_flow` script), with video from Veo 3 or file playback.
  - PyRoki-based trajectory optimizer and Viser visualization.
- **Not yet released:** the README TODO lists "Release RL reward design and code".
- **License:** no LICENSE file (raw `LICENSE` returns 404).
- **Last commit:** 2026-03-28.

## 3. Datasets
- No training dataset for the main method; it is zero-shot.
- Push-T dynamics model: trained on simulated push data in OmniGibson (Appendix B; size not read).
- Evaluation scenes:
  - Real robot: 3 tasks, 10 trials each, plus robustness runs (6 scenarios × 5 trials).
  - Simulation: Push-T in OmniGibson (100 trials) and Door Opening in robosuite (100 door poses).
- Example real scene data is available on Hugging Face.

## 4. Evaluation and downstream tasks
**Real robot vs. alternative interfaces (Table I, Veo 3, 10 trials each):**

| Task | Dream2Flow | AVDC | RIGVID |
|---|---|---|---|
| Bread in Bowl | 8/10 | 7/10 | 6/10 |
| Open Oven | 8/10 | 0/10 | 6/10 |
| Cover Bowl (deformable) | 3/10 | 2/10 | 1/10 |

**RL with 3D-flow reward (Table II, SAC, Door Opening, 100 door poses):** comparable to the hand-crafted object-state reward.

| Embodiment | 3D-flow reward | Object-state reward |
|---|---|---|
| Franka | 100/100 | 99/100 |
| Spot | 100/100 | 99/100 |
| GR1 | 94/100 | 96/100 |

**Choice of video model (Table III):**

| Video model | Push-T (sim) | Open Oven (real) |
|---|---|---|
| Wan2.1 | 52/100 | 2/10 |
| Kling 2.1 | 31/100 | 4/10 |
| Veo 3 | n/a (no goal-image prompting) | 8/10 |

**Choice of dynamics model for Push-T (Table IV):** particle dynamics 52/100, pose model 17/100, heuristic 12/100.

**Failure analysis (60 real trials, Fig. 7):**
- 12 video-generation failures (object morphing or hallucinated objects).
- 4 flow-extraction failures.
- 4 execution failures.

**Downstream tasks:** zero-shot open-world manipulation of rigid, articulated, deformable and granular objects; RL reward design; cross-embodiment transfer (Franka, Spot, GR1).

## 5. Architecture and techniques
- **World model:** a pretrained image-to-video diffusion model used as a zero-shot visual predictor. The prompt is text plus an initial frame without the robot.
- **3D flow extraction:**
  1. SpatialTrackerV2 estimates video depth, scale/shift-aligned to the robot's first depth frame.
  2. Grounding DINO and SAM 2 produce the object mask.
  3. CoTracker3 tracks 2D points.
  4. Points are lifted to 3D with visibility flags.
- **Action inference:** minimize Σ‖x̂_obj_t − P̃_t‖² plus control costs, subject to the dynamics model.
  - Push-T: a learned particle forward model over 14-dimensional point features, with random-shooting push primitives.
  - Real robot: a rigid-grasp dynamics model, AnyGrasp candidates chosen near the thumb detected by HaMeR, and PyRoki optimization.
  - Door: SAC with the 3D-flow reward.

## 6. Conclusions and future work
- **Authors' conclusions:** 3D object flow is a scalable, embodiment-agnostic interface between video generation and robot control.
- **Bottlenecks identified by the authors:** video artifacts, occlusion-induced tracking dropouts, and grasp selection.
- **Own observations:**
  - Real-robot evaluation is small (10 trials per task).
  - The best real-world results depend on a closed commercial model (Veo 3).
  - The rigid-grasp assumption limits deformable tasks (Cover Bowl 3/10).
  - The pipeline assumes a static camera.
  - The video model is not action-conditioned on robot actions, so it acts as a goal or plan generator rather than a closed-loop dynamics model.

## 7. Sources and verification
- **Paper:** arXiv PDF v1 https://arxiv.org/pdf/2512.24766. Read Secs. I–V and Tables I–IV. Appendices not read.
- **Code:** repo README https://github.com/KDharmarajanDev/Dream2Flow
- **Venue and DOI:** Semantic Scholar API `paper/arXiv:2512.24766` gives the venue "IEEE International Conference on Robotics and Automation" and DOI 10.1109/ICRA57385.2026.11697043.
- **Not verified:** the ICRA 2026 program page (not opened). Acceptance rests on the IEEE ICRA DOI.
