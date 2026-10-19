---
citation_count: 303
citation_source: "Semantic Scholar, 2026-10-13 (carried over; 2026-10-15 re-check returned HTTP 429)"
venue: "CVPR 2026"
date: 2025-12-15
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# Motus: A Unified Latent Action World Model

- **Authors:** Hongzhe Bi, Hengkai Tan, Shenghao Xie, Zeyuan Wang, Shuhe Huang, Haitian Liu, Ruowen Zhao, Yao Feng, Chendong Xiang, Yinze Rong, Hongyan Zhao, Hanyu Liu, Zhizhong Su, Lei Ma, Hang Su, **Jun Zhu**
- **Group role:** Senior / last author (Jun Zhu, TSAIL, Tsinghua). Hang Su (TSAIL) is second to last. Bi and Tan are joint project leads. Co-authors are from Peking University and Horizon Robotics.
- **Date published:** 2026-06 (CVPR 2026; poster session 2026-06-07). arXiv v1 2025-12-15, v2 2025-12-25.
- **Venue:** CVPR 2026, poster.
- **Links:** arXiv https://arxiv.org/abs/2512.13030 · poster https://cvpr.thecvf.com/virtual/2026/poster/38174 · project https://motus-robotics.github.io/motus · code https://github.com/thu-ml/Motus · weights https://huggingface.co/motus-robotics
- **Number of citations:** 303 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Motus is an embodied foundation model that unifies five modes in one network: world model p(o' | o, a), VLA, inverse dynamics, video generation, and joint video-action prediction. It is a Mixture-of-Transformers combining a pretrained video generator (Wan2.2-5B), a VLM (Qwen3-VL-2B) and an action expert, joined through shared "Tri-model Joint Attention". A UniDiffuser-style scheduler gives separate flow-matching timesteps to video and actions, which lets the model switch between modes. Latent actions derived from optical flow allow action-expert pre-training on unlabeled videos.

## 1. Weights availability
- Released on Hugging Face (organization `motus-robotics`), linked from the repo, which is Apache-2.0.
- `motus-robotics/Motus_Wan2_2_5B_pretrain` (stage 1, video-generation backbone), `motus-robotics/Motus` (stage 2, latent-action pre-trained), `motus-robotics/Motus_robotwin2` (stage 3, fine-tuned on RoboTwin 2.0).
- Total size about 8B per the README: VGM 5.00B, VLM 2.13B, action expert 641.5M, understanding expert 253.5M.
- Requires the third-party base models Wan2.2-TI2V-5B and Qwen3-VL-2B-Instruct.

## 2. Code
- https://github.com/thu-ml/Motus, Apache-2.0, about 1.3k stars. Releases from 2025-12-16 to 2025-12-27 added checkpoints, training code, RoboTwin inference, and LeRobot dataset support.
- Contains a three-stage training pipeline (`train/`, TRAINING.md), inference (`inference/`, INFERENCE.md), RoboTwin 2.0 evaluation and data converters.
- The project page also links `motus-robotics/Motus` and an older arXiv ID (2507.23523). These look like related or legacy resources.

## 3. Datasets
- Six-level "embodied data pyramid": web data (through the pretrained VGM and VLM), egocentric human videos, synthetic data, task-agnostic data (Curobo random actions, following AnyPos), multi-robot trajectories, and target-robot trajectories.
- The paper cites the public datasets it uses but does not list exact sizes in the main text. No datasets are released by the authors.
- **Evaluation:** RoboTwin 2.0 simulation (50 tasks; 2,500 clean plus 25,000 randomized demonstrations), and real AC-One and Agilex-Aloha-2 dual-arm robots (100 trajectories per task).

## 4. Evaluation and downstream tasks
- **Policy (VLA / joint mode), RoboTwin 2.0, average success over 50 tasks (Table 2):**

| Model | Clean | Randomized |
|---|---|---|
| pi0.5 | 42.98% | 43.84% |
| X-VLA | 72.80% | 72.84% |
| w/o pre-train | 72.8% | 77.00% |
| Stage 1 only | 82.86% | 81.86% |
| Motus | 88.66% | 87.02% |

- **Real world (Table 3, partial success rate):** beats pi0.5 on both platforms (abstract: +11 to 48%). Example: Put Bread into Oven, 42% for Motus against 12% for pi0.5.
- **World-model mode (Table 6, real robot data):**

| Platform | FID | FVD | SSIM | LPIPS | PSNR |
|---|---|---|---|---|---|
| Agilex-Aloha-2 | 9.46 | 49.3 | 0.886 | 0.054 | 26.1 |
| AC-One | 12.96 | 73.1 | 0.846 | 0.073 | 24.0 |

  The world-model mode is reported only as generation quality, with no world-model baseline and no use in planning or policy evaluation.
- **Other modes:** IDM, video generation and joint prediction are shown qualitatively or with VLA-vs-joint success rates in the appendix (Table 8).
- **Ablation (Fig. 6):** stage-1 and stage-2 pre-training each add success on RoboTwin randomized.

## 5. Architecture and techniques
- **Mixture-of-Transformers:** the generative expert is Wan2.2-5B (video diffusion transformer). The understanding expert takes the last-layer tokens of Qwen3-VL-2B. The action expert is a transformer of Wan's depth with AdaLN timestep injection. All three share joint self-attention.
- **Training objective:** rectified flow on both video and action chunks, with independent timesteps per modality (UniDiffuser-like). This gives world-model mode (clean actions, noisy video), VLA mode, IDM, VGM and joint mode.
- **Action-dense, video-sparse prediction:** video frame rate is about 1/6 of the action rate, to balance token counts.
- **Latent actions:** DPFlow optical flow is rendered as RGB, encoded by DC-AE into 4x512 tokens, then projected to a 14-dim latent action. Training uses reconstruction, alignment to real actions on 10% labeled data, and KL.
- **Three training stages:** video generation adaptation, unified latent-action pre-training (VLM frozen), and target-robot SFT.

## 6. Conclusions and future work
- **Authors:** unifying VLM and VGM priors with motion-based latent actions improves embodied policies in simulation (+15% over X-VLA, +45% over pi0.5) and on real robots. Future work: better unified architectures, more universal motion priors, and learning latent actions from internet-scale video.
- **Own observation:** The paper's main evaluation is policy success rate (VLA-style). The world-model capability is a secondary mode, evaluated only by generation metrics with no baselines. Its inclusion here rests on the paper's own "world model" framing. The 8B size makes it far heavier than the other papers in this folder.

## 7. Sources and verification
- Full arXiv PDF v2 (main text and appendix Table 6, from a layout-mode extraction), CVPR 2026 poster page, project page, GitHub README, Semantic Scholar API.
- Not verified: the CVPR proceedings (CVF open access) URL, per-dataset pre-training sizes, and full per-task real-world numbers.
