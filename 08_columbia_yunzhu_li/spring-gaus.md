---
citation_count: 127
citation_source: "Semantic Scholar, 2026-10-13 (carried over from note; live re-check returned HTTP 429 on 2026-10-15)"
venue: "ECCV 2024"
date: 2024-03-14
research_group: "Columbia (Yunzhu Li)"
---

# Reconstruction and Simulation of Elastic Objects with Spring-Mass 3D Gaussians (Spring-Gaus)

- **Authors:** Licheng Zhong, Hong-Xing Yu, Jiajun Wu, **Yunzhu Li**
- **Group role:** Senior (last) author. The paper lists Yunzhu Li's affiliations as Stanford, Columbia and UIUC. Jiajun Wu (07) is second-to-last, so by the last-author rule the paper belongs here.
- **Date published:** 2024-09/10 (ECCV 2024, Milan, 29 September to 4 October 2024). arXiv v1 2024-03-14 (v3 2024-07-19).
- **Venue:** European Conference on Computer Vision (ECCV) 2024. The BibTeX on the project page and the PI's homepage give ECCV 2024, as does the Semantic Scholar venue field. Presentation type not found.
- **Links:** arXiv https://arxiv.org/abs/2403.09434 · Project https://zlicheng.com/spring_gaus/ · Code https://github.com/Colmar-zlicheng/Spring-Gaus · Data https://drive.google.com/drive/folders/1SN4vrPZSuBnbkDTLl0nu9g-boVUr5yO1
- **Number of citations:** 127 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Spring-Gaus builds a physics-structured, simulatable world model of an elastic object from multi-view video. The object's appearance is a set of static 3D Gaussians. Its dynamics come from a differentiable spring-mass system on sampled anchor points:
- Stiffness is learned per anchor.
- A learnable "soft vector" sets the effective spring connectivity.
- Boundary-condition parameters such as friction are learned.

The Gaussians follow the anchors by inverse-distance weighting. The physical parameters are identified by differentiable simulation and rendering. The resulting asset predicts future motion and can be re-simulated under new initial velocities, gravity, stiffness and ground conditions.

## 1. Weights availability
- There are no pretrained weights in the usual sense: each object is optimised per scene.
- The repo saves per-run checkpoints (`exp/{EXP_ID}_{timestamp}/checkpoints/`, file `Spring_Mass.pth.tar`) and evaluates them with `test.py --dy_reload`. The README does not list released pretrained checkpoints.
- Synthetic and real data are released on Google Drive (link above).

## 2. Code
- https://github.com/Colmar-zlicheng/Spring-Gaus, Apache-2.0 (LICENSE file).
- Contains training (`train.py` with per-object configs, e.g. `config/mpm_synthetic/torus.yaml`, `config/real_capture/burger.yaml`) and evaluation (`test.py`). Single-GPU only.
- Last-commit date not verified (GitHub API rate-limited).

## 3. Datasets
- **Synthetic (Sec. 4.1):** 14 3D models, some from PAC-NeRF and OmniObject3D. They are simulated with MPM and rendered in Blender: 10 views × 30 frames at 512×512. Frames 1–20 are used for reconstruction and frames 21–30 for future-prediction evaluation.
- **Real (Sec. 4.1):** static captures of 50–70 images per object (COLMAP poses), plus 3-view dynamic videos at 1920×1080 with checkerboard calibration and SAM masks. Objects include bun, burger, pig and dog.
- Both are public on Google Drive (link above).

## 4. Evaluation and downstream tasks
- **Future prediction on synthetic data:** the 7-object mean from Table 1. CD is in units of 1×10³ mm², squared distance.

  | Metric | Spring-Gaus | PAC-NeRF |
  |---|---|---|
  | CD ↓ | 5.16 | 17.94 |
  | EMD ↓ | 0.095 | 0.134 |
  | PSNR ↑ | 17.06 | 15.77 |
  | SSIM ↑ | 0.897 | 0.870 |

- **Dynamic reconstruction on synthetic data:** the mean from Table 2.

  | Metric | Spring-Gaus | PAC-NeRF | 4D-Gaussians | Dynamic 3D Gaussians |
  |---|---|---|---|---|
  | CD ↓ | 0.85 | 2.11 | 4.43 | 1305 |
  | EMD ↓ | 0.040 | 0.052 | 0.095 | 1.116 |

- **Real world:** qualitative future prediction (Fig. 5) and re-simulation under edited boundary and physical conditions (Figs. 6–7). There are no quantitative real-world metrics.
- **Downstream tasks:** future prediction, system identification, and simulation of digital assets under new conditions. No robot control in this paper.

## 5. Architecture and techniques
- **Representation:** isotropic 3D Gaussians (center, scale, colour, opacity) for appearance, plus anchors from volume sampling for physics.
- **Dynamics:**
  - Each anchor connects to its kNN anchors by springs with stiffness k_i/l_ij and damping ζ0/l_ij.
  - An optional nonlinear spring law uses exponent p_k.
  - Integration is semi-implicit Euler with learnable boundary conditions.
  - Gaussians follow the anchors by inverse-distance-weighted interpolation (Eqs. 5–15).
- **Optimisation stages:** (a) static 3DGS on the first frame; (b) Gaussian refinement after anchor interpolation; (c) dynamic reconstruction. Stage (c) optimises the initial velocity, per-anchor stiffness, κ and the boundary parameters through differentiable simulation and rendering, with an increasing keyframe interval.
- **Loss:** L1 + D-SSIM. Real data adds mask-center and VGG perceptual losses, and a registration step aligns static and dynamic coordinates.
- **Size:** there are no network weights to count; the parameters are physical.

## 6. Conclusions and future work
- **Authors:** Spring-Gaus yields simulatable digital assets of elastic objects from video. Separating appearance and physics learning avoids optimisation interference, and the method predicts short-term future dynamics under new conditions. No explicit future-work list.
- **My own observations:**
  - The method covers elastic objects only and needs multi-view calibrated video.
  - Real-world results are qualitative.
  - Predictions are evaluated for 10 frames, which is short-term.
  - No external actions or robot interaction are modelled. That gap is addressed later by PhysTwin from the same group.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2403.09434 (v3, full main text read).
- Code README and LICENSE: raw.githubusercontent.com (Colmar-zlicheng/Spring-Gaus).
- Project page: https://zlicheng.com/spring_gaus/
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - The ECVA or Springer proceedings page (not fetched).
  - Presentation type.
  - Repository last-update date.
