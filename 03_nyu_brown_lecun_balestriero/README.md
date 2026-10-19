# NYU / Brown / Mila network — Yann LeCun, Randall Balestriero

- **PIs:** Yann LeCun (NYU Courant / Center for Data Science; Meta FAIR until 2025), Randall Balestriero (Brown University, Computer Science).
- **Lab / project pages:** LeCun http://yann.lecun.com · Balestriero https://randallbalestriero.github.io · Balestriero's code organization (galilai-group) https://github.com/galilai-group · stable-worldmodel https://galilai-group.github.io/stable-worldmodel/
- **Line:** JEPA-style world models that predict in representation space and plan there (DINO-WM, PLDM, LeWM, C-JEPA, SMWM), plus video world models for navigation and egocentric prediction (NWM, PEVA) and planning methods and tooling for latent world models (GRASP, temporal straightening, stable-worldmodel).

## Included papers (top venue, accepted, world-model topic)

Citation counts: Semantic Scholar API, checked 2026-10-13.

| Paper | Venue | Year | Group role | Citations |
|---|---|---|---|---|
| [DINO-WM: World Models on Pre-trained Visual Features enable Zero-shot Planning](dino-wm.md) | ICML (poster) | 2025 | LeCun co-author (last: L. Pinto, NYU) | 472 |
| [Navigation World Models](navigation-world-models.md) | CVPR (oral) | 2025 | LeCun senior | 350 |
| [Learning from Reward-Free Offline Data: A Case for Planning with Latent Dynamics Models (PLDM)](pldm.md) | NeurIPS (poster) | 2025 | LeCun senior, Balestriero co-author | 110 |
| [Whole-Body Conditioned Egocentric Video Prediction (PEVA)](peva.md) | NeurIPS (poster) | 2025 | LeCun co-senior (last: J. Malik) | 34 |
| [OSVI-WM: One-Shot Visual Imitation for Unseen Tasks using World-Model-Guided Trajectory Generation](osvi-wm.md) | NeurIPS (poster) | 2025 | LeCun co-author (last: F. Khorrami, NYU) | 12 |
| [Causal-JEPA: Learning World Models through Object-Level Latent Masking](causal-jepa.md) | ICML | 2026 | Balestriero senior, LeCun co-author | 19 |
| [Temporal Straightening for Latent Planning](temporal-straightening.md) | ICML (poster) | 2026 | LeCun co-senior, Balestriero co-author (last: M. Ren, NYU) | 42 |
| [Learning Latent Action World Models in the Wild](latent-action-world-models-in-the-wild.md) | ICML (poster) | 2026 | LeCun co-author (last: M. Rabbat, Meta) | 48 |
| [Parallel Stochastic Gradient-Based Planning for World Models (GRASP)](grasp-parallel-stochastic-planning.md) | ICML | 2026 | LeCun co-author (last: A. Bar, Meta) | 9 |
| [When Does LeJEPA Learn a World Model?](lejepa-world-model-identifiability.md) | NeurIPS (poster) | 2026 | Balestriero senior, LeCun co-author | 30 |
| [stable-worldmodel: A Platform for Reproducible World Modeling Research and Evaluation](stable-worldmodel.md) | NeurIPS, Evaluations & Datasets Track (poster) | 2026 | Balestriero senior, LeCun co-author | 18 |
| [Sensorimotor World Models: Perception for Action via Inverse Dynamics](sensorimotor-world-models.md) (NeurIPS title: "Perception for Action in Latent World Models") | NeurIPS (poster) | 2026 | Balestriero co-author (last: B. Schölkopf) | 14 |

NeurIPS 2026 papers are accepted (decision "Accept (poster)" in the official NeurIPS 2026 accepted-papers data) but the conference takes place in December 2026, so proceedings are not yet published.

Ownership note: several papers have a last author who is not LeCun or Balestriero (Pinto, Malik, Khorrami, Ren, Rabbat, Bar, Schölkopf). None of these is a PI of another group in the summary file, so the papers are kept here, with the group role stated in each file.

## Cross-group papers

| Paper | Venue | Year | Owning folder |
|---|---|---|---|
| Hierarchical World Models as Visual Whole-Body Humanoid Controllers (Puppeteer; Hansen, Jyothir S V, Sobal, LeCun, X. Wang, H. Su) | ICLR (poster) | 2025 | Section 1 group folder (UC San Diego — Xiaolong Wang & Hao Su); last author Hao Su |

## Excluded items

