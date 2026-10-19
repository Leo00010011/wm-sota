# World Models: Publication Record by Academic Group

Compiled 2026-10-15 from `world_models_academic_groups.md`. Each group folder has a `README.md` (included papers, cross-group papers, exclusions, method, open issues) and one markdown file per paper with the nine requested fields: weights, code, datasets, evaluation and downstream tasks, architecture, conclusions and future work, publication date, venue, citations.

Citation counts come from Semantic Scholar, checked 2026-10-13 to 2026-10-15, unless a group README says otherwise. Google Scholar counts were used only where Semantic Scholar was unavailable (group 2).

## Groups

| # | Folder | Group | Papers | Notes |
|---|---|---|---|---|
| 1 | [01_ucsd_wang_su](01_ucsd_wang_su/README.md) | UC San Diego (Wang, Su, Hansen) | 15 | TD-MPC line, Newt |
| 2 | [02_geneva_fleuret](02_geneva_fleuret/README.md) | Geneva (Fleuret) | 3 | No 2025–26 top-venue papers found |
| 3 | [03_nyu_brown_lecun_balestriero](03_nyu_brown_lecun_balestriero/README.md) | NYU / Brown (LeCun, Balestriero) | 12 | LeWM is arXiv-only, so excluded |
| 4 | [04_athena_crete_valeo_komodakis_gidaris](04_athena_crete_valeo_komodakis_gidaris/README.md) | Athena / Crete + valeo.ai | 3 | Feature-space forecasting |
| 5 | [05_mit_sitzmann_tedrake](05_mit_sitzmann_tedrake/README.md) | MIT (Sitzmann, Tedrake) | 8 | Diffusion Forcing, DFoT |
| 6 | [06_tsinghua_long_zhu](06_tsinghua_long_zhu/README.md) | Tsinghua (Long, Zhu) | 8 | minWM excluded (arXiv only) |
| 7 | [07_stanford_wu](07_stanford_wu/README.md) | Stanford (Jiajun Wu) | 18 | Includes co-authored papers without a summary-PI senior author |
| 8 | [08_columbia_yunzhu_li](08_columbia_yunzhu_li/README.md) | Columbia (Yunzhu Li) | 16 | Particle / physics-informed models |
| 9 | [09_stanford_toronto_policy_eval](09_stanford_toronto_policy_eval/README.md) | Stanford / Toronto (policy evaluation) | 17 | WorldGym, Ctrl-World, dWorldEval, VLAW |
| 10 | [10_hku_opendrivelab_hongyang_li](10_hku_opendrivelab_hongyang_li/README.md) | HKU / OpenDriveLab (Hongyang Li) | 7 | Driving world models; 3 borderline papers |
| 11 | [11_shanghai_ai_lab](11_shanghai_ai_lab/README.md) | Shanghai AI Lab (Physical Intelligence Team) | 2 | Weakest group: see judgment calls |

Total: 109 paper files.

## Ownership decisions across groups

Each paper lives in one folder: the group whose PI is last or corresponding author.

- **Puppeteer** (ICLR 2025): folder 01 (Su last). Also listed in group 3's material.
- **DPI-Net** (ICLR 2019) and **3D Neural Scene Representations** (CoRL 2021): folder 08 (Yunzhu Li first author). Not in folder 05.
- **Koopman** (compositional), **VGPL**, **V-CDN**: folder 08 (Li first or corresponding author). **V-CDN's last author is Animesh Garg**, who may be the unnamed Toronto PI in group 9. Open: move it to folder 09 if so.
- **RoboCraft, RoboCook, RoboPack, Dynamic-Resolution, VP²** (ICLR 2023), **O2P2** (ICLR 2019), **PSD**: folder 07 (Wu last).
- **Sparse Neural Dynamics, Spring-Gaus**: folder 08. **PropNet, Keypoints into the Future, Compositional NeRF**: folder 05.
- **Co-authored papers with no summary-PI senior author** (MaskViT, PhysDreamer, Dream2Flow, IMPASTO in 07; ReSim in 10): kept in the folder of the group that lists them, so the rule is applied the same way across groups.
- **Hansen-only papers** (MoDem-V2, PWM, RoboTALES) in folder 01 have no Wang or Su authors. They are kept because Hansen is listed under UCSD; drop them if strict PI authorship is required.
- **Aether, OmniWorld** in folder 11: senior author Tong He is not a current team PI. Kept as a judgment call. Under a strict reading, folder 11 would be empty and the team has no top-venue world-model papers under its own name.

## Corrections to the summary file

Verified against the papers by the group agents:

- **Group 1 (UCSD):** Hansen-only papers noted above.
- **Group 3 (NYU):** Puppeteer belongs in folder 01, not here.
- **Group 5 (MIT):** DFoT has no zero-shot robot motion planning. Its robot result is imitation learning.
- **Group 4 (Athena):** the "48 future frames / 2.9 s" figure for DINO-Foresight was not found in the paper.
- **Group 8 (Columbia):** PhysWorld (arXiv 2510.21447) is by Wangmeng Zuo's group, not Yunzhu Li.
- **Group 9 (Stanford / Toronto):** WPE and WorldGym are the same paper (ICLR 2026). arXiv 2607.02642 is GigaWorld-1, not WMBench, and is excluded.
- **Group 10 (HKU):** Vista's "+55% FID, +27% FVD" uses different baselines for each metric. ReSim's "+44%" is FVD, "+2%" is absolute NAVSIM score, and "+25%" is absolute policy-selection score (the table caption says +26%). GenAD's OpenDV is 1,747 h of YouTube data (2,059 h full). RISE's "+45%" is measured against the RECAP baseline. The strongest baseline gives +35%.
- **Group 11 (Shanghai AI Lab):** arXiv 2607.06401 (the summary's only item) is an arXiv-only technical report and is excluded.

## Unresolved conflicts

- **RoboCook award:** folder 08 says it won an award. Folder 07 says it was only a finalist. Check the paper before citing either.
- **dWorldEval:** listed as both spotlight and poster. The venue is ICML 2026 either way.
- **Re2Pix:** numbers come from arXiv v1, not the ECCV camera-ready version.
- **Borderline inclusions** (drop if the scope is narrowed to action-conditioned video or sensor-level world models): ViDAR, Nexus, CLOVER (folder 10); COMBO, LDP, UPN, RoboNet (folder 09); Foresight Diffusion (06); Generative View Stitching (05); Re2Pix (04); Dream2Flow (07); PWM / RoboTALES (01).

## Known limitations

- DBLP, OpenReview and the arXiv API were blocked for several agents, so a few top-venue papers by these groups may be missing. Each group README lists what it checked.
- Team membership in group 11 is inferred from author lists, not from a published roster.
- Repository license and last-commit dates were not confirmed for every repo (GitHub API rate limits).
- Some table values were reconstructed from garbled PDF text. The affected files flag this.
- Group 7's shared instructions file could not be found when the group 11 agent looked for it. The agent used the format of the existing folders instead, which matches the other groups.
