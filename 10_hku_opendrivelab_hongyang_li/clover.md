---
citation_count: 82
citation_source: "Semantic Scholar, 2026-10-15 (value carried over from existing note; re-check returned HTTP 429)"
venue: "NeurIPS 2024"
date: 2024-09-13
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# CLOVER: Closed-Loop Visuomotor Control with Generative Expectation for Robotic Manipulation (borderline)

- **Authors:** Qingwen Bu*, Jia Zeng*, Li Chen*, Yanchao Yang♮, Guyue Zhou, Junchi Yan, Ping Luo, Heming Cui, Yi Ma, **Hongyang Li**♮
  - *Equal contribution. ♮Project leads (Yanchao Yang, Hongyang Li).
  - Contact: Qingwen Bu.
- **Group role:** Hongyang Li is last author and co-project lead (Shanghai AI Lab / OpenDriveLab; co-authors from HKU, SJTU and Tsinghua).
- **Date published:** 2024-12 (NeurIPS 2024). arXiv v1 2024-09-13, v3 2024-10-16.
- **Venue:** NeurIPS 2024 (arXiv comment, README and Semantic Scholar). The presentation type was not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2409.09016
  - Code: https://github.com/OpenDriveLab/CLOVER
  - Weights: https://huggingface.co/qwbu/CLOVER
- **Number of citations:** 82 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
CLOVER is a "planner + executor" manipulation framework built around a generative visual planner. Its three parts are:

1. A text-conditioned RGB-D video diffusion model that predicts future sub-goal frames, regularized with optical flow.
2. A learned embedding space that measures the error between the current observation and the predicted sub-goal.
3. A feedback-driven inverse-dynamics policy that acts toward the sub-goal, moves to the next one when close enough, and triggers a replan when the plan looks unreachable.

It is included as borderline under the "video prediction as a world model for control" precedent (UniPi or SuSIE-style planning, compare Dream2Flow in 07_stanford_wu). The paper itself never uses the term "world model", and the video model is conditioned on text, not actions.

## 1. Weights availability
- Visual planner and feedback-driven policy checkpoints are on Hugging Face at qwbu/CLOVER, released around October 2024.

## 2. Code
- https://github.com/OpenDriveLab/CLOVER, Apache-2.0, about 136 stars. It includes planner training, CALVIN policy training and evaluation (codebase opened 2024-12-01).

## 3. Datasets
- CALVIN (ABC→D, language subset) for simulation.
- About 50 demonstrations per real-world task, RGB only because no depth sensor was available on the real robot.

## 4. Evaluation and downstream tasks
**CALVIN ABC→D (Table 1, tasks completed in a row; Avg. Len. = average number of tasks completed in a row, out of 5):**

| Method | 1 | 2 | 3 | 4 | 5 | Avg. Len. |
|---|---|---|---|---|---|---|
| UniPi | 56.0 | 16.0 | 8.0 | 8.0 | 4.0 | 0.92 |
| SuSIE | 87.0 | 69.0 | 49.0 | 38.0 | 26.0 | 2.69 |
| GR-1 | 85.4 | 71.2 | 59.6 | 49.7 | 40.1 | 3.06 |
| 3D Diffuser Actor | 92.2 | 78.7 | 63.9 | 51.2 | 41.2 | 3.27 |
| **CLOVER** | 96.0 | 83.5 | 70.8 | 57.5 | 45.4 | **3.53** |

- The "+8%" in the abstract matches 3.53 vs 3.27 (3D Diffuser Actor). Against the other planner+executor methods the gain is larger: +31% over SuSIE.

**Real robot (Table 2):** on a 3-step long-horizon pot task, Avg. Len. is 2.1, against 1.1 for RT-1, 0.7 for R3M and 0.6 for ACT. The single tasks are pour shrimp (80.0%) and stack bowls (86.7%).

**Ablations:** closed-loop feedback adds +0.44 Avg. Len. on CALVIN, and adaptive replanning adds more. Depth and optical-flow regularization improve both generation quality and control.

## 5. Architecture and techniques
- **Visual planner:** a video diffusion model predicting RGB-D frames (RGB and depth concatenated on channels, with separate noise), plus an optical-flow regularization loss with λ = 0.1 and min-SNR weighting. On CALVIN it trains for 300K iterations on 8 A100 GPUs at batch size 32, with EMA 0.999; real-world tasks are fine-tuned for 50K iterations.
- **Embedding:** separate RGB and depth encoders with feature fusion. Two queries give the current-state and goal embeddings, whose distance drives sub-goal transition and replanning thresholds.
- **Policy:** an inverse dynamics model πϕ(a | O_current, O_goal). On CALVIN it trains for 10 epochs (about 10 h on 8 A100 GPUs).

## 6. Conclusions and future work
- **Authors:** closed-loop feedback against generated visual plans makes long-horizon manipulation more robust. Future work: pre-train the planner on actionless web video and the inverse dynamics model on random-action data for few-shot use.
- **Own observation:**
  - The video model is a goal or plan generator, not an action-conditioned simulator. It cannot evaluate alternative actions.
  - Results are on CALVIN and two small real-robot tasks (15 trials).

## 7. Sources and verification
- **Read:** full arXiv v3 PDF (pdftotext, including the layout-mode tables), GitHub README and API metadata, Semantic Scholar API.
- **Not verified:** the NeurIPS proceedings page and presentation type.
