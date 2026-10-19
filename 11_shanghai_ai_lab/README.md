# Shanghai AI Laboratory — Physical Intelligence Team

- **Who the group is:** "Physical Intelligence Team, Shanghai AI Laboratory" is a 2026 team byline. It is not a classic PI-led lab, and it has no lab page listing members. Its outputs appear under the InternRobotics GitHub organization (https://github.com/InternRobotics). Papers under this byline so far:
  - **A Definition and Roadmap for World Models** (arXiv 2607.06401, July 2026). Contributors listed alphabetically: Xinyuan Chen, Haoyu Guo, Shi Guo, Bingqi Jiang, Chunhua Shen, Xing Shen, Tianfan Xue, Yufei Xue, Mulin Yu, Weinan Zhang, Bin Zhao, Bowen Zhou, Ming Zhou.
  - **InternW0** (arXiv 2609.27656, Sept 2026, 26 authors).
  - **InternW0-Δ** (arXiv 2609.31394, Sept 2026, 48 authors).
  - **GAUGE** (arXiv 2608.05948, Aug 2026).
  - A forthcoming monograph, *Physical AI: Concepts, Techniques, and Challenges* (https://internrobotics.github.io/PhysicalAI/).
- **Senior authors:** the recurring senior block is Bowen Zhou (Shanghai AI Lab director / chief scientist), Bin Zhao, Tianfan Xue, Weinan Zhang and Chunhua Shen. Ming Zhou, Yufei Xue, Haoyu Guo, Bingqi Jiang and Mulin Yu recur as members.
- **Predecessor:** Shanghai AI Lab's earlier **World Model team**, led by Tong He (with Jiangmiao Pang, under Yu Qiao). Tong He has since moved to JD Future Academy (per his homepage). Several of its members (Zizun Li, Jianjun Zhou, Haoyu Guo, Xinyue Li, Yating Wang) are now on Physical Intelligence Team papers.
- **Line:** a definitional/roadmap view of world models as compression of physical state transitions. Concrete work is on geometry-aware 4D video world models (Aether, DeepVerse), 4D world-modelling data (OmniWorld), and 2026 world-action models for robot manipulation with force/tactile input (InternW0, InternW0-Δ).

## Scope decision
Included: top-venue world-model papers whose senior affiliation is Shanghai AI Lab, and whose authors are on Physical Intelligence Team papers or come from its direct predecessor (Tong He's World Model team, whose staff and repos moved into InternRobotics). Both included papers predate the "Physical Intelligence Team" name. If a strict reading is wanted (only papers under that byline), **this team has no top-venue world-model paper yet**: everything under the byline is an arXiv technical report.

## Included papers

| Paper | Venue | Year | Citations (Semantic Scholar, 2026-10-15) |
|---|---|---|---|
| [Aether: Geometric-Aware Unified World Modeling](aether.md) | ICCV (+ ICCV RIWM workshop Outstanding Paper) | 2025 | 105 |
| [OmniWorld: A Multi-Domain and Multi-Modal Dataset for 4D World Modeling](omniworld.md) | ICLR (poster) | 2026 | 61 |

Tong He is last and corresponding author on both. Chunhua Shen and Jiangmiao Pang are co-authors on both.

## Cross-group papers
- **OpenDriveLab papers (GenAD CVPR 2024, Vista NeurIPS 2024, ReSim NeurIPS 2025)** have Shanghai AI Lab affiliations, but they belong to Hongyang Li's group (section 10, `10_*`). Not duplicated here.
- No other overlap with the summary's groups found.

## Excluded items
- **A Definition and Roadmap for World Models** (arXiv 2607.06401, v1 2026-07-07; the only item in the summary). arXiv comment: "Technical report, 58 pages, 10 figures". No venue acceptance found on arXiv, Semantic Scholar (venue "arXiv.org", 3 citations), the authors' pages or web search. **Excluded as arXiv-only.** For reference:
  - It defines a world model as "a compression modeling of the state transition processes of the physical world" under finite compute. The stated key properties are omnimodality, multidimensional asynchrony and locality (POMDP).
  - It adds a two-dimensional (function × architecture) taxonomy on top of Fei-Fei Li's renderer/simulator/planner split.
  - Its three-stage roadmap: unified multimodal world models, then a unified physical representation ("one state, many decoders"), then foundation-scale interactive simulators. It closes with a "Trinity" architecture (Agent, Evaluator, World Model as curriculum designer) toward "Physical AGI".
  - It is a perspective with no experiments. The 36kr claim that "systems are at L1-L2" comes from a different (NTU) paper, not this one.
- **InternW0: A Foundational Physical World Model for Efficient Real-World Interactions** (arXiv 2609.27656, 2026-09-23; "A technical report of world models"). Video expert plus a fast action expert with force/tactile input. Reported results: LIBERO 98.6%, RoboTwin 2.0 93.12%, five real-world lab tasks. arXiv-only. Project https://internrobotics.github.io/internw0.
- **InternW0-Δ** (arXiv 2609.31394, 2026-09-25). World-action MoT with "Causal Imprint" and 4D distillation, trained on 20K+ hours of open data. Code at https://github.com/InternRobotics/InternW0-Delta (Hugging Face weights "coming soon"). arXiv-only.
- **GAUGE: A Measurement-Grounded Benchmark for Physical Fidelity in Simulation Engines and Video World Models** (arXiv 2608.05948, 2026-08; seniors Shen, Zhang). It finds that video models often get the right trajectory form but wrong accelerations and momentum. arXiv-only.
- **DeepVerse: 4D Autoregressive Video Generation as a World Model** (arXiv 2506.01103; Tong He last). Follow-up to Aether. No venue found (cited as an arXiv preprint in 2026 papers; listed as arXiv on Tong He's page).
- **Sekai: A Video Dataset towards World Exploration** (ICLR 2026 per Tong He's page) and **Yume / Yume1.5** interactive world generation (arXiv). The senior author is Kaipeng Zhang, of a different Shanghai AI Lab team (He and Pang are middle authors). Excluded as outside this team; Sekai's venue was not independently verified.
- **World Model-based Perception for Visual Legged Locomotion (WMP)** (ICRA 2025; Weinan Zhang last). A top-venue world-model paper, but its affiliations are SJTU and ByteDance, not Shanghai AI Lab, and it predates the team. Excluded.
- **WebWorld** (EMNLP 2026 main; Ming Zhou last): not one of the target venues, and a "browser as world model" for code, not a physical world model. **MARIE** multi-agent world models (TMLR; Bin Zhao co-author, Xuelong Li last): not a target venue and not team-senior. **M3PC** (ICLR 2025; Yao Mu co-author, not senior).
- **Seer** (ICLR 2025 oral), **F1-VLA**, **InternVLA-A1 / A1.5** (InternRobotics): VLA policies with visual-foresight objectives, not world models. Seer is the only one of them with a top venue, and its senior author is Jiangmiao Pang, not a team member.
- **π3** (ICLR 2026; Shen co-author, He last): visual geometry, not a world model.

## Answers to the brief
- Identity: the 2607.06401 PDF has no author block on the title page. It credits "Physical Intelligence Team, Shanghai AI Laboratory" and lists the 13 contributors alphabetically at the end, so no single PI can be named. The InternW0 papers suggest the senior leads are Bowen Zhou, Bin Zhao, Tianfan Xue, Weinan Zhang and Chunhua Shen.
- The summary's item (2607.06401) is arXiv-only. Excluded.
- Top-venue world-model papers under the team byline itself: none. With the predecessor World Model team counted, there are two (Aether, ICCV 2025; OmniWorld, ICLR 2026).

## Method
- Sources: arXiv abstract/HTML/PDF of 2607.06401 (full text extracted, including the contributors section and reference list, checked for self-citations); arXiv pages of InternW0, InternW0-Δ, GAUGE, AHA-WAM, DeepVerse and OmniWorld.
- arXiv website search for "world model" with each of the 13 contributors plus Yao Mu, Tong He and Jiangmiao Pang.
- The InternRobotics GitHub organization (all 100 repos listed via the API) and the READMEs of the PhysicalAI, InternW0-Delta, GAUGE, internw0, Aether, F1-VLA and InternVLA-A-series repos.
- Tong He's homepage; CVF ICCV 2025 open-access listing; ICLR 2026 virtual page and proceedings; Hugging Face APIs; Semantic Scholar API (citation counts); web searches in English and Chinese.
- Blocked: DBLP (bot check), arXiv export API (no results returned). Semantic Scholar worked after retries.

## Open issues
- Team membership is inferred from author lists. No official roster exists (the monograph site says author profiles "will be added").
- Counting Aether and OmniWorld depends on treating Tong He's World Model team as the predecessor. If the caller prefers the strict reading, move both to "excluded" and the group has no included papers.
- Possibly missed: 2025-26 top-venue papers by team members that do not use "world model" in the abstract (for example, papers from Tianfan Xue's or Bin Zhao's other affiliations). DBLP could not be used to check them.
- Citation counts come only from Semantic Scholar. Google Scholar was not checked.
