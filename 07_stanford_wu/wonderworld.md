---
citation_count: 235
citation_source: "Semantic Scholar, 2026-10-13 (from existing note; live re-check returned HTTP 429 on 2026-10-15)"
venue: "CVPR 2025"
date: 2024-06-13
research_group: "Stanford (Jiajun Wu)"
---

# WonderWorld: Interactive 3D Scene Generation from a Single Image

- **Authors:** Hong-Xing Yu*, Haoyi Duan*, Charles Herrmann, William T. Freeman, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author; first authors are Wu's group members at Stanford
- **Date published:** 2025-06 (CVPR 2025 conference, June 2025; arXiv v1 2024-06-13)
- **Venue:** CVPR 2025 (main conference; DOI 10.1109/CVPR52734.2025.00555)
- **Links:** arXiv https://arxiv.org/abs/2406.09394 · project https://kovenyu.com/WonderWorld/ · code https://github.com/KovenYu/WonderWorld
- **Number of citations:** 235 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
WonderWorld turns one image into a large, explorable 3D world that grows as the user acts. The user moves a camera and types a text prompt, and the system generates and attaches a new 3D scene in under 10 s per scene on one A6000 GPU. It is a camera- and text-conditioned 3D *world generator*: it extrapolates unseen space, not time. It does not model dynamics.

## 1. Weights availability
- No weights trained for this paper. The pipeline is training-free and uses off-the-shelf models:
  - Stable Diffusion 2 inpainting: https://huggingface.co/stabilityai/stable-diffusion-2-inpainting
  - Marigold depth and normals
  - OneFormer for segmentation
  - RepViT-SAM, from https://github.com/THU-MIG/RepViT/releases/download/v1.0/repvit_sam.pt
  - An optional LLM (GPT-4) for scene descriptions
- The README explains how to get each model. It states that 48 GB of GPU memory is needed.

## 2. Code
- https://github.com/KovenYu/WonderWorld: full generation code, config examples, and a local web viewer (via `haoyi-duan/splat`).
- There is no training code because nothing is trained.
- No LICENSE file was found in the repo root (raw `LICENSE` returns 404), so no license is declared.
- Last commit: 2025-04-14 (from the GitHub commits Atom feed).
- WorldScore's repo has added WonderWorld evaluation code (11/2025).

## 3. Datasets
- No training data.
- Evaluation used four test examples (real and synthetic images of city, campus, nature and fantasy scenes), with 7 scenes generated each, for 28 scenes in total. Some examples come from WonderJourney and LucidDreamer (Sec. 4). This set has not been released as a named benchmark.

## 4. Evaluation and downstream tasks
- **Task:** interactive / extrapolative 3D scene generation along a fixed panoramic camera path. There are no robotics, planning or RL tasks.
- **Speed (Table 1):** 9.5 s per scene, against 749.5 s (WonderJourney), 798.1 s (LucidDreamer) and 766.9 s (Text2Room).
- **Novel-view quality (Table 2):**

  | Metric | WonderWorld | Best baseline |
  |---|---|---|
  | CLIP score | 29.47 | 27.34 (WonderJourney) |
  | CLIP consistency | 0.9948 | 0.9544 |
  | CLIP-IQA+ | 0.6512 | 0.6443 |
  | Q-Align | 3.6411 | 2.7355 |
  | CLIP aesthetic | 5.9543 | 5.6007 |

- **Human 2AFC (Table 3, 204 responses):** preferred 98.5% / 98.6% / 98.0% over WonderJourney / LucidDreamer / Text2Room.
- **Ablations (Table 4):** removing geometry-based initialization, layers or depth guidance lowers every metric. For example, CLIP score drops from 29.47 to 27.23–27.32.

## 5. Architecture and techniques
- **Representation:** FLAGS (Fast LAyered Gaussian Surfels). Three layers (foreground, background, sky) of 2D Gaussian surfels with view-independent colour.
- **Initialization:** geometry-based. Each pixel becomes one surfel, with position from monocular depth, orientation from estimated normals, and scale from a Nyquist-sampling rule. Optimization is then only 100 Adam iterations per layer, with no densification.
- **Content generation:** text-guided outpainting with SD-inpainting. An LLM produces structured foreground, background and style prompts. Occluded regions are inpainted at the layer level, not by generating many views.
- **Guided depth diffusion:** training-free guidance of the Marigold latent depth diffusion. It adds a gradient that matches the depth already rendered from existing geometry, which reduces seams between scenes.
- **Action conditioning:** user camera pose and text prompt only. There are no physical actions and no time dimension.

## 6. Conclusions and future work
- **Authors' conclusions:**
  - It is presented as the first interactive 3D scene generation system.
  - Limitations: scenes have only front-facing surfaces (the back sides of objects are not generated), and fine structures such as trees leave holes or floaters.
  - Suggested future work: add a 3D object generator (e.g., GRM), and use WonderWorld as a coarse prototyping tool refined by slower video-diffusion models.
- **Own observations:**
  - The world is static. There is no prediction of future states, so this is a "world generator" rather than a dynamics world model. It is included because the summary lists it and because it is the base of the later dynamic work (WonderPlay, PerpetualWonder).
  - The evaluation is small (28 scenes) and relies on CLIP-based proxies. There is no geometric ground truth.

## 7. Sources and verification
- arXiv PDF v4 (CVPR 2025 version): https://arxiv.org/pdf/2406.09394. Read Secs. 1–5 and Tables 1–4.
- Project page https://kovenyu.com/WonderWorld/ and repo README https://github.com/KovenYu/WonderWorld
- Semantic Scholar API: `paper/arXiv:2406.09394`. It gives the venue "Computer Vision and Pattern Recognition" and the CVPR 2025 DOI.
- Wu's homepage (https://jiajunwu.com/) lists "CVPR 2025".
- Not verified: presentation type (oral, highlight or poster). The CVF listing was not opened.