| Item | Reason |
|---|---|
| LeWorldModel (LeWM), arXiv 2603.19312 (Maes, Le Lidec, Scieur, LeCun, Balestriero), 282 citations | arXiv only as of 2026-10-13 (v3, 2026-06-03, no journal-ref or venue comment); not in ICML 2026 PMLR v306 and not in the NeurIPS 2026 accepted-papers list |
| Latent Geometry Beyond Search (arXiv 2605.08732) | arXiv only; authors Hoang Nguyen, Xiaohao Xu, Xiaonan Huang — not this group |
| FF-JEPA (arXiv 2606.09311) | arXiv only; authors Masip, Swinnen, Hu, Detry, Tuytelaars (KU Leuven) — not this group |
| Beyond the Next Step: Variable-Length Latent World Models (arXiv 2606.21775) | arXiv only; authors Du, Zhang, Y. Wang, Y. Wang (Peking University / Amazon) per secondary listing — not this group |
| Dual-WM: Beyond a single latent space (arXiv 2609.37644) | arXiv only; authors Zhao, Yue, …, Limin Wang, Yali Wang — not this group |
| Hierarchical Planning with Latent World Models (arXiv 2604.03208; Zhang, …, Balestriero, Bar, Bardes, LeCun, Ballas) | arXiv only (OpenReview lists it as CoRR 2026) |
| H-JEPA: End-to-End Learning of Hierarchical World Models for Visual Planning (arXiv 2610.06805; Zhang, Terver, Rabbat, LeCun, Balestriero) | arXiv only (submitted 2026-10-05) |
| What Drives Success in Physical Planning with JEPA World Models? (JEPA-WM, arXiv 2512.24497; last author LeCun) | Accepted by TMLR — not a top venue under the scope rules (an ICLR 2026 submission is listed on OpenReview only as "Submitted to ICLR 2026", i.e. not accepted there) |
| V-JEPA 2 (arXiv 2506.09985) and V-JEPA 2.1 | arXiv only (Semantic Scholar venue arXiv; OpenReview CoRR); last author N. Ballas |
| Intuitive physics understanding emerges from self-supervised pretraining on natural videos (arXiv 2502.11831) | arXiv only |
| Learning and Leveraging World Models in Visual Representation Learning (IWM, arXiv 2403.00504) | arXiv only |
| Gradient-based Planning with World Models (arXiv 2312.17227) | arXiv only |
| Value-guided action planning with JEPA world models (arXiv 2601.00844) | Workshop poster (World Modeling Workshop 2026) / arXiv |
| AdaJEPA: An Adaptive Latent World Model (Wang, Bounou, LeCun, Ren) | arXiv; ICLR 2027 submission (under review) |
| A Lightweight Library for Energy-Based JEPAs (EB-JEPA) | ICLR 2026 World Models workshop only |
| stable-worldmodel-v1 (arXiv 2602.08968) | ICLR 2026 workshop tiny paper; superseded by the NeurIPS 2026 paper above |
| LeVLWM: Text-Goal Control with a Single Vision-Language World Model | Workshop only (NeurIPS 2026 PTA, WM-PAI) |
| Stress-Testing Offline Reward-Free RL (PLDM workshop version) | ICLR 2025 workshop; main version included as PLDM |
| Interpreting Physics in Video World Models (ICML 2026; Joseph, Garrido, Balestriero, …, Rabbat) | Top venue, but an interpretability study of video encoders (V-JEPA-style); it does not build or use a predictive world model. Borderline, excluded |
| Towards Financial World Modeling (NeurIPS 2026; Balestriero co-author) and Joint-Embedding Predictive Learning of Latent Market States (ICML 2026) | Representation learning on market data; no action-conditioned future-state world model. Excluded as out of scope |
| LeJEPA, LLM-JEPA, VL-JEPA, Semantic Tube Prediction, Rectified LpJEPA (ICLR/ICML 2026) | JEPA representation learning, not world models |

## Method

- Read Section 3 of `world_models_academic_groups.md` as a starting point.
- Venue checks: PMLR v267 (ICML 2025) and v306 (ICML 2026) full indexes, filtered by author name; NeurIPS 2026 accepted-papers JSON (`neurips-2026-orals-posters.json`, 9,242 entries) filtered by author; CVF Open Access CVPR 2026 and ICCV 2025 listings (no LeCun/Balestriero world-model papers); OpenReview search API (venue fields for NeurIPS 2025, ICLR 2025/2026, ICML 2026, workshops); NeurIPS 2025 proceedings/poster pages; CVPR 2025 oral page; arXiv abstract pages (comments/journal-ref) for LeWM and the 2026 follow-ups; Balestriero's homepage; web searches.
- DBLP and Google Scholar could not be used (DBLP returned a bot-protection page; no Scholar access). OpenReview profile queries required login.
- Papers read from arXiv HTML or PDF (main text and relevant appendices); code, license and weights from the GitHub API and Hugging Face API.
- Citation counts: Semantic Scholar Graph API batch endpoint, checked 2026-10-13.

## Open issues

- LeWM, the flagship paper in the summary, is not in a top venue yet; it may appear in a later venue (e.g. ICLR 2027). Its summary figures (15M parameters, 48× faster planning, 200× fewer tokens) were not verified because it is excluded.
- Disagreements with the summary file:
  - DINO-WM is listed as an NYU paper without venue; it is ICML 2025, and its last author is Lerrel Pinto (LeCun is third author).
  - The summary says DINO-WM "beat DreamerV3 and TD-MPC2"; in the paper's Table 1, DreamerV3 is slightly better on Maze (1.00 vs 0.98) and Wall (1.00 vs 0.96); DINO-WM wins on Reach, PushT, Rope and Granular.
  - The four "follow-up preprints" (Latent Geometry Beyond Search, FF-JEPA, Variable-Length Latent World Models, Dual-WM) are all by other groups and all arXiv-only.
  - The summary's "Known gaps" says no 2025–26 Mila world-model papers were found; this group has several (LeWM, C-JEPA, stable-worldmodel with Mila first authors).
- NWM weights: the GitHub repo license is CC BY-NC 4.0, while the gated Hugging Face metadata reports CC BY 4.0 — unresolved.
- PEVA: the README says weights are released under CC BY-NC 4.0 but no download link was found.
- GRASP and Causal-JEPA: ICML 2026 presentation type (poster/oral) not verified; C-JEPA code repo has no declared license.
- Temporal Straightening: the code repo documents a post-publication train/val split bug and re-run numbers; the authors state results reproduce, but the published tables may differ slightly from the current code.
- Sensorimotor World Models: the NeurIPS 2026 title differs from arXiv; matched by authors and topic.
- Possible misses: CoRL/ICRA/RSS/AAAI/IJCAI 2025–26 were checked only by web search, not by full proceedings scans.
