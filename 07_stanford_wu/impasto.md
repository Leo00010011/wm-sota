---
citation_count: 0
citation_source: "Semantic Scholar, 2026-10-15"
venue: "IEEE ICRA 2026"
date: 2026-03-31
research_group: "Stanford (Jiajun Wu)"
---

# IMPASTO: Integrating Model-Based Planning with Learned Dynamics Models for Robotic Oil Painting Reproduction

- **Authors:** Yingke Wang, Hao Li, Yifeng Zhu, Hong-Xing Yu, Ken Goldberg, Li Fei-Fei, **Jiajun Wu**, Yunzhu Li, Ruohan Zhang
- **Group role:** co-author (7th of 9). The last author is Ruohan Zhang (then at Stanford SVL, now Northwestern), who is not a summary PI. Yunzhu Li (PI of summary group 8) is second-to-last, not senior, so the paper is kept here rather than in 08.
- **Date published:** 2026-06 (ICRA 2026; arXiv 2026-03-31). The month is assumed from the conference date; the venue is confirmed by the IEEE DOI.
- **Venue:** IEEE ICRA 2026, DOI 10.1109/ICRA57385.2026.11696393 (Semantic Scholar). Ruohan Zhang's CV also lists it as ICRA 2026. Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2603.29315
  - Project page: https://impasto-robopainting.github.io/
- **Number of citations:** 0 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
IMPASTO learns an action-conditioned pixel dynamics model of oil-paint brushstrokes from robot self-play. Given the current canvas image and stroke parameters (start point, length, bend, angle, force), it predicts the next canvas image. A receding-horizon MPPI/CMA planner uses this model to infer the trajectories and forces needed to reproduce human artists' strokes and multi-stroke paintings. A force-controlled Franka arm executes the plan.

## 1. Weights availability
Not released. The project page has no code, weights or data links: the only GitHub href is the nerfies page template. I found no repo by search.

## 2. Code
No code found. The project page was checked for links.

## 3. Datasets
- **Training data:** a self-play dataset D = {(I_t, u_t, I_{t+1})} collected by uniform random strokes with a fixed colour set. About 900 samples give unweighted L1 = 0.0285 (Fig. 5). Not released.
- **Evaluation data (not used for training, not released):**
  - "Single Strokes": 5 expert artists × 12 strokes.
  - "Overlaid Strokes": 50 random strokes painted on top of each other by the robot.
  - 2 artist paintings: "Flower" (17 strokes) and "Fish" (18 strokes).

## 4. Evaluation and downstream tasks
- **Metrics:** weighted L1 and LPIPS, each measured at two stages: on planned predictions and on the executed canvas photo.
- **Single strokes (Table I, averaged over the artists).** IMPASTO-UNet reduces:
  - planning L_wℓ1 by 16.45% vs. FRIDA-CNN and 5.28% vs. IMPASTO-LR;
  - execution L_wℓ1 by 19.48%, 5.33% and 21.21% vs. FRIDA-CNN, IMPASTO-LR and Heuristics-only;
  - LPIPS by 35.36%, 18.21% and 15.68% vs. the same three baselines.

  It has the best LPIPS for every artist. IMPASTO-LR is slightly better on L_wℓ1 for some artists.
- **Overlaid strokes.** Execution LPIPS is 0.2209, against 0.2645 for FRIDA-CNN. Planning L_wℓ1 is 10.42% lower than FRIDA-CNN.
- **Multi-step paintings (Table II, horizon H = 5).** Compared with FRIDA-CNN, IMPASTO reduces planning L_wℓ1 by 24.01%, execution L_wℓ1 by 10.55% and LPIPS by 14.63%. Example: Flower planning L_wℓ1 is 0.0427 vs. 0.0597.
- **Complex painting.** A 204-step painting is shown qualitatively (Fig. 9).
- **Downstream task:** MPC planning for robotic painting reproduction.

## 5. Architecture and techniques
- **Action:** a quadratic-Bézier stroke u = (x0, y0, l, b, α, F).
- **Colour:** predicted separately, by nearest-neighbour matching over a database of pigment patches.
- **Dynamics model f_θ(I_t, u_t):** a U-Net image encoder plus an MLP action encoder, fused in the decoder.
  - The input includes a pre-rendered stroke image R_t, made by rasterizing the Bézier curve with a force-to-width calibration.
  - Inputs are 100×100 grayscale crops around the stroke.
  - Loss: L1 weighted by a dilated difference mask. Optimizer: Adam. Training uses data augmentation.
- **Planner:** receding-horizon MPC with H = 5, executing 2 steps before replanning.
  - Initialization: PCA–RANSAC splitter.
  - Sample-and-refine: MPPI with CMA-style covariance adaptation and EMA updates.
  - Cost: weighted image L1 to the goal.
- **Control:** Franka in impedance mode with a normal-force admittance loop, using a tip-mounted F/T sensor.

## 6. Conclusions and future work
- **Authors' conclusion:** learned dynamics plus model-based planning lets a robot approximate human brushstrokes and paintings without demonstrations or a simulator.
- **Limitations noted in the method section:**
  - The stroke parameterization cannot capture serif-like endings or abrupt changes of direction.
  - Execution is stochastic, so evaluation required controlled brush and paint conditions.
- **Own observations:**
  - The world model is narrow: single grayscale stroke patches, with colour handled separately.
  - Gains over the IMPASTO-LR ablation are small in L_wℓ1.
  - There is no code or data release.

## 7. Sources and verification
- **Paper:** arXiv PDF v1, https://arxiv.org/pdf/2603.29315. Read Secs. I–V and Tables I–II.
- **Project page:** https://impasto-robopainting.github.io/, checked for code links.
- **Venue:** Semantic Scholar API (`paper/arXiv:2603.29315`) gives the venue and DOI. Ruohan Zhang's CV (via web search) also lists ICRA 2026.
- **Not verified:** the ICRA program page (not opened).
