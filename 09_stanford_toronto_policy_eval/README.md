# Group 9: Stanford (Percy Liang, Sherry Yang, Chelsea Finn) and University of Toronto: world models as policy evaluators

## Group
- **PIs:** Chelsea Finn (Stanford IRIS lab), Percy Liang (Stanford CRFM), Sherry Yang (NYU Courant / Google DeepMind; Stanford affiliation on WorldGym), Yichen Zhu (listed with a University of Toronto affiliation on dWorldEval; lead institution Current Robotics).
- **Lab pages:** IRIS lab https://irislab.stanford.edu/publications.html · WorldGym https://world-model-eval.github.io · Ctrl-World https://ctrl-world.github.io · dWorldEval https://dworldeval.github.io/
- **Line of work:** action-conditioned video world models used to evaluate and improve robot policies (WorldGym, Ctrl-World, VLAW, dWorldEval), plus Finn's longer record on visual foresight, video prediction and latent world models for planning and model-based RL.

## Included papers (17)

| Title | Venue | Year | Citations |
|---|---|---|---|
| [WorldGym: World Model as An Environment for Policy Evaluation](worldgym.md) | ICLR (poster) | 2026 | 65 |
| [Ctrl-World: A Controllable Generative World Model for Robot Manipulation](ctrl-world.md) | ICLR (poster) | 2026 | 196 |
| [VLAW: Iterative Co-Improvement of Vision-Language-Action Policy and World Model](vlaw.md) | ICML (poster) | 2026 | 46 |
| [dWorldEval: Scalable Robotic Policy Evaluation via Discrete Diffusion World Model](dworldeval.md) | ICML (poster; spotlight per project page) | 2026 | 11 |
| [Latent Diffusion Planning for Imitation Learning](latent-diffusion-planning.md) | ICML (spotlight poster) | 2025 | 30 |
| [MOTO: Offline Pre-training to Online Fine-tuning for Model-based Robot Learning](moto.md) | CoRL | 2023 | 20 |
| [COMBO: Conservative Offline Model-Based Policy Optimization](combo.md) | NeurIPS | 2021 | 554 |
| [Visual Adversarial Imitation Learning using Variational Models (V-MAIL)](v-mail.md) | NeurIPS | 2021 | 61 |
| [Example-Driven Model-Based RL for Long-Horizon Visuomotor Tasks (EMBER)](ember.md) | CoRL | 2021 | 26 |
| [Greedy Hierarchical VAEs for Large-Scale Video Prediction (GHVAE)](ghvae.md) | CVPR | 2021 | 122 |
| [Learning Predictive Models From Observation and Interaction](predictive-models-observation-interaction.md) | ECCV | 2020 | 67 |
| [Goal-Aware Prediction: Learning to Model What Matters](goal-aware-prediction.md) | ICML | 2020 | 76 |
| [Hierarchical Foresight (HVF)](hierarchical-visual-foresight.md) | ICLR | 2020 | 161 |
| [RoboNet: Large-Scale Multi-Robot Learning](robonet.md) | CoRL | 2019 | 465 |
| [Improvisation through Physical Understanding: Novel Objects as Tools with Visual Foresight](guided-visual-foresight-tool-use.md) | RSS | 2019 | 98 |
| [Robustness via Retrying](robustness-via-retrying.md) | CoRL | 2018 | 73 |
| [Universal Planning Networks](universal-planning-networks.md) | ICML | 2018 | 147 |

Citation counts: Semantic Scholar API, checked 2026-10-15.

### Ownership decisions
- **Ctrl-World and VLAW (Stanford + Tsinghua):** last author is Chelsea Finn; the Tsinghua co-author is Jianyu Chen (IIIS), who is not a PI of group 06 (Mingsheng Long, Jun Zhu, Chongxuan Li). Both papers belong here, not in 06_tsinghua_long_zhu.
- **WorldGym:** last and corresponding author Sherry Yang, with Percy Liang second-to-last; owned here.
- **dWorldEval:** last author Yichen Zhu (University of Toronto affiliation); owned here per the group definition. The ICML 2026 author list differs from arXiv (adds Junjie Wen, Chaomin Shen, Yaxin Peng; drops Yaokai Xue); Yichen Zhu is last in both.
- **Older Finn papers:** included only where Finn is last author and no PI of another summary group is last or corresponding. Co-advisors Fei-Fei Li (GHVAE, EMBER) and Dorsa Sadigh (LDP) are not summary PIs.

## Cross-group papers (not written here)
- **A Control-Centric Benchmark for Video Prediction (VP2)** — Stephen Tian, Chelsea Finn, Jiajun Wu; ICLR 2023. Last author Jiajun Wu: belongs to **07_stanford_wu**.
- **Reasoning About Physical Interactions with Object-Oriented Prediction and Planning (O2P2)** — Janner, Levine, Freeman, Tenenbaum, Finn, Wu; ICLR 2019. Last author Jiajun Wu: belongs to **07_stanford_wu**.

## Excluded items
| Item | Reason |
|---|---|
| GigaWorld-1: A Roadmap to Build World Models for Robot Policy Evaluation (arXiv 2607.02642; introduces WMBench) | Not this group (GigaAI / Tsinghua, GigaWorld Team; last author Zheng Zhu) and arXiv-only. The summary file mislabels it as "WMBench". |
| WorldEval: World Model as Real-World Robot Policies Evaluator (arXiv 2505.19017) | Preprint only; last author Yi Xu, Yichen Zhu is second author. |
| World-Gymnast (arXiv 2602.02454; Sherry Yang last) | Workshop only (ICLR 2026 2nd Workshop on World Models). |
| VideoAgent: Self-Improving Video Generation (arXiv 2410.10076; Sherry Yang last, Liang co-author) | Only a workshop spotlight (NeurIPS 2025 LAW workshop); no main-track acceptance found. |
| Learning Interactive Real-World Simulators (UniSim, ICLR 2024 oral) | Sherry Yang is first author; last author Pieter Abbeel (not a summary PI). Fails the last/corresponding-author rule as far as verified. |
| Cosmos Policy (ICLR 2026; Liang and Finn co-authors) | Last author Jinwei Gu (NVIDIA); not a group PI. |
| CoT-VLA (CVPR 2025; Finn co-author) | Last author Tsung-Yi Lin (NVIDIA); also a VLA rather than a world model. |
| Q-Learning With World Models; World Action Verifier (2026, Finn co-author) | arXiv only (IRIS listing); World Action Verifier's last author is Yilun Du. |
| Evaluating Gemini Robotics Policies in a Veo World Simulator (Sherry Yang co-author) | Industry report, not a top-venue paper; Gemini Robotics Team. |
| Efficient Imitation Learning with Conservative World Models (L4DC 2024); Offline RL from Images with Latent Space Models (LOMPO, L4DC 2021) | Finn last author but L4DC is not a listed top venue. |
| Few-Shot Goal Inference for Visuomotor Learning and Planning (CoRL 2018; Finn last) | Contribution is a meta-learned goal classifier (reward); the video model is an off-the-shelf component in one experiment. Judged out of scope. |
| Unsupervised Visuomotor Control via Distributional Planning Networks (RSS 2019; Finn last) | Learns a distance metric, not a predictive world model. |
| Finn co-authored world-model papers with Sergey Levine or others as last author: Unsupervised Learning for Physical Interaction through Video Prediction (NeurIPS 2016), Deep Visual Foresight (ICRA 2017), SNA / Self-Supervised Visual Planning (CoRL 2017), SV2P (ICLR 2018), Learning to Adapt / Deep Online Learning via Meta-Learning for MBRL (ICLR 2019), Manipulation by Feel (ICRA 2019), Entity Abstraction in Visual MBRL (CoRL 2019), SimPLe / MBRL for Atari (ICLR 2020), VideoFlow (ICLR 2020), MOPO (NeurIPS 2020, last author Tengyu Ma), Long-Horizon Visual Planning with GCP (NeurIPS 2020), Latent State Models for Meta-RL from Images (CoRL 2020), Model-Based Visual Planning with Functional Distances (ICLR 2021) | Senior author is not a group PI (mostly Levine, who is not a PI of any summary group). |
| Percy Liang: no top-venue world-model paper with Liang as last author found | He appears as second-to-last on WorldGym and VLAW (both included). |

## Method
- Summary file section 9 read first; every claim re-checked.
- Papers: arXiv PDFs downloaded and read (main text, key tables, selected appendices) for all 17 included papers.
- Venues: ICLR/ICML virtual pages, PMLR, NeurIPS proceedings, ECVA, CVF, mlanthology, arXiv comment fields, Finn's CV (https://ai.stanford.edu/~cbfinn/_files/cv.pdf, publications through early 2025), Stanford IRIS publications page (2024-2026).
- Author searches: Finn CV and IRIS listing; arXiv API; web search for Sherry Yang, Percy Liang and Yichen Zhu. DBLP was blocked (bot check) and not used.
- Code and weights: GitHub API, repository READMEs, project pages.
- Citation counts: Semantic Scholar Graph API (batch endpoint), checked 2026-10-15.

## Open issues
- **Summary file errors:** (1) arXiv 2506.00613 is WorldGym itself; "WPE / Evaluating Robot Policies in a World Model" was its earlier title, so WPE and WorldGym are one paper. (2) arXiv 2607.02642 is GigaWorld-1 (GigaAI/Tsinghua), not a paper called WMBench; WMBench is the benchmark inside it, and it does not belong to this group. (3) The claim that WPE "underestimates in-distribution and overestimates out-of-distribution values" is not in WorldGym v3 (probably from v1; not verified). (4) The summary lists dWorldEval without a venue; it is ICML 2026.
- **dWorldEval presentation type:** ICML poster page says poster; the project page and an ICML spotlight page say spotlight. Not resolved.
- **Yichen Zhu's role at the University of Toronto** (faculty or other) was not verified; the Toronto PI was taken from the paper's affiliation line as instructed.
- **Sherry Yang's corresponding-author status on UniSim** not verified; if she was corresponding author it would qualify under the ownership rule.
- **Finn's full 2025-2026 record:** the CV covers up to early 2025; later items were taken from the IRIS lab page and searches. NeurIPS 2026 / CoRL 2026 world-model acceptances could be missing.
- **Scope judgement calls:** COMBO (mostly state-space dynamics), LDP (action-free latent forecaster), UPN (latent model trained for plannability) and RoboNet (dataset paper) are borderline world-model papers and are flagged as such in their files.
- Exact ICLR/ICML camera-ready dates not checked; conference months used.
